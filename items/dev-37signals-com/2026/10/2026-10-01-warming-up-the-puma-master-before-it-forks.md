---
title: Warming up the Puma master before it forks
link: https://dev.37signals.com/warming-up-the-puma-master-before-it-forks/
source: dev-37signals-com
published: 2026-10-01T17:00:00Z
updated: 2026-10-01T17:00:00Z
first_seen: 2026-10-02T15:44:26.851147349Z
authors:
- Lewis Buckley
summary: How we cut Basecamp 5’s post-deploy request queues by running signed-in requests through the Rails app in the Puma master, before it forks its workers.
content: extracted
html: 2026-10-01-warming-up-the-puma-master-before-it-forks.html
preview:
  file: 2026-10-01-warming-up-the-puma-master-before-it-forks.preview-3faba46664a7.webp
  width: 256
  height: 134
  alt: 37signals Dev
  color: '#101010'
images:
- source: https://dev.37signals.com/assets/images/opengraph/warming-up-the-puma-master-before-it-forks.png
  original:
    file: 2026-10-01-warming-up-the-puma-master-before-it-forks.image-ab51b20d378f.png
    width: 2400
    height: 1260
  variants:
  - file: 2026-10-01-warming-up-the-puma-master-before-it-forks.image-7aa86de534f8.webp
    width: 320
    height: 168
  - file: 2026-10-01-warming-up-the-puma-master-before-it-forks.image-b8b68aa56051.webp
    width: 640
    height: 336
  - file: 2026-10-01-warming-up-the-puma-master-before-it-forks.image-a91089bb4f33.webp
    width: 960
    height: 504
  - file: 2026-10-01-warming-up-the-puma-master-before-it-forks.image-267916de3c8f.webp
    width: 1280
    height: 672
  - file: 2026-10-01-warming-up-the-puma-master-before-it-forks.image-8df75b19ef58.webp
    width: 1600
    height: 840
  - file: 2026-10-01-warming-up-the-puma-master-before-it-forks.image-a906e699146e.webp
    width: 2400
    height: 1260
  color: '#000000'
extra:
  thumbnail: https://dev.37signals.com/assets/images/opengraph/warming-up-the-puma-master-before-it-forks.png
---

