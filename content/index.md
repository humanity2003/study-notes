---
title: Study Notes
description: Learning notes on the way from Scrum Master to Machine Vision Platform Engineering Manager.
aliases:
  - home
---

> [!abstract] Why these notes exist
> The goal for the next 12–18 months is to **own outcomes, not ceremonies**: to become the Engineering Manager for a Machine Vision / AI platform, with enough architectural credibility to make and defend real technical decisions.
>
> These notes follow that roadmap one month at a time. Each chapter explains a topic, connects it to an image → annotation → training → deployment pipeline, and ends with questions to check myself.

## Sections

| Section | What it covers | Status |
| --- | --- | --- |
| [[Software Architecture/index\|Software Architecture]] | Styles, boundaries, quality attributes, C4, ADRs and trade-offs | Month 1 · Nov 2026 |
| [[System Design/index\|System Design]] | Requirements, estimation, scaling, caching, queues, data distribution, reliability | Month 1 · Nov 2026 |
| Python & FastAPI | Coming in December | Planned |
| Containers, CI/CD & MongoDB | Coming in January | Planned |

## How to use a chapter

Every chapter has the same shape, so it's easy to revise:

1. **In one line**: the idea, in one line.
2. **The idea** and **how it works**: the explanation, with a diagram where it helps.
3. **On the platform**: what it looks like in a machine-vision pipeline (Blob Storage, Azure Functions, Kubeflow, MongoDB, V7).
4. **Trade-offs** and **common mistakes**.
5. **Check yourself**: questions with hidden answers. Click to reveal.
6. **Apply it**: one small task for the capstone or for work.

> [!tip] When to tick a topic
> Tick an item on the roadmap only when I can explain it **without notes** and have **used it once** in the capstone or at work.

## The rules for the year

1. One primary resource at a time.
2. At least half of each week's hours spent building, not watching.
3. Every month ends with a deliverable.
4. YouTube is for specific blockers, not a curriculum.

## Month 1 deliverable

- [ ] A current-state architecture diagram of the real platform (C4 context + container)
- [ ] A skills-gap list
- [ ] A first draft of platform KPIs

See [[Software Architecture/07 C4 Diagrams|C4 Diagrams]] and [[System Design/01 Requirements|Requirements]] to get started.
