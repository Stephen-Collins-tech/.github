# Linux CFS Scheduler — `kernel/sched/fair.c`

**Repository:** `torvalds/linux`
**File:** [`kernel/sched/fair.c`](https://github.com/torvalds/linux/blob/master/kernel/sched/fair.c)
**Lines:** ~13,000 (as of 2026)
**Primary function of interest:** `task_tick_fair()` and its call tree

---

## Why It's Here

`fair.c` is the implementation of Linux's Completely Fair Scheduler (CFS). It decides, thousands of times per second, which process gets CPU time on every core of every Linux machine on the planet.

It is not a simple function. It is a 13,000-line file containing dozens of interlocked functions, each of which assumes deep knowledge of the others. The central scheduling function `__schedule()` calls into `fair.c` via a table of function pointers — a layer of indirection that makes static analysis difficult and human reasoning harder.

This is complexity that is *earned*. But it is still complexity.

---

## Structural Properties

| Property | Observation |
|---|---|
| **Cyclomatic complexity** | Extremely high. The main scheduling path has 40+ decision branches. |
| **Coupling** | `fair.c` directly references global kernel state: runqueues, cgroups, NUMA topology, load balancing domains. |
| **Change frequency** | Commits to this file never stop. Every kernel release touches it. |
| **Test surface** | Nearly untestable in isolation. Behavior emerges from the interaction of hundreds of concurrent processes. |
| **Documentation** | Sparse inline comments. Understanding requires reading Molnar's original CFS paper alongside the code. |

---

## What Makes It Interesting

CFS is built around a red-black tree of virtual runtimes. The scheduler picks the leftmost node (lowest `vruntime`) and runs it. Simple in theory.

In practice, `fair.c` handles:

- NUMA-aware load balancing across CPU domains
- cgroup CPU quotas and throttling
- Scheduler groups and bandwidth control
- Idle CPU migration
- Wake-up preemption heuristics
- Real-time priority interaction

Each of these systems interacts with the others. Change the load balancing heuristic and you affect wake-up latency. Change throttling and you affect fairness. The complexity is not accidental — it reflects real physical constraints of modern hardware.

**The lesson:** Some complexity cannot be abstracted away. It can only be managed. `fair.c` is the canonical example of complexity that tracks the problem domain exactly.

---

## The Risk Angle

If you run Hotspots against the Linux kernel, `fair.c` will appear near the top of any risk report — not because it is poorly written, but because it is the file where:

1. Change frequency is extremely high
2. Understanding requires global context
3. Failures are invisible until they affect performance at scale
4. Regression is nearly impossible to detect in testing

This is the profile of a file that warrants the most careful review on every commit. The Linux maintainers know this, which is why CFS patches go through Ingo Molnar personally.

---

## Further Reading

- [CFS Scheduler Design](https://www.kernel.org/doc/html/latest/scheduler/sched-design-CFS.html) — official kernel docs
- [Molnar's original CFS announcement](https://lwn.net/Articles/230501/) — lwn.net, 2007
- [An Analysis of Linux Scalability to Many Cores](https://pdos.csail.mit.edu/papers/linux:osdi10.pdf) — MIT CSAIL

---

*Analyzed by [Hotspots](https://hotspots.dev) · Stephen Collins.tech LLC*