[Basecamp 5](https://basecamp.com) runs on [Puma](https://github.com/puma/puma) in cluster mode: one master process with `preload_app!` and 63 single-threaded workers per host, deployed as a Docker container with [Kamal](https://dev.37signals.com/kamal-2/).

We serve Basecamp from several sites. Each site has its own web hosts and a read replica of the database, and writes go to a single primary database in one of them.

On our busiest hosts, each deploy left up to 2,000 requests waiting while the new workers warmed up. We reduced those queues by running signed-in requests through the app in the Puma master, before it forked the workers.

## Why 63 single-threaded workers?

Basecamp has always served web requests from processes rather than threads. It ran on Unicorn, which only does processes, until we moved to Puma in January 2025, and we kept the same setup:

```ruby
workers (Concurrent.physical_processor_count * 1.3).ceil
threads 1, 1
preload_app!
```

On a 48-core host that’s 63 workers, each handling one request at a time.

We chose 1.3 after benchmarking HEY in 2023, when we [moved our apps out of the cloud](https://dev.37signals.com/bringing-our-apps-back-home/) and onto our own hardware. We tested several combinations of workers and threads with a mix of GET and POST requests on a 32-vCPU VM. Every multithreaded configuration we tested was slower and handled fewer requests than single-threaded workers. Adding workers beyond about 1.2 to 1.3 per vCPU brought little benefit.

The threaded workers spent a lot of their time waiting for Ruby’s global VM lock. That made single-threaded workers a good fit for this workload, and we use the same setup for Basecamp. An app that spends more time waiting on its database or other services may benefit from more threads, so benchmark your own app.

The other reason is the app itself. Basecamp has class-level state in places and has never needed to be thread-safe. With one request per process, it still doesn’t.

Processes do use more memory than threads, and `preload_app!` reduces the difference. The master loads the app once and the workers share its memory through copy-on-write until they write to it. Shopify’s [comparison of Ruby execution models](https://shopify.engineering/ruby-execution-models) explains the trade-off well. In the HEY benchmark the best setup came to about 260 MB of PSS per core, where PSS counts each shared page once, split between the processes using it, and the gap to a threaded setup was smaller than we’d expected.

Two things about this setup matter for the rest of the post. A worker that’s compiling or loading something is fully blocked — there’s no other thread to pick up the next request. And whatever the master has in memory before it forks, all 63 workers share. Whatever they build after the fork, they build 63 times.

## What happens when we deploy

Kamal starts the new container alongside the old one, and [kamal-proxy](https://github.com/basecamp/kamal-proxy) moves the host’s traffic across as soon as the health check passes. At that moment, the new workers have handled health checks but no customer requests.

`preload_app!` means the master loads the app once and the workers inherit it through `fork`. That covers the code. It doesn’t cover anything Ruby and Rails set up on first use:

- **YJIT compiled code.** [YJIT](https://dev.37signals.com/yjit-is-fast/) compiles a method once it’s been called a certain number of times. The master calls very little during boot, so every worker compiles the same methods again on its own first requests.
- **Compiled templates.** Action View turns each ERB template into a Ruby method the first time it’s rendered.
- **The schema cache.** Active Record reads each model’s columns from the database the first time that model is used.
- **Inline caches and memoized values** throughout Ruby, Rails and the app.

All 63 workers did all of this at once, while serving the traffic the old container had been handling a second earlier. In the test environment with YJIT on, the first request to a project page on a cold process took 652 ms, 151 ms of it YJIT compiling. The same request to a warm process took 28 ms. In production, CPU time per request peaked at around 200 ms while kamal-proxy moved traffic to the new container, against about 30 ms once the workers had warmed up.

A host with spare CPU absorbs this. Every one of our web hosts has 48 cores and 63 workers, but each Amsterdam host serves around 250 requests per second, against 25 to 60 at our other sites. In Amsterdam the slow first requests turned into a queue. At a peak-hour deploy, the Puma backlog on an Amsterdam host reached anywhere from 250 to 2,238 requests, and kamal-proxy’s p99 response time hit about 10 seconds.

[Eron](https://dev.37signals.com/author/eron/), our Director of Operations, had been tracking this since June. Another server in Amsterdam would help, but it would take weeks to arrive, so we also wanted to make deploys cheaper on the hardware we already had.

## What didn’t work

We tried a few things first. In June, [Donal](https://dev.37signals.com/author/donal/) tested the first two on a single Amsterdam host, comparing it with its neighbors, and they ruled out two likely causes.

**Warming each worker’s database connections.** Puma’s `before_fork` hook clears the master’s connections, and each worker opened its own on its first request. Opening them in `before_worker_boot` instead made no difference. Queries on a freshly booted production host were already under a millisecond, so connections weren’t the problem.

**A synthetic request in each worker.** Next, each worker made a few requests in `before_worker_boot` to an internal controller that touched every model. That ran the middleware, routing and Active Record paths, but it ran them in 63 workers at once — exactly the CPU spike we were trying to avoid. And a request with no real data renders no real views, so most of the app stayed cold.

**Spreading YJIT compilation out.** Delaying YJIT in each worker by a random interval spread the compiling out over a few minutes, but every worker still ran interpreted until its delay ended. The queue didn’t change.

**Reforking from a warm worker.** This is what Shopify’s [Pitchfork](https://github.com/Shopify/pitchfork) does: let one worker serve traffic until it’s warm, then fork the others from it. Puma has an experimental version called `fork_worker`, and on beta it worked — the reforked workers were warm after three to five requests, where fresh ones took up to 30 seconds. But with `fork_worker` the template is worker 0, and it keeps serving requests. If it exits, the workers waiting to be forked never start ([puma/puma#3596](https://github.com/puma/puma/issues/3596)). If it gets no traffic, the refork never happens, which is what we saw on beta. Instacart have a `mold_worker` patch that promotes a warm worker to a template that stops serving, but it isn’t in a Puma release. We have a branch of it, and we may come back to it.

That last experiment did show us where the fix was, though. Everything a warm worker has that a cold one lacks is in its memory, and `fork` copies memory. The master already has the app loaded. It just never runs it.

## Run the requests in the master

So now, before the master binds its socket and forks, it makes the app’s own requests, in-process, the way a signed-in user would.

Rack has a hook for exactly this. [`Rack::Builder#warmup`](https://rubydoc.info/gems/rack/Rack/Builder#warmup-instance_method) takes a block that’s called once with the built app, before the server starts. `rails server` builds the app from `config.ru`, so the change to boot is one line:

```ruby
require_relative "config/environment"

warmup { WarmUp.configured.run } if ENV["WARM_UP"]

run Rails.application
```

With `preload_app!` this runs in the master, and the workers inherit whatever it did. Puma binds its socket after the app is built, so until the warm-up finishes the health check’s connection is refused and kamal-proxy keeps retrying. No request reaches a worker that hasn’t been warmed.

The warm-up has three steps. After precompiling the views, it gives the page requests and schema loading a shared 20-second budget, checked before each page or model.

### 1\. Precompile the views

[`actionview_precompiler`](https://github.com/jhawthorn/actionview_precompiler) reads every template for its `render` calls and compiles each one with the locals it’s passed. For us that’s 1,394 templates in about two seconds. A first request to a project page then compiles 2 templates instead of 44.

### 2\. Request the pages, signed in

A small browser class makes the requests through `Rack::MockRequest`, with the two cookies a real sign-in sets, then goes back for each page’s lazy Turbo frames:

```ruby
class WarmUp::Browser
  def initialize(signed_in_as:)
    @client = Rack::MockRequest.new(Rails.application)
    @headers = {
      "HTTP_USER_AGENT" => "Basecamp warm-up",
      "HTTP_COOKIE" => cookie_for(signed_in_as),
      "bc3.warm_up" => true
    }
  end

  def visit(path)
    page = get(path)
    frames_in(page).each { |id, src| get(src, "HTTP_TURBO_FRAME" => id) }
  end

  private
    def get(path, headers = {})
      @client.get("https://#{host}#{path}", @headers.merge(headers))
    end

    def frames_in(page)
      Nokogiri::HTML5(page.body).css("turbo-frame[src]").map { |frame| [ frame["id"], frame["src"] ] }
    end
end
```

**The requests are signed in.** The user is a monitoring account we already use for automated checks, and the pages are its own project, Campfire, to-dos, documents and messages. Public pages weren’t enough: after warming up with signed-out pages only, the first signed-in request to the projects page still took 131 ms, because authentication, the signed-in controllers and their views had never run. With signed-in pages it took 40 ms. `cookie_for` writes the same signed cookie the sign-in controller does, using the app’s own cookie jar, so there’s no API token and no secret to store.

**The frames are followed.** The busiest HTML requests in production aren’t pages at all but Turbo frames — the sidebar badge, the inbox, the navigation menus. The browser parses each page and requests its `<turbo-frame src>` URLs with the `Turbo-Frame` header, so those controllers and views get warmed too. Our first four pages turned into 60 requests.

**The requests are excluded from rate limiting.** They are internal, so they do not count against the rate limits that apply to real visitors.

### 3\. Load the rest of the schema

The page requests load the schema for the models they touch. The last step loads the rest, from the read replica:

```ruby
ApplicationRecord.reading do
  models.lazy.take_while { time_left? }.each { |model| model.load_schema if model.table_exists? }
end
```

The step checks 261 models and loads any schema information still missing. Those database round trips add up when the primary is far away: outside a request, Active Record uses the writing role, and from a host a long way from the primary each round trip is tens of milliseconds. Reading from the local replica brings the step down from about 20 seconds to 3.5. The pages go first because they load most of the schema anyway. If the time budget runs out, the step stops, logs how many models it got through, and the workers load the rest on first use like they always did.

Rails can also load the schema from a dumped cache file at boot (`bin/rails db:schema:cache:dump`), which would make this step unnecessary. We don’t ship one in our image yet, because the dump needs a database to read from at build time, and we have several databases to cover. It’s on the list.

## What to close before the fork

Running requests in the master opens things the master never opened before, and every worker inherits them. Two processes writing to the same socket will corrupt each other’s traffic, so you need to know what’s open before you fork.

The way to find out is to list the master’s open file descriptors — `ls -l /proc/<pid>/fd` — before and after a warm-up, in an environment set up like production. Development wasn’t enough for us: it stores files on disk, so our S3 connections only showed up in production. Then, for each thing that’s open, check how its library handles a fork. We found three kinds:

- **Already handled.** Plenty of libraries detect a fork on their own, either by recording the PID they connected from and reconnecting in the child, by opening per-process files, or by resetting their thread pools. Redis clients, metrics libraries and concurrency libraries tend to be in this group. Check, but you probably don’t need to do anything.
- **Already closed.** Database connections are the classic one, and most Puma configs already clear them in `before_fork`. Anything else that’s opened per process — we have a SQLite cache the workers open on boot — needs closing when the warm-up finishes.
- **Needs a new step.** HTTP clients with keep-alive connections are the ones to look for: cloud SDKs with connection pools, tracing exporters, error reporters. They usually have no fork handling at all. We empty the aws-sdk connection pools in `before_fork`, and we run the warm-up untraced so the OpenTelemetry exporter never opens its connection to Tempo in the first place.

Once that’s done, `before_fork` finishes with `Process.warmup`, which Ruby 3.3 added for this purpose: a major GC, a heap compaction, and every surviving object promoted to the old generation, so the memory pages the workers share change as little as possible afterwards.

## Choosing the pages

The first list was the four pages that ran the busiest requests on beta. Once the warm-up was live, production showed us which endpoints were still cold. For one deploy, we compared each endpoint’s mean duration in the six minutes after kamal-proxy moved traffic to the new container with the same endpoint an hour later, then multiplied the difference by the number of requests in those six minutes. That gives the extra time each endpoint cost us because it was cold:

| Endpoint            | Cold   | Warm   | Requests in 6 min | Extra seconds |
| ------------------- | ------ | ------ | ----------------- | ------------- |
| Campfire            | 246 ms | 70 ms  | 6,490             | 1,140         |
| Projects (JSON API) | 84 ms  | 50 ms  | 22,077            | 771           |
| Docs & Files        | 262 ms | 177 ms | 4,996             | 421           |
| To-dos tool         | 205 ms | 113 ms | 4,018             | 371           |
| To-dos (JSON API)   | 33 ms  | 16 ms  | 18,738            | 320           |

The pages already in the warm-up showed what to expect: the project page kept a 36 ms gap after a deploy, and the to-do page 10 ms. We’ve proposed adding these five requests, and expect them to add about five to seven seconds to the page step. The two JSON endpoints were a surprise. The warm-up’s page list had no API requests in it, so nothing on the API path had run before the first real request: not the API controllers, and not the Jbuilder templates rendering real records. Precompiling the views covers JSON templates too, but it isn’t a substitute for running the request.

## Results

The warm-up is on for all 68 web hosts. With the first four pages it took 12 to 16 seconds per host: about 2 seconds to precompile the views, 7 to 9 for the 60 requests, and 3.5 for the schema. Deploys take that much longer per host, and we raised the deploy timeout from 30 to 60 seconds to cover it.

In Amsterdam, at a peak-hour deploy:

| During deploy                  | Before             | After           |
| ------------------------------ | ------------------ | --------------- |
| Peak Puma backlog per host     | 250–2,238 requests | 19–223 requests |
| Peak kamal-proxy p99           | about 10 s         | 2.4–4.8 s       |
| Peak CPU time per request      | 201–214 ms         | 88–132 ms       |
| Peak database time per request | 56–69 ms           | 39–47 ms        |

The deploy in the middle, with four hosts warmed and four not, shows why every host needed the warm-up. Each warmed host recovered faster on its own: mean request duration peaked at 130 to 173 ms, against 203 to 311 ms on the hosts that weren’t warmed. But the backlog on all eight was about the same, because they were all waiting on the same database.

Memory came down too. The workers now share compiled templates, YJIT code and the schema with the master instead of each building their own copy. On beta, the view precompiler alone took a busy worker’s private memory from 174–202 MB to 119–135 MB. Thirty minutes after the deploy, the web containers used about 39 GB less memory than the previous day’s containers at the same age and traffic. Amsterdam served most of our traffic at the times we tested. In Amsterdam, each new container used about 2 GB less just after traffic moved to it, which lowers the peak while the old and new containers overlap.

## Working with Claude

Claude Code helped throughout. It combed through the per-worker backlogs and per-endpoint timings in [Prometheus](https://dev.37signals.com/kamal-prometheus/) and Loki after each deploy, worked out the cold-versus-warm cost of each endpoint, and prepared the changes and the pull request descriptions with the benchmarks in them. We decided what to try, deployed it and read the results.

## If you do this

**Warm the master before it forks.** Compile common code and templates and load their schema in the master, so workers inherit that work. With `preload_app!`, `Rack::Builder#warmup` runs before the workers start accepting traffic.

**Use the app’s real requests.** Public pages, internal endpoints and synthetic queries warm the paths they run and nothing else. Signed-in requests to real records, frames included, run what production runs.

**Measure the cold penalty per endpoint.** The difference between an endpoint’s cold and warm duration, times its request count after a deploy, ranks the pages worth adding. Ours weren’t the ones we’d have guessed, and two of them were JSON.

**Check what the warm-up leaves open.** List the master’s file descriptors after a warm-up and account for every one before the fork. Two of ours needed changes.

**Set a time budget.** A warm-up that runs long on one slow host fails the deploy on that host. Ours gives the page requests and schema loading a shared 20-second budget, checked before each page or model, puts the most valuable pages first, and logs what it skipped.

Reforking from a warm worker, as Pitchfork does, solves the same problem continuously rather than once at boot, and it would warm paths no fixed list of pages covers. We may still get there: [our branch](https://github.com/basecamp/puma/pull/1) brings Instacart’s `mold_worker` up to date with Puma’s main branch and fixes the bugs we found in it. But warming the master works with the Puma we already run, took a few days to implement, and substantially reduced the queues after deployment.
