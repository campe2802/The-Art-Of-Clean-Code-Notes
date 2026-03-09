
# Chapter 7: Do one thing well and other unix principles

This is the Unix philosophy: Write programs that do one thing
and do it well. Write programs to work together. Write programs to
handle text streams, because that is a universal interface.
—Douglas McIlroy

### Philosophy Overview

By focusing every function on one purpose only, you improve the maintainability
and extensibility of your code. The output of one program is the
input of another. You reduce complexity, avoid clutter in the output, and
focus on implementing one thing well.

### 15 Useful Unix Principles
- 1. Make Each Function Do One Thing Well
    - The Single Responsibility Principle: a function should have one reason to change.
    - If your function name contains "and", it’s doing too much—split it.
    - In Python, keep functions short and focused; compose larger behavior by calling smaller functions.
- 2. Simple Is Better Than Complex
    - Directly from Python’s Zen (`import this`): prefer readability and straightforward logic.
    - Resist clever one-liners that sacrifice clarity. Simple code is easier to debug, test, and extend.
- 3. Small Is Beautiful:
    - Reduce complexity.
    - Improve maintainability.
    - Improve testability.
- 4. Build a Prototype as Soon as Possible
    - Ship a working version fast, get feedback, iterate. Don’t aim for perfection on the first pass.
    - A prototype exposes design flaws early, when they’re cheapest to fix.
    - In Python, rapid prototyping is natural thanks to the REPL, dynamic typing, and rich libraries.
- 5. Choose Portability Over Efficiency:
    - Portability is the ability of a system or a program to be moved from one environment
to another and still function properly.
- 6. Store Data in Flat Text Files
    - Human-readable, portable, and easy to debug with standard tools (grep, awk, sed).
    - Prefer CSV, JSON, or YAML over proprietary binary formats whenever possible.
    - Text files enable version control, diffing, and interoperability across systems.
- 7. Use Software Leverage to Your Advantage
    - Don’t reinvent the wheel. Use libraries, frameworks, and existing solutions.
    - In Python: leverage PyPI’s vast ecosystem and the rich standard library before writing custom code.
    - Standing on the shoulders of well-tested code multiplies your productivity.
- 8. Avoid Captive User Interfaces:
    - Captive user interfaces are those that require the user to interact with the
program before proceeding with the main execution flow.
- 9. Make Every Program a Filter
    - Think stdin → process → stdout. Each function or program transforms data and passes it along.
    - This enables piping and composition—like chaining Python generators or using `map`/`filter`.
    - Design functions that take input, transform it, and return output without side effects.
- 10. Worse Is Better
    - A simple, working solution today beats a perfect one that never ships.
    - Related to Richard Gabriel’s philosophy and the KISS principle.
    - Get it working first, then improve incrementally based on real feedback.
- 11. Clean Code Is Better Than Clever Code
    - Clarity is better than cleverness.
- 12. Design Programs to Connect With Other Programs
    - The great programmer is as much an architect as a craftsperson. They create new programs as
a unique combination of old and new functions and other people’s programs.
- 13. Make Your Code Robust
    - A codebase is robust if it cannot be easily broken.
    - Handle edge cases and unexpected input at system boundaries (user input, external APIs).
    - Use defensive programming where it matters, but trust internal code to reduce unnecessary checks.
- 14. Repair What You Can—But Fail Early and Noisily
    - Never fail silently. Raise clear, descriptive exceptions so problems are caught immediately.
    - Python’s Zen: "Errors should never pass silently. Unless explicitly silenced."
    - Recover when possible, but if you can’t, crash loudly with enough context to diagnose the issue.
- 15. Avoid Hand-Hacking: Write Programs to Write Programs If You Can
    - Automate repetitive tasks. If you do something manually more than twice, write a script.
    - Think code generators, templates, and metaprogramming to eliminate human error.
    - In Python: use tools like `cookiecutter`, `Jinja2`, or even simple string formatting to automate boilerplate.

### Key Takeaway

The 15 Unix principles all point to the same central theme of the book: **reducing complexity**.
By making each piece do one thing well, keeping things simple and small, and designing components
that connect with each other, you create systems that are composable—the unifying idea behind Unix
philosophy. Composability means you can build powerful, complex behavior from simple, well-defined
building blocks, rather than writing monolithic programs that try to do everything at once.