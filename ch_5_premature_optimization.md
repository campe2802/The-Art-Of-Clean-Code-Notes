# Chapter 5: Premature Optimization Is the Root of All Evil

> "Premature optimization is the root of all evil." —Donald Knuth

The urge to optimize before understanding the real bottlenecks leads to wasted effort, harder-to-read code, and solving problems that don't exist.

## Six Types of Premature Optimization

- **Optimizing Code Functions:** Be wary of spending time optimizing functions before you know how much
those functions will be used. Profile first—only optimize hot paths that actually impact performance.
- **Optimizing Features:** Avoid adding features that aren't strictly necessary and wasting time
optimizing those features. A feature nobody uses is pure cost, no matter how fast it runs.
- **Optimizing Planning:** Over-planning before you start coding wastes time. You can't predict every requirement upfront. Plan enough to get started, then iterate based on real feedback.
- **Optimizing Scalability:** Don't engineer for millions of users when you have hundreds. Build for today's scale and refactor when growth demands it—most scalability problems are good problems to have.
- **Optimizing Test Design:** Writing elaborate test suites for code that may change or be discarded is premature. Start with simple, essential tests and expand coverage as the codebase stabilizes.
- **Optimizing Object-Oriented World Building:** Creating deep class hierarchies, complex design patterns, and elaborate abstractions before understanding the problem domain adds unnecessary complexity. Start simple and refactor toward the right abstractions as patterns emerge.

## Six Tips for Performance Tuning

- **Measure First, Improve Second:**
    - What you don't measure can't be improved. Use profilers (`cProfile`, `line_profiler`) and benchmarks before touching code.

```python
# Profile before optimizing
import cProfile
cProfile.run('my_function()')
```

- **Pareto Is King:** 80% of performance problems come from 20% of the code. Focus your optimization effort on the critical few bottlenecks, not the trivial many.
- **Algorithmic Optimization Wins:**
    - Many bottlenecks can be resolved by tuning your algorithms and data structures. Switching from O(n²) to O(n log n) beats any micro-optimization.

```python
# Bad — O(n) lookup on every check
if item in large_list:       # list scan
    ...

# Good — O(1) lookup
if item in large_set:        # hash lookup
    ...
```

- **All Hail the Cache:** Store the results of expensive computations so you don't repeat them. Caching is one of the simplest and most effective performance wins.

```python
from functools import lru_cache

@lru_cache(maxsize=128)
def expensive_computation(n):
    # result is cached after the first call with each n
    return sum(i * i for i in range(n))
```

- **Less Is More:** The fastest code is the code that never runs. Remove unnecessary processing, reduce data transformations, and simplify logic before trying to make it faster.
- **Know When to Stop:**
    - Ask yourself regularly: Is it worth the effort to keep optimizing? Diminishing returns are real. "Good enough" performance with clean code beats marginal speed gains with unreadable code.

## Key Takeaway
Write clean, readable code first. Optimize only when you have evidence of a real performance problem—measured, not guessed. Most code doesn't need to be fast; it needs to be correct, clear, and maintainable. When you do optimize, target the bottleneck, not the whole codebase.
