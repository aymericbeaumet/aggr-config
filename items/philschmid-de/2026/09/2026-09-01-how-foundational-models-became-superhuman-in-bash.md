---
title: How Foundational Models Became Superhuman in Bash
link: https://www.philschmid.de/superhuman-bash
source: philschmid-de
published: 2026-09-01T00:00:00Z
updated: 2026-09-01T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Frontier coding agents can compose shell workflows that replace entire catalogs of file tools. Here is what changes when Bash becomes the primary execution interface.
content: extracted
html: 2026-09-01-how-foundational-models-became-superhuman-in-bash.html
preview:
  file: 2026-09-01-how-foundational-models-became-superhuman-in-bash.preview-089014688bf0.webp
  width: 256
  height: 134
  alt: How Foundational Models Became Superhuman in Bash
  color: '#191a1d'
images:
- source: https://www.philschmid.de/static/blog/superhuman-bash/thumbnail.jpg
  original:
    file: 2026-09-01-how-foundational-models-became-superhuman-in-bash.image-37b538232e4a.jpg
    width: 1500
    height: 785
  color: '#0b0c10'
---

If you have used coding agents, you may have noticed that they started to use Bash for tasks that would have traditionally used dedicated tools such as `read`, `edit`, and `search`. Well, models became superhuman in bash and might consider dedicated tools not good enough.

