---
title: I Could've Accessed 17T Microsoft Records
link: https://blog.faav.net/how-i-couldve-accessed-17-trillion-microsoft-records
source: hnrss-org
published: 2026-09-28T20:32:46Z
updated: 2026-09-28T20:32:46Z
first_seen: 2026-09-30T20:48:45.272500234Z
authors:
- luispa
content: extracted
html: 2026-09-28-i-could-ve-accessed-17t-microsoft-records.html
preview:
  file: 2026-09-28-i-could-ve-accessed-17t-microsoft-records.preview-4ae209a9b4f5.webp
  width: 256
  height: 81
  alt: image
  color: '#efe1e1'
images:
- source: https://blog.faav.net/assets/images/647380bf-4a6a-462c-b2f6-f16f2f02bc19.png
  original:
    file: 2026-09-28-i-could-ve-accessed-17t-microsoft-records.image-1f783648a2c7.png
    width: 2253
    height: 711
  color: '#f9f8f7'
- source: https://blog.faav.net/assets/images/abff2b7d-7f42-4806-ac65-365e0f7588a4.png
  original:
    file: 2026-09-28-i-could-ve-accessed-17t-microsoft-records.image-9937e49a2929.png
    width: 2254
    height: 1276
  variants:
  - file: 2026-09-28-i-could-ve-accessed-17t-microsoft-records.image-a49c5f89fec4.webp
    width: 320
    height: 181
  - file: 2026-09-28-i-could-ve-accessed-17t-microsoft-records.image-020fbd322fd0.webp
    width: 640
    height: 362
  - file: 2026-09-28-i-could-ve-accessed-17t-microsoft-records.image-b1d0116224af.webp
    width: 960
    height: 543
  - file: 2026-09-28-i-could-ve-accessed-17t-microsoft-records.image-de5dfac17498.webp
    width: 1280
    height: 725
  - file: 2026-09-28-i-could-ve-accessed-17t-microsoft-records.image-c65356dabe8a.webp
    width: 1600
    height: 906
  - file: 2026-09-28-i-could-ve-accessed-17t-microsoft-records.image-880ce30a8189.webp
    width: 2254
    height: 1276
  color: '#f7f7f7'
- source: https://blog.faav.net/assets/images/b83ca30b-5b6b-4ba4-bc37-faf96b16bef7.png
  original:
    file: 2026-09-28-i-could-ve-accessed-17t-microsoft-records.image-33fdfd7b83a7.png
    width: 1492
    height: 564
  color: '#30333b'
- source: https://blog.faav.net/assets/images/83d346c9-f469-4848-85c1-17022a9ea5b5.png
  original:
    file: 2026-09-28-i-could-ve-accessed-17t-microsoft-records.image-6b0344cab79b.png
    width: 1483
    height: 582
  variants:
  - file: 2026-09-28-i-could-ve-accessed-17t-microsoft-records.image-a56bc70b8ac9.webp
    width: 320
    height: 126
  - file: 2026-09-28-i-could-ve-accessed-17t-microsoft-records.image-1077a19c75ac.webp
    width: 640
    height: 251
  - file: 2026-09-28-i-could-ve-accessed-17t-microsoft-records.image-92356f9e3ab4.webp
    width: 1483
    height: 582
  color: '#30333b'
- source: https://blog.faav.net/assets/images/a32a3d97-1bef-43ea-85f6-0f8ded60eb2c.png
  original:
    file: 2026-09-28-i-could-ve-accessed-17t-microsoft-records.image-48e1a32bb45a.png
    width: 1504
    height: 647
  color: '#31333b'
---

Sep 25, 2026

**An estimated 17.3 trillion stored rows across a wide range of Microsoft datasets were reachable through a single internal analytics service, all because it never checked the signature on a login token. That flaw let me claim an administrator’s identity and submit unauthorized SQL queries without any real credentials. I used only table descriptions, metadata, and bounded sample rows to understand the potential scope.**

Two quick notes first. The impact I describe is hypothetical. It’s what an attacker could have done with this access, but luckily I found the bug instead, reported it, and never touched any customer data or PII. And for transparency: Microsoft had editorial control over this post, cutting sections and figures and reshaping how the impact is described before publication.

Microsoft said the following about this finding:

> “We appreciate the opportunity to investigate the findings reported by Faav. Their submission and coordinated vulnerability disclosure helped us to better protect our customers by hardening our services. We value and appreciate safe security research under the terms of the Microsoft Bug Bounty Program and look forward to continuing to work with Faav in the future.”

Hey! I’m Faav. A little over a year ago, when I was 15, I published *Break into any Microsoft building: Leaking PII in Microsoft Guest Check-In*, my first Microsoft write-up. I’m 16 now, and this one is a little bigger.

Since then I’ve gone all-in on bug bounty. I’ve spent the year hacking Microsoft off and on around school, and finding bugs across Amazon, Google, Adobe, and a bunch of other companies. I also started building AI into how I hunt, which led me to develop Antares, my personal AI hackbot.

