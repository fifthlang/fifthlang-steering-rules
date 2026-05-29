---
id: steering-grammar-and-parser
title: Grammar And Parser Rules
inclusion: fileMatch
fileMatchPattern: "src/parser/**,docs/**/*.5th,test/**/*.5th,src/parser/grammar/**"
---

# Grammar and Parser Rules

## Split Grammar Architecture

:::rule id="GRAM-001" category="architecture" mandatory="true"
Keep syntax responsibilities split across lexer, parser, and AST builder. To comply, define tokens in `FifthLexer.g4`, syntax rules in `FifthParser.g4`, and parse-tree mapping in `AstBuilderVisitor.cs`.
:::

## Grammar Change Workflow

:::rule id="GRAM-002" category="workflow" mandatory="true"
Any grammar change must follow the full grammar-update workflow. To comply, update grammar files and `AstBuilderVisitor.cs`, add samples, run parser tests, then run `dotnet test fifthlang.sln`.
:::

## Grammar Compliance for Examples and Tests

:::rule id="GRAM-003" category="validation" mandatory="true"
All non-negative `.5th` samples in docs and tests must parse with the current grammar. To comply, run `just validate-examples` before commit.
:::

## Common Non-Fifth Patterns to Avoid

:::rule id="GRAM-004" category="syntax" mandatory="true"
Do not use C# style `var <name> =` in Fifth examples or tests. To comply, use canonical declarations such as `name: type = value`.
:::

## Canonical Guard Syntax

:::rule id="GRAM-007" category="guards"
Use only parameter-constraint guard syntax. To comply, follow this contrast:

```fifth
// INVALID
myprint(int x) when x == 0 => std.print(x);

// VALID
myprint(int x | x == 0) { std.print(x); }
```
:::

## Negative Tests

:::rule id="GRAM-008" mandatory="false" category="validation"
Exclude intentional negative tests from normal example-validation checks. To comply, keep them in `*/Invalid/*`, include `invalid` in filename, or use explicit negative-test markers; use `--include-negatives` only for debugging.
:::