Every few months, I rebuild an [agent harness](https://www.philschmid.de/agent-harness-2026) from scratch to learn what changed and what I can delete. This time I removed every model-facing tool except for a `bash` and a `media` viewer.

The outcome matched what I had already seen in daily use. Task completion stayed in the same range, while the agent moved through complex work and verifications with fewer tool boundaries.

Frontier models have become superhuman in Bash. I mean that in a narrow sense: they synthesize disposable command-line programs, in seconds, that most developers would need much longer to assemble and verify.

## What “superhuman in Bash” means

You probably know the individual pieces `git`, `rg`, `jq`, Python, temporary files, process substitution, and test runners. Writing a one-off 40-line workflow that combines them under time pressure is different. Most of us work through the problem interactively, checking syntax and state between steps.

Coding agents assemble these workflows from patterns learned across POSIX utilities, programming languages, build systems, and public source code. They still make mistakes. The difference is how much correct orchestration they can attempt at once.

Bash is the routing layer here. The heavy work may happen in Python, Git, SQLite, a compiler, or a project-specific CLI. One shell interface lets the agent compose all of them without forcing the runtime designer to predict every useful operation.

The following are simplified examples from my personal agent traces.

### Example 1: In-place multi-file edits

You work on a project and you want to rename a variable across an implementation, its callers, and the tests.

Bash

```bash
python3 <<'PY'
from pathlib import Path
 
changes = {
    "src/services/billing.py": [
        (
            "def charge_customer(customer_id: str, amount: int):",
            "def charge_customer(customer_id: str, amount_cents: int):",
            1,
        ),
        (
            "client.charge(token, amount)",
            "client.charge(token, amount_cents)",
            1,
        ),
    ],
    "src/api/checkout.py": [
        (
            "charge_customer(order.customer_id, order.total)",
            "charge_customer(order.customer_id, order.total_cents)",
            1,
        ),
    ],
    "src/jobs/retry_payment.py": [
        (
            "charge_customer(job.customer_id, job.amount)",
            "charge_customer(job.customer_id, job.amount_cents)",
            1,
        ),
    ],
    "tests/test_billing.py": [
        (
            "charge_customer(customer.id, 1200)",
            "charge_customer(customer.id, amount_cents=1200)",
            2,
        ),
    ],
}
 
prepared = {}
for filename, replacements in changes.items():
    path = Path(filename)
    text = path.read_text()
    for old, new, expected in replacements:
        found = text.count(old)
        if found != expected:
            raise SystemExit(
                f"{filename}: expected {expected} occurrences of {old!r}, found {found}"
            )
        text = text.replace(old, new)
    prepared[path] = text
 
for path, text in prepared.items():
    path.write_text(text)
PY
 
ruff format src/services/billing.py src/api/checkout.py src/jobs/retry_payment.py tests/test_billing.py
ruff check src/services/billing.py src/api/checkout.py src/jobs/retry_payment.py tests/test_billing.py
pyright src/services/billing.py src/api/checkout.py src/jobs/retry_payment.py
pytest -q tests/test_billing.py tests/test_checkout.py tests/test_retry_payment.py
git diff --check
git diff --stat -- src/services/billing.py src/api/checkout.py src/jobs/retry_payment.py tests/test_billing.py
```

The script first counts every old snippet. If a file has the wrong number of matches, it exits before writing anything. Only then does it apply the four-file rename in one pass, so you never sit in a half-updated tree. After the write it formats, lints, type-checks, runs the focused tests, and prints a compact diff. Failed checks stay in the working tree, so the next turn can inspect and fix them. That verify-before-done cycle is the [inner loop](https://www.philschmid.de/inner-loop-vs-outer-loop).

### Example 2: Reproducing and bisecting a flaky regression

You have a test that fails only sometimes. You want the first commit that introduced the flake, without moving your current checkout.

Bash

```bash
set -euo pipefail
 
test -n "${GOOD_REV:-}"
repo=$(pwd -P)
scratch=$(mktemp -d)
 
cleanup() {
  git -C "$repo" worktree remove --force "$scratch/repo" >/dev/null 2>&1 || true
  rm -rf "$scratch"
}
trap cleanup EXIT
 
git worktree add --detach "$scratch/repo" HEAD >/dev/null
cd "$scratch/repo"
git bisect start HEAD "$GOOD_REV"
 
git bisect run bash -c '
  failures=0
  for seed in 11 29 47 71 101; do
    if ! TEST_SEED=$seed pytest -q tests/test_checkout_race.py; then
      failures=$((failures + 1))
    fi
  done
  test "$failures" -lt 3
' >"$scratch/bisect.log" 2>&1
 
bad=$(git rev-parse HEAD)
git bisect reset >/dev/null
 
printf 'first bad commit: %s\n' "$bad"
git show --no-ext-diff --format=fuller --stat "$bad"
git diff --no-ext-diff "$bad^" "$bad" -- src/checkout tests/test_checkout_race.py | sed -n '1,220p'
printf '\nlast bisect events:\n'
tail -n 40 "$scratch/bisect.log"
```

The extra worktree keeps your local files in place. Each commit is classified with five seeds, and it only counts as good if fewer than three runs fail, so one unlucky flake does not poison the bisect. When it lands, the script prints the first bad commit, a truncated diff of the suspected change, and the tail of the log. You get a diagnosis, not every test run in the prompt.

### Example 3: Correlating compressed production logs

You have compressed production logs that are too large to load into the model. You want the failing endpoints, their error classes, and P95 latency.

Bash

```bash
python3 <<'PY'
import gzip
import glob
import json
import sqlite3
import tempfile
from pathlib import Path
 
def records(pattern):
    for name in sorted(glob.glob(pattern)):
        path = Path(name)
        opener = gzip.open if path.suffix == ".gz" else open
        with opener(path, "rt", encoding="utf-8") as stream:
            for line in stream:
                try:
                    yield json.loads(line)
                except json.JSONDecodeError:
                    continue
 
with tempfile.TemporaryDirectory() as directory:
    db = sqlite3.connect(Path(directory) / "logs.sqlite")
    db.executescript("""
        PRAGMA journal_mode = OFF;
        PRAGMA synchronous = OFF;
        PRAGMA temp_store = FILE;
 
        CREATE TABLE access (
            req_id TEXT PRIMARY KEY,
            path TEXT NOT NULL,
            status INTEGER NOT NULL,
            latency_ms REAL NOT NULL
        );
 
        CREATE TABLE errors (
            req_id TEXT PRIMARY KEY,
            kind TEXT NOT NULL
        );
    """)
 
    access_batch = []
    for row in records("logs/access.jsonl*"):
        access_batch.append((
            row["req_id"],
            row["path"],
            int(row["status"]),
            float(row["latency_ms"]),
        ))
        if len(access_batch) == 10_000:
            db.executemany("INSERT OR REPLACE INTO access VALUES (?, ?, ?, ?)", access_batch)
            access_batch.clear()
    db.executemany("INSERT OR REPLACE INTO access VALUES (?, ?, ?, ?)", access_batch)
 
    error_batch = []
    for row in records("logs/error.jsonl*"):
        error_batch.append((row["req_id"], row["error_class"]))
        if len(error_batch) == 10_000:
            db.executemany("INSERT OR REPLACE INTO errors VALUES (?, ?)", error_batch)
            error_batch.clear()
    db.executemany("INSERT OR REPLACE INTO errors VALUES (?, ?)", error_batch)
 
    db.executescript("""
        CREATE INDEX access_status_req ON access(status, req_id);
        CREATE INDEX errors_req ON errors(req_id);
    """)
 
    rows = db.execute("""
        WITH matched AS (
            SELECT a.path, a.latency_ms, e.kind
            FROM access AS a
            JOIN errors AS e USING (req_id)
            WHERE a.status >= 500
        ),
        ranked AS (
            SELECT
                path,
                kind,
                latency_ms,
                row_number() OVER (
                    PARTITION BY path ORDER BY latency_ms
                ) AS position,
                count(*) OVER (
                    PARTITION BY path
                ) AS samples
            FROM matched
        )
        SELECT
            path,
            count(*) AS failures,
            round(avg(latency_ms), 1) AS average_ms,
            max(
                CASE
                    WHEN position = CAST((samples * 95 + 99) / 100 AS INTEGER)
                    THEN latency_ms
                END
            ) AS p95_ms,
            group_concat(DISTINCT kind) AS error_classes
        FROM ranked
        GROUP BY path
        ORDER BY failures DESC
        LIMIT 5
    """).fetchall()
 
    print(json.dumps([
        {
            "endpoint": path,
            "failures": failures,
            "average_ms": average,
            "p95_ms": p95,
            "error_classes": kinds.split(","),
        }
        for path, failures, average, p95, kinds in rows
    ], indent=2))
PY
```

Python reads the rotated files in batches and never dumps them into the prompt. SQLite does the join, the P95 ranking, and the error-class aggregation. What comes back is five JSON records: endpoint, failure count, average latency, P95, and error classes. [Context offloading](https://www.philschmid.de/context-engineering-part-2) keeps the intermediate data in the environment, and puts only the summary in the model.

## Why atomic tools were right

A few months ago, I argued that coding agents should [start with robust atomic tools](https://www.philschmid.de/agent-harness-2026) and avoid shell commands such as `cat`, `sed`, and `echo`. A careless command could flood the context window, return an opaque error, or corrupt an edit through bad quoting. Text output also cannot carry visual information into a vision model.

Those limits still apply to an uninstrumented shell. Foundational models are now much better at composing small Python patchers, Git commands, quoted heredocs, and focused tests. The Harness also absorbs the protections that made atomic tools useful:

- **Output control:** truncate large results and tell the agent how to request a narrower slice.
- **Diagnostics:** return the exit status, duration, timeout state, and process information.
- **Isolation and policy:** gate paths, network access, and destructive actions outside the model.
- **Asynchronous processes:** let the agent start, inspect, and stop long-running commands without blocking a turn.

**The one exception multimodal input:**

Text output cannot make a screenshot/image visible. A shell command can render a page, capture a chart, or save a video frame, but the pixels still need to enter the model through a multimodal channel.

## What this means for harness engineering

I compared a shell-centered setup with a configuration that exposed separate tools for file reading, writing, editing, and search. Both ran against the same coding task set under the same conditions. The shell-centered setup achieved on par or better performance.

That is [the Bitter Lesson](http://www.incompleteideas.net/IncIdeas/BitterLesson.html) applied to harness design: general methods that scale with computation beat hand-built shortcuts. Bash is that general computation layer.

A good agent should have more tools when they provide a better interface to a capability. If the model can do something better with a tool, it should use the tool. Browser control is a good example.

A browser tool can navigate, click, type, wait for the page to settle, and return a screenshot in the same call. Doing this through Bash means invoking a CLI, locating the captured artifact, and passing it through `view_media` in a second step. Service integrations can benefit in a [similar way](https://www.philschmid.de/mcp-best-practices), or stay on the shell with something like [mcp-cli](https://www.philschmid.de/mcp-cli) so the schemas never sit in the prompt.

Here is what to try:

- **Delete the micro-tools:** Try to use Bash to handle file reading, search, multi-file edits, diffs, and verification.
- **Use subagents as execution firewalls:** Delegate messy exploration and debugging, then return a clean result to the parent context.
- **Keep the system instructions minimal:** Give the agent more room for repository instructions, domain knowledge, and the task itself.

Our goal is a smaller interface with a larger action space.
