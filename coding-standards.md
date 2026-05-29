---
id: steering-coding-standards
title: Coding Standards And Principles
inclusion: auto
---

# Coding Standards and Principles

## Core Principles

:::rule id="CODE-002" category="design"
Prefer the simplest design that satisfies the functional requirement and the non-functional requirements and other guidelines. To comply, verify abstractions meet all requirements.
:::

:::rule id="CODE-003" category="maintainability"
Keep changes minimal and scoped to the target behavior. To comply, avoid unrelated refactors and preserve existing public APIs unless the change requires breaking them.
:::

:::rule id="CODE-006" category="versioning"
Use Semantic Versioning in `MAJOR.MINOR.PATCH` format. To comply, bump version components according to compatibility impact.
:::

## CLI and Text I/O Discipline

:::rule id="CODE-007" category="cli"
Use stdin/args for input, stdout for normal output, and stderr for errors. To comply, route diagnostics to stderr and keep success output on stdout.
:::

:::rule id="CODE-008" category="cli"
Default CLI output must be human-readable text. To comply, add JSON output only when it meaningfully improves automation.
:::

:::rule id="CODE-009" category="cli"
CLI output must be deterministic. To comply, avoid timestamps and non-deterministic ordering in command output.
:::

## File Editing Rules

:::rule id="CODE-010" category="generation" mandatory="true"
NEVER hand-edit files in `src/ast-generated/`. To comply, make changes in metamodel or templates, then regenerate.
:::

:::rule id="CODE-011" category="generation" mandatory="true"
Modify AST structure only through `src/ast-model/` metamodels. To comply, regenerate generated AST output after metamodel edits.
:::

:::rule id="CODE-012" category="parser" mandatory="true"
Update grammar files when grammar behavior changes. To comply, change `FifthLexer.g4` and `FifthParser.g4` as required by the syntax change.
:::

:::rule id="CODE-013" category="parser" mandatory="true"
Keep `AstBuilderVisitor.cs` synchronized with grammar changes. To comply, update visitor methods whenever parse-tree shape or surface syntax changes.
:::

## Repository Cleanliness

:::rule id="CODE-015" category="repository" mandatory="true"
Use `scripts/` only for durable automation. To comply, keep one-off scripts outside tracked repository paths.
:::

## Security

:::rule id="CODE-018" category="security" mandatory="true"
Do not execute arbitrary code during parsing or generation. To comply, restrict execution paths to trusted, explicit operations.
:::

:::rule id="CODE-019" category="security"
Validate all external inputs before use. To comply, keep user input data separated from internal template logic.
:::
