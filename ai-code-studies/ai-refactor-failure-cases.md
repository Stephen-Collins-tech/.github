# AI Refactor Failure Cases

*Cases where AI-suggested refactors made structural risk worse, not better.*

The goal is not to bash AI tooling — it's to understand where it fails so you can use it more safely.

---

## Case 1: The "Clean" God Object

**Scenario:** An engineer uses Cursor to refactor a 600-line service class. The agent correctly identifies that the class is doing too much and splits it into four smaller classes.

**What looked better:**
- Each class was under 150 lines
- Naming was consistent and clear
- Linter reported zero issues
- Tests passed

**What got worse:**
- The four new classes shared mutable state through a context object passed to every method
- The coupling that existed implicitly in the original class became explicit — and increased
- Instead of one place to change when the business logic changed, there were four
- A follow-up change three months later touched all four classes simultaneously

**LRS change:** +2.1 average across the four new files vs. the original single file.

**Lesson:** Splitting a file is not the same as reducing complexity. If the split introduces shared state or tight coupling between the new modules, it moves complexity rather than removing it.

---

## Case 2: Test Suite Hallucination

**Scenario:** An agent generates a test suite for a payments module. 94% code coverage. All tests pass.

**What looked better:**
- Coverage metrics green
- Every public method had tests
- Edge cases were described in test names

**What got worse:**
- Many "edge case" tests were structurally identical to happy path tests with different input values
- The tests asserted on outputs but not on side effects (database writes, event emissions)
- The coverage metric counted lines, not branches — several critical error paths had zero actual coverage
- The first production incident revealed a bug in an error path that no test touched

**Lesson:** AI-generated test suites can produce high coverage numbers while providing low actual safety. The tests that an agent doesn't write are often more important than the ones it does.

---

## Case 3: The Abstraction That Went Nowhere

**Scenario:** An agent refactors three similar API handlers into a single generic handler with a configuration object.

**What looked better:**
- DRY principle satisfied
- 300 lines reduced to 80
- Single place to modify handler behavior

**What got worse:**
- The configuration object grew to accommodate every edge case the three original handlers handled differently
- Within six months, the configuration object had 22 fields, most of which only applied to one of the three original use cases
- New engineers had to read all 22 configuration options to understand any single handler
- Adding a fourth handler required understanding all the complexity of the existing three

**LRS change:** The generic handler file increased in LRS by 3.4 over 18 months as the configuration object grew.

**Lesson:** Premature abstraction has a compounding cost. AI tools are particularly prone to this because they optimize for reducing lines in the immediate diff, not for the long-term cost of the abstraction they introduce.

---

## Pattern Summary

| Failure Mode | Root Cause | Detection Signal |
|---|---|---|
| Split without decoupling | Agent optimizes for file length, not coupling | Rising fan-in on new files |
| Shallow test coverage | Agent can't know which paths matter most | Branch coverage vs. line coverage gap |
| Premature abstraction | Agent optimizes for DRY in the immediate diff | Configuration object growth over time |
| Style-correct, semantic-wrong | Agent matches patterns without understanding intent | Post-merge incident rate |

---

## What This Suggests

AI refactors are most useful for:
- Purely mechanical transformations (rename, format, extract constant)
- Well-specified refactors with explicit constraints ("split this class but don't add shared state")

AI refactors are highest-risk for:
- Anything that requires global codebase context
- Test generation in security-critical or payments paths
- Abstractions that will be extended by future engineers

---

*Research by [Stephen Collins.tech LLC](https://stephencollins.tech) · Powered by [Hotspots](https://hotspots.dev)*
