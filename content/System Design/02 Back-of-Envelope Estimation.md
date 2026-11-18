---
title: "02 · Back-of-Envelope Capacity Estimation"
description: Quick, rough maths for requests, storage, bandwidth and compute, to size a design before building it.
tags:
  - system-design
  - estimation
aliases:
  - Capacity estimation
  - Napkin math
---

> [!summary] In one line
> Five minutes of rough arithmetic tells you whether a design needs one machine or a hundred, gigabytes or petabytes, before anyone writes code.

## The idea

You don't need precise numbers; you need the right **order of magnitude**. Estimation answers:

- How many **requests/events per second** (average and peak)?
- How much **storage** per day, per year, in total?
- How much **bandwidth** in and out?
- How much **compute** (CPU, GPU-hours)?
- Roughly what will it **cost**?

## How it works

### Handy numbers

| Fact | Value |
| --- | --- |
| Seconds in a day | 86,400 ≈ **~10⁵** |
| Seconds in a month | ≈ 2.6 million |
| 1 million per day | ≈ **12 per second** |
| 1 KB / 1 MB / 1 GB / 1 TB / 1 PB | 10³ / 10⁶ / 10⁹ / 10¹² / 10¹⁵ bytes (roughly) |
| Peak vs average | assume **2–10×**, depending on burstiness |

### Rough latencies (orders of magnitude)

| Operation | ~Time |
| --- | --- |
| Read from memory | nanoseconds to ~100 ns |
| Read 1 MB sequentially from SSD | ~1 ms or less |
| Round trip within an Azure region | ~1 ms or less |
| Round trip Europe ↔ India | ~100–150 ms |
| Read from Blob Storage (small object, hot tier) | ~tens of ms |

These matter for design: a page that makes 50 sequential cross-region calls will never be fast.

### The method

1. Write down **assumptions** (and label them as assumptions).
2. Compute **average** rates, then apply a **peak factor**.
3. Compute **storage growth** including replicas and metadata.
4. **Round aggressively.** 86,400 → 100,000.
5. Sanity-check: does the answer fit one machine, one database, one storage account?

## On the platform: worked example

**Assumptions** (placeholders, replace with real ones):

- 20 sites × 10 robots × 1 image every 10 s while operating
- 16 operating hours/day
- 2D image average **3 MB**; 10% of captures also produce a **50 MB** 3D point cloud
- Metadata ~ **2 KB** per image in MongoDB

**Rate**
- Images/s per robot = 0.1 → 200 robots → **20 images/s average**
- Peak (commissioning, bursts) ×5 → **~100 images/s**
- Per day: 20 × 16 h × 3,600 ≈ **1.15 M images/day**

**Storage per day**
- 2D: 1.15 M × 3 MB ≈ **3.5 TB/day**
- 3D: 115 k × 50 MB ≈ **5.8 TB/day**
- Total raw ≈ **~9 TB/day → ~3.4 PB/year** 😮

**Metadata**
- 1.15 M × 2 KB ≈ **2.3 GB/day → ~0.85 TB/year**: comfortable for MongoDB with good indexes and maybe partitioning later.

**Bandwidth in**
- 9 TB / 57,600 s ≈ **~160 MB/s average** (~1.3 Gbit/s), with peaks several times higher.

**What the numbers tell us**
- Raw storage cost dominates, so **storage tiering and retention policy** matter far more than API server size. See [[10 Blob Storage Patterns]].
- Do we really need to keep *every* frame? **Sampling or deduplication at the edge** could cut cost by 10×. That's a product question worth asking.
- 100 images/s of validation is easy to scale horizontally with Functions or workers. Compute isn't the bottleneck.

> [!tip] The best output of estimation is a question
> "Do we need all 3.4 PB?" is worth more than any server sizing.

## Common mistakes

- Forgetting **peak** load, replicas (×3 for redundancy), backups and indexes.
- Mixing bits and bytes (network in Gbit/s, storage in GB).
- False precision: "3,412.7 TB". Say "~3.4 PB".
- Not writing down assumptions, so nobody can challenge them.

## Check yourself

> [!question]- Roughly how many requests per second is 10 million requests per day?
> 10⁷ / 86,400 ≈ **~115/s** average (or 10⁷ / 10⁵ = 100/s as a quick round). Plan for several hundred per second at peak.

> [!question]- 500,000 images per day at 4 MB each. Storage per year?
> 500 k × 4 MB = 2 TB/day → ~730 TB/year, before replication.

> [!question]- Why estimate peak separately from average?
> Systems fail at peak. Capacity, autoscaling limits and queue sizing must handle bursts, not the daily mean.

## Apply it

- [ ] Get **real** numbers from the team (images/day, average size, retention) and redo this estimate for the actual platform.
- [ ] Compare the storage estimate with the actual Azure bill for storage. If they differ a lot, find out why.

## Go deeper

- [Jeff Dean's "Numbers everyone should know" (interactive)](https://colin-scott.github.io/personal_website/research/interactive_latency.html)
- [System Design Primer: back-of-the-envelope](https://github.com/donnemartin/system-design-primer#appendix)

**Related:** [[01 Requirements]] · [[03 Vertical vs Horizontal Scaling]] · [[10 Blob Storage Patterns]]
