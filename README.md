# Stack to Transformers — AI Systems Foundations (2026)

A training log to become a globally competitive **AI Systems / ML Engineer** by end of **2026**.  
Focus: **70% Systems (OS/Compilers/Memory/Arch/Distributed)** + **30% ML internals**.

> Rule: **No reference without implementation. No implementation without artifact.**

---

## Goals (End of 2026)
- Explain program execution at **register / stack / heap / virtual memory** level.
- Build small but real components:
  - a tiny **interpreter/compiler** piece
  - **profiling + memory/perf** experiments
  - a small **distributed** ML/system demo (concept + runnable artifact)
- Understand transformer memory costs:
  - explain **attention O(n²)** as **data movement + memory bandwidth** problem
- Produce **meaningful artifacts** (code, logs, diagrams, reports).

---

## Training Principles
Every task must include:

1. **Concept** — the fundamental idea
2. **Reference** — book chapter + docs + blog/article (and optional video)
3. **Implementation** — concrete code or experiment
4. **Artifact** — evidence (code/log/diagram/report)

Every explanation should connect to:
- Memory layout (**stack / heap**)
- Registers intuition
- CPU execution flow
- Virtual memory
- Cache intuition
- Data movement (bus / network)

---

## Repo Structure
```text
.
├── weeks/
│   ├── week-01/
│   │   ├── plan.md
│   │   └── daily/
│   │       ├── day-01.md
│   │       └── day-02.md
│   └── week-02/...
├── labs/
│   ├── os/
│   ├── memory/
│   ├── compiler/
│   ├── perf/
│   └── distributed/
├── ml/
│   ├── transformer-internals/
│   └── profiling/
├── artifacts/
│   ├── screenshots/
│   ├── logs/
│   └── diagrams/
└── README.md
```

---

## Daily Workflow (Mandatory)
Each day (or session) includes:

- ✅ Today’s 2–3 tasks (1–3h each)
- ✅ Concept focus
- ✅ References (specific sections)
- ✅ Implementation steps
- ✅ Artifact to submit
- ✅ 1 quiz question
- ✅ 1 reflection question

---

## Day Template (copy into `weeks/week-XX/daily/day-YY.md`)
```md
# Week XX — Day YY: <title>

## 1) Today’s Tasks (2–3 tasks)
- [ ] Task 1:
- [ ] Task 2:
- [ ] Task 3 (optional):

## 2) Concept Focus
-

## 3) References
- **Book:** <book name>, Ch. <x> / Section <y>
- **Official Docs:** <link>
- **Blog/Article:** <link>
- **Optional Video:** <link>

## 4) Implementation
Steps:
1.
2.
3.

Commands:
```bash
# commands here
```

## 5) Artifacts (Evidence)
- Code: ...
- Logs: artifacts/logs/...
- Screenshot: artifacts/screenshots/...
- Diagram/Notes: artifacts/diagrams/...

## 6) Results / Notes
Observations:

What surprised me:

What I still don’t understand:

## 7) Quiz (1 question)
Q:
A:

## 8) Reflection (1 question)
```

---

## Artifact Standards
Artifacts must be **reproducible**:
- Include build/run commands
- Include environment notes if needed (OS, compiler version, Python version)
- Save outputs:
  - `artifacts/logs/*.txt`
  - `artifacts/screenshots/*.png`
  - `artifacts/diagrams/*.md` or `.png`

---

## Getting Started
### Recommended environment
- Linux or WSL2
- Tooling:
  - `gcc` / `clang`
  - `gdb`
  - `perf` (optional)
  - Python 3.10+

### Quick sanity check
```bash
mkdir -p artifacts/{logs,screenshots,diagrams}
mkdir -p weeks/week-01/daily
```

## Progress Index
### Week Plans
- Week 01 Plan

### Daily Logs
- [Week 01 Day 01](weeks/week-01/daily/day-01.md)

## Current Focus (Exploration Phase — 8 weeks)
- Systems/OS/Compiler fundamentals (70%)
- ML internals & performance intuition (30%)

## License
Personal learning project. Reuse welcome with attribution.
