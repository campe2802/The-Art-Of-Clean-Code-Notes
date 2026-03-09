# Chapter 4: Write Clean and Simple Code
*A practical cheat‑sheet of 17 principles for building software that stays readable, maintainable, and safe to change.*

> "Most of the cost of a software project is in long-term maintenance. Writing clean code from the start is the most cost-effective strategy."

> **Core idea:** Write code that is easy to **understand**, easy to **change**, and hard to **break**.

---

## Writing Clean Code: The Principles
1. [Think About the Big Picture](#1-think-about-the-big-picture)  
2. [Stand on the Shoulders of Giants](#2-stand-on-the-shoulders-of-giants)  
3. [Code for People, Not Machines](#3-code-for-people-not-machines)  
4. [Use the Right Names](#4-use-the-right-names)  
5. [Adhere to Standards and Be Consistent](#5-adhere-to-standards-and-be-consistent)  
6. [Use Comments](#6-use-comments)  
7. [Avoid Unnecessary Comments](#7-avoid-unnecessary-comments)  
8. [Principle of Least Surprise](#8-principle-of-least-surprise)  
9. [Don’t Repeat Yourself](#9-dont-repeat-yourself)  
10. [Single Responsibility Principle](#10-single-responsibility-principle)  
11. [Test](#11-test)  
12. [Small Is Beautiful](#12-small-is-beautiful)  
13. [Law of Demeter](#13-law-of-demeter)  
14. [YAGNI](#14-yagni-you-aint-gonna-need-it)  
15. [Don’t Use Too Many Levels of Indentation](#15-dont-use-too-many-levels-of-indentation)  
16. [Use Metrics](#16-use-metrics)  
17. [Boy Scout Rule & Refactoring](#17-boy-scout-rule--refactoring)  

---

## A quick mindset
- **Clarity beats cleverness.**
- **Simple beats flexible (until you truly need flexibility).**
- **Local wins matter—but don’t harm the system as a whole.**
- **Optimize for reading and change, not for typing speed.**

---

## 1) Think About the Big Picture
Big-picture thinking is a time-efficient way to drastically reduce the complexity of your application as a whole.

**What it means**
- Consider the system end‑to‑end: architecture, data flow, error handling, deployment, and long-term maintenance.
- Prefer solutions that reduce the number of concepts a developer must keep in their head.

**Practical habits**
- Sketch the flow before coding: inputs → transformations → outputs.
- Identify the “dominant” abstractions and keep them few and consistent.
- Choose the simplest architecture that meets requirements today **and** won’t fight you tomorrow.

**Red flags**
- “It works” but requires special knowledge to operate or debug.
- Multiple ways to do the same thing, with no clear standard.

---

## 2) Stand on the Shoulders of Giants
**What it means**
- Prefer proven libraries, patterns, and platform features over custom reinventions.

**Examples**
- Use standard logging, configuration management, and dependency injection patterns (when relevant).
- Use well-known data structures and algorithms instead of “homegrown” versions.

**Rule of thumb**
- Reinvent only when you can clearly justify the cost and maintenance burden.

---

## 3) Code for People, Not Machines
**What it means**
- Computers will run almost anything. Humans must **understand** it, debug it, and evolve it.

**Tactics**
- Write obvious code even if it’s a few lines longer.
- Keep functions short and focused.
- Prefer explicit over implicit when it improves clarity.

**Ask yourself**
- “Could a teammate understand this in 30 seconds?”

---

## 4) Use the Right Names
Good names reduce the need for comments and prevent mistakes.

### Naming rules
- **Choose descriptive names:** `usd_to_eur(amount)` rather than `f(x)`
- **Choose unambiguous names:** `usd_to_eur(amount)` rather than `dollars_to_eur` (what kind of dollars?)
- **Use pronounceable names:** `customer_list` rather than `cstmr_lst`
- **Use named constants, not magic numbers:** `CONVERSION_RATE = 0.9`

```python
# Bad
def f(x):
    return x * 0.9

# Good
CONVERSION_RATE = 0.9

def usd_to_eur(amount):
    return amount * CONVERSION_RATE
```

### Additional naming guidelines
- Prefer **nouns** for data, **verbs** for actions.
- Encode units when helpful: `timeout_seconds`, `price_usd`.
- Avoid jokes and “temporary” names in production code (`foo`, `bar`, `lol`, `final2`).
- Use consistent naming across layers (API ↔ DB ↔ domain).

---

## 5) Adhere to Standards and Be Consistent
**What it means**
- Consistency is a feature. It prevents accidental complexity.

**How to apply**
- Use formatters/linters: e.g., `black`, `ruff`, `isort`, `pre-commit`.
- Follow language and framework conventions.
- Create small team standards (naming, folders, logging, error handling).

**Goal**
- Reduce “style decisions” so the team focuses on solving real problems.

---

## 6) Use Comments
Comments are helpful when they explain **intent**, **context**, or **non-obvious constraints**.

**Good reasons to comment**
- Why a decision was made (“We do X because Y external system behaves like Z”).
- Tradeoffs (“We accept minor duplication here to keep this module independent”).
- Safety constraints (“Do not change ordering; required by downstream parser”).

---

## 7) Avoid Unnecessary Comments
Bad comments rot and lie.

**Avoid comments that**
- Repeat what the code already says.
- Explain *how* the code works when the code could be written more clearly.
- Are outdated or vague.

**Better alternatives**
- Rename variables/functions.
- Extract helpers.
- Simplify logic.

---

## 8) Principle of Least Surprise
**What it means**
- Code should behave the way a competent developer expects.

**Examples**
- `save_user()` should not also send emails unless the name makes that obvious.
- A function named `get_*` should not mutate state.
- Sorting functions should not silently change global settings.

**How to apply**
- Keep side effects explicit.
- Use predictable defaults.
- Avoid “hidden” behavior in constructors and imports.

---

## 9) Don’t Repeat Yourself (DRY)
**What it means**
- Avoid duplicating knowledge. Duplication increases inconsistency and bugs.

**Common refactor patterns**
- Extract shared functions/modules.
- Replace copy-paste with data-driven loops/config.
- Centralize business rules in one place.

**Caution**
- Don’t create “abstract” helpers too early. Duplicate a tiny bit until patterns are stable.

```python
# Bad — duplicated logic
def get_active_users():
    return [u for u in users if u.is_active and not u.is_banned]

def get_active_admins():
    return [u for u in admins if u.is_active and not u.is_banned]

# Good — extract shared logic
def is_eligible(user):
    return user.is_active and not user.is_banned

def get_active_users():
    return [u for u in users if is_eligible(u)]

def get_active_admins():
    return [u for u in admins if is_eligible(u)]
```

---

## 10) Single Responsibility Principle
> A responsibility is a reason to change.

**What it means**
- A module/function/class should have **one primary reason** to change.
- Separate concerns: parsing ≠ validation ≠ persistence ≠ business logic ≠ presentation.

**Benefits**
- Easier testing.
- Safer refactors.
- More reusable components.

---

## 11) Test
Testing protects behavior and enables confident change.

### Types of tests
- **Unit tests:** fast, small scope (pure logic, edge cases).
- **User Acceptance tests:** validate user-facing requirements end-to-end.
- **Smoke tests:** quick checks to ensure the system is “basically alive”.
- **Performance tests:** measure latency/throughput under load.
- **Scalability tests:** validate behavior as data/users grow.

**Practical guidance**
- Unit tests should be cheap and plentiful.
- Keep end-to-end tests fewer but high-value.
- Automate tests in CI and make failures actionable.

---

## 12) Small Is Beautiful
**What it means**
- Small functions, small modules, small interfaces.

**Why it helps**
- Less to read, less to break.
- Better reuse.
- Clearer responsibilities.

**Technique**
- Break large functions into steps with meaningful names.

---

## 13) Law of Demeter
**Goal:** maintain loose coupling so you can modify objects and operations without serious impact on each other.

**What it means**
- “Only talk to your immediate friends.”
- Avoid deep chains like: `a.b().c.d().e()`

**Why it matters**
- Deep chains create brittle coupling and make changes expensive.

**Fixes**
- Introduce methods that hide internal structure (tell, don’t ask).
- Pass data explicitly instead of navigating object graphs.

---

## 14) YAGNI (You Ain’t Gonna Need It)
**What it means**
- Never implement code because you *suspect* you’ll need it someday.
- Write code only if you’re **100% sure it’s necessary** now.

**Key ideas**
- The simplest and cleanest code is the **empty file**.
- “Think global, not local”: features that look useful locally can add global complexity.

**How to apply**
- Start with the smallest implementation that meets today’s requirements.
- Generalize only when real use cases demand it.

---

## 15) Don’t Use Too Many Levels of Indentation
**Why it matters**
- Deep nesting hides intent and makes code harder to scan.

**Techniques**
- Early returns/guard clauses.
- Extract nested blocks into named functions.
- Use polymorphism/dispatch tables instead of giant if/else ladders.

```python
# Bad — deep nesting
def process_order(order):
    if order:
        if order.is_valid:
            if order.has_stock:
                order.ship()

# Good — guard clauses
def process_order(order):
    if not order:
        return
    if not order.is_valid:
        return
    if not order.has_stock:
        return
    order.ship()
```

---

## 16) Use Metrics
**What it means**
- Measure what matters so decisions are based on evidence.

**Examples**
- Code metrics: complexity, duplication, test coverage (use as signals, not goals).
- Runtime metrics: latency, error rate, throughput, queue depth.
- Delivery metrics: lead time, deploy frequency, MTTR.

**Rule of thumb**
- Track a few metrics consistently and act on them.

---

## 17) Boy Scout Rule & Refactoring
- **Boy Scout Rule:** Leave the campground cleaner than you found it.  
- Improving code structure without changing behavior is **refactoring**.  
- **Rubber duck debugging:** Explain the problem out loud (to a person—or a rubber duck) to clarify your thinking.

**How to practice**
- Make small cleanups when touching code (rename, extract, simplify).
- Refactor behind tests.
- Keep refactors small and reviewable.

---

## Quick checklist (printable)
- [ ] Can a teammate understand this without extra context?
- [ ] Are names precise and consistent?
- [ ] Is behavior unsurprising (side effects explicit)?
- [ ] Is duplication minimized without premature abstraction?
- [ ] Are responsibilities separated?
- [ ] Are there tests at the right level?
- [ ] Is nesting shallow?
- [ ] Are important decisions documented (when needed)?
- [ ] Do we measure the right things?
- [ ] Did we leave the code a little cleaner?

---

## Key Takeaway
These 17 rules all serve the same goal: **reducing complexity**. Good names, small functions, minimal repetition, and shallow nesting make code easier to read and change. Clean code isn't about perfection—it's about writing software that respects the time and attention of every developer who will touch it after you.

---

*End of document.*