This one started as an automated lead that Antares couldn’t finish. Ten days later, after a Friday of schoolwork and one late-night hunch, it turned into the biggest Microsoft bug I’d ever found.

## Finding the Titan API

On August 25, 2026, Antares identified an internal Microsoft service called Titan. Its web interface sat behind a **VPN REQUIRED** page for Microsoft employees, so the frontend was out of reach. But since when has a locked front door stopped anyone?

![image](https://blog.faav.net/assets/images/647380bf-4a6a-462c-b2f6-f16f2f02bc19.png)

*The “VPN REQUIRED” page shown to a non-employee visiting Titan’s frontend.*

The API wasn’t linked anywhere on the frontend, so Antares searched Microsoft subdomains and found a separate endpoint that resolved to an Azure Cloud Services host. Its public Swagger file listed four routes:

```text
/GetConfiguration
/GetOnboardedTables
/v2/Query
/v2/Insert
```

The Swagger doc specified Azure AD bearer authentication for three of the four routes. The exception was `/v2/Query`, which also happened to be the one that accepted raw SQL. So naturally, that’s where I started poking.

The query needed a `tableName`, and Swagger gave no example values. I pulled 2023 snapshots of Titan’s login and privacy pages from the Wayback Machine, and reading the archived Superset configuration recovered 56 table definitions, including a routing value called `TestData`.

![image](https://blog.faav.net/assets/images/abff2b7d-7f42-4806-ac65-365e0f7588a4.png)

*The archived Titan interface before the current VPN restriction.*

I tried that value against the live API:

```http
POST /v2/Query HTTP/1.1
Host: [redacted]
Content-Type: application/json

{"query":"SELECT 1","tableName":"TestData","rowLimit":1}
```

With no authorization header it returned `401 Unauthorized`, so Antares started probing how it validated JWTs.

## Breaking the JWT

Over the next ten days, while I worked through hundreds of other leads, Antares kept coming back to Titan and chipping away at its JWT checks one error at a time.

It started with a token from my external Entra test tenant, created months earlier and used regularly for testing. Titan threw back a tenant error. Changing the tenant to Microsoft’s reached an audience error. Changing the audience hit an application allowlist error. Changing the application ID finally reached a user lookup.

The payload kept changing while the signature stayed exactly the same, and Titan kept accepting the new claims, like a bouncer checking the name on every ID but never looking at the photo. That was the first big clue it wasn’t verifying signatures.

I’d exploited an unsigned JWT bypass by hand before I ever used AI, so I recognized the pattern immediately.

Next I replaced the token entirely with a synthetic JWT using this header:

```json
{"alg":"none","typ":"JWT"}
```

A normal signed JWT has three populated sections: `header.payload.signature`. Mine ended with a bare period, because the third section was empty:

```text
base64url(header).base64url(payload).
```

Titan wasn’t validating the signature at all.

The payload used the values Titan expected, but with a `upn` I controlled:

```json
{
  "aud": "[redacted]",
  "tid": "[redacted]",
  "appid": "[redacted]",
  "upn": "test@microsoft.com",
  "oid": "00000000-0000-0000-0000-000000000000"
}
```

That cleared the tenant, audience, and application checks, then returned:

```text
User 'test@microsoft.com' not found
```

Antares was running Codex and Claude on the lead. Because a UPN is normally an email-formatted Entra identity, both models kept testing placeholders, published service aliases, and Microsoft employee-style addresses.

The unsigned token was already reaching Titan’s local user lookup. Antares just couldn’t find a UPN Titan recognized, so it had no working query and no impact to show, and the finding stayed a lead.

### Trying `admin`

I’d spent Friday on schoolwork. By the time the Titan lead popped up again and I decided to take another look myself, it was after 1 AM on Saturday, September 5.

I tried a few valid-looking UPNs and they failed too. So I stopped guessing and started thinking about what the backend was actually doing with the claim. What if it was using `upn` to look up a local application username? I changed the unsigned token’s `upn` from an email-formatted identity to `admin`.

The result was only the number `1`, but after ten days of authentication errors, it was a very interesting `1`. I was finally executing SQL as Titan’s administrator.

![image](https://blog.faav.net/assets/images/b83ca30b-5b6b-4ba4-bc37-faf96b16bef7.png)

*Titan accepted the unsigned administrator identity and executed the SQL query.*

The funny part is that `admin` is obvious, but obviously not a valid UPN. That’s exactly why Antares never guessed it. I only tried it because I stopped taking the field name at face value and thought about what a developer might have done on the backend.

Titan used the unsigned `upn` claim as a local username. `admin` resolved to local user ID 1, which held the `Admin` role, and the SQL ran. Sometimes the answer really is just `admin`.

## Inside the Databases

I tried `TestData` first, assuming it would only hold fake data, and it did: test cases in a test database. But `SHOW DATABASES` revealed the rest, including Titan’s platform metadata database. From there I could query application tables directly.

The metadata database held the real data. The first thing I found was a table of active application user accounts with names, email addresses, and login history. Across the platform metadata:

- Approximately 25,000 account and email records.
- 17,990 employee email records.
- 15,001 employee organization records.
- 355 database configurations.
- 20,979 virtual-dataset SQL definitions.
- 24,569 dashboards, 425,891 charts, and 27,347 dataset definitions.

![image](https://blog.faav.net/assets/images/83d346c9-f469-4848-85c1-17022a9ea5b5.png)

*An application user record with identifying information and login activity. The password field held a placeholder hash from Superset’s local user model, not a real Microsoft credential. Sensitive values are redacted.*

The Titan user and usage directory exposed employee details such as job titles, departments, and management hierarchy for staff associated with Titan. This data only covered a subset of Microsoft employees, not the full employee directory. It could’ve helped an attacker craft targeted social-engineering attempts, though I never tested or demonstrated that.

At that point the employee data looked like the main finding. Then I noticed a separate Bing analytics source.

## Validating a Bounded Bing Analytics Sample

I requested one row from the latest available Bing analytics partition. A second one-row query returned a different record from the same partition. These bounded samples showed that Bing search analytics were reachable through the service. I limited my testing to two one-row samples. Bing was only one of the databases reachable this way.

![image](https://blog.faav.net/assets/images/a32a3d97-1bef-43ea-85f6-0f8ded60eb2c.png)

*One analytics record containing search, identifier, and high-level location fields. My query pulled only a few of the available fields. Note that the location values didn’t contain precise user locations, but rather country or state-level information from reverse IP. Sensitive values are redacted.*

Because MUIDs appeared in more than one dataset, it’s plausible that user activity could’ve been correlated across services, though I never actually did this. I didn’t identify anyone, link records across datasets, or build any profile from what I sampled.

I’d started drafting a report to MSRC as soon as I found the employee records. After seeing the Bing data I reported it immediately.

## 17.3 Trillion Rows

The total scale was the last thing I worked out.

I tested all 56 routing values from the archived config with `SELECT 1`. 30 were still active. Each routing value pointed to a backend configuration, and each configuration contained one or more databases, so the 30 live values resolved through 24 configurations to 17 connected analytics databases spanning 9,863 unique table names.

There was no convenient grand total. I had separate metadata counts from 17 ClickHouse databases, so I handed them to the AI to sum while I checked how each `Distributed` table mapped to its backing cluster. I counted one replica per shard and verified the total through two metadata paths: `system.tables.total_rows` and active `system.parts`.

The sum came back unformatted:

```text
17333335124315
```

I had to split it into groups of three just to read it:

```text
17,333,335,124,315
```

Seventeen trillion - a storage estimate from metadata that likely includes historical, duplicated, and derived data, but quite the high number nonetheless. That was the estimated scale of the environment technically reachable through this bypass.

My first thought was that the replicas had been double-counted, or a few extra zeroes had slipped in. I checked again. They hadn’t. Both paths returned the same number.

It was 2 AM. I wanted to yell, or at least say something out loud, but my parents were asleep. So I just sat there staring at `17,333,335,124,315` and checked the math again.

## Closing Thoughts

There are two things I took away from this.

First, AI and human intuition compounded here. Antares did ten days of work I didn’t have to: enumerating subdomains, grinding through JWT error messages one field at a time, mapping the whole attack surface, and getting the unsigned token through four layers of validation. What it couldn’t do was realize that `upn` wasn’t actually a UPN. Looking back, the `User not found` error should’ve been the tell. The crafted JWT had already passed Titan’s authentication checks, and it was simply trying to match `upn` to one of its own users. Antares wouldn’t have gotten here alone, and neither would I. Its persistence, plus one human hunch, is what made this find possible.

Second, the bug. Titan validated the contents of the JWT (tenant, audience, app ID, user) but never verified the signature, the most important part of any authentication check. The authentication checks felt like a hotel where every door had a working keycard reader, but any keycard unlocked any room. Despite all the access-control logic existing in the app, the one missing piece made it all pointless. If you’re a developer (or coding agent) reading this, the most important takeaway from this post is to make sure you verify signatures above all else when building auth.

## Timeline

- 08/25/26 - Antares surfaced Titan’s public API and saved it as a lead. I recovered the archived Superset configuration and its 56 table definitions from the Wayback Machine.
- 08/25/26–09/05/26 - Antares repeatedly worked through Titan’s JWT checks and tested email-formatted UPNs.
- 09/05/26 - I returned to the request, changed `upn` to `admin`, and got the SQL query working.
- 09/05/26 - Confirmed access to 30 live routing targets and 17 connected analytics databases without dumping the underlying records, then reported the vulnerability to MSRC. Case 144051 was opened the same day.
- 09/06/26–09/08/26 - MSRC asked me to stop testing and requested my IP address to confirm there was no activity beyond my security research. They confirmed the issue was receiving priority attention.
- 09/09/26 - The API endpoint was locked down. MSRC indicated that the report prompted immediate investigation and remediation to address the remaining exposure.
- 09/17/26 - Awarded $5,000
- 09/22/26 - Met with Microsoft to discuss the finding and coordinate disclosure.
- 09/22/26–09/24/26 - Reworked this writeup at Microsoft’s request, rewording impact and removing some sections and figures ahead of publication.
