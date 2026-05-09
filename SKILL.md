---
name: volven-debt-python
description: Review Python code for technical debt that creates future cost. Use when asked to find Python maintenance risk around dynamic dictionaries, missing validation, untyped public functions, mutable globals, swallowed exceptions, implicit schemas, async error handling, or brittle mock-heavy tests.
---

# Vølven Python Debt Review

Vølven sees the Python code that will hurt later.

Use this skill with the base `volven-debt` skill. This Python variant looks for future cost caused by dynamic shapes, hidden schemas, weak boundaries, and tests that prove the mock, not the behavior.

Do not report formatting, import sorting, ordinary type-checker findings, or lint findings already owned by Ruff, mypy, pyright, Black, isort, or the project's existing checks.

## Python Debt Signals

Look for real debt in these places:

- dictionaries passed through important paths without validation or a named schema
- untyped public functions in modules that other code depends on
- global config, mutable module state, or singleton-style state that leaks between calls or tests
- swallowed exceptions that turn failures into ambiguous `None`, empty dicts, or silent skips
- script-shaped code pretending to be a stable module boundary
- implicit schemas in data pipelines, API clients, queues, or file imports
- async code without cancellation, timeout, retry, or error discipline
- tests that mock the whole dependency graph and assert implementation details
- fixtures that define the real contract better than production code does

## Hidden Contracts

Be extra suspicious when Python code depends on:

- dict keys that exist only because one caller happens to send them
- environment variables read deep inside business logic
- task ordering in async code that is not enforced
- monkeypatches that hide production behavior
- comments that explain required input shape instead of code checking it

## Reporting Rules

Use the base finding format with `language: python`.

Prefer small repayment steps:

- add one typed dataclass, TypedDict, Pydantic model, or boundary validator
- type the public function that other modules call
- move config reads to one explicit boundary
- replace one swallowed exception with a narrow handled case and visible failure
- rewrite one mock-heavy test as a behavior test at the nearest useful boundary

## Ignore If

Do not report a finding when:

- the code is a throwaway script with no shared contract
- the dynamic shape is validated immediately at the edge
- the exception is intentionally swallowed with a narrow reason and observable outcome
- the async work is local, bounded, and cannot escape the call
- the test double protects a slow or external dependency while still asserting behavior
