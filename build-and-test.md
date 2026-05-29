---
id: steering-build-and-test
title: Build And Test Commands
inclusion: auto
---

# Build and Test Commands

## Prerequisites

:::rule id="BUILD-001" category="prerequisites" mandatory="true"
Use the pinned toolchain: .NET SDK `10.0.100` from `global.json`, Java 17+, and `src/parser/tools/antlr-4.13.2-complete.jar`. To comply, run `dotnet --version` and `java -version` before troubleshooting build failures.
:::

## Essential Commands

:::rule id="BUILD-002" category="commands"
Use `dotnet restore fifthlang.sln` as the standard restore entry point. To comply, do not cancel restore; set automation timeout to at least 120 seconds.
:::

:::rule id="BUILD-003" category="commands"
Use `dotnet build fifthlang.sln` as the standard build entry point. To comply, do not cancel build; set automation timeout to at least 120 seconds.
:::

:::rule id="BUILD-004" category="testing"
Use `dotnet test fifthlang.sln` as the default regression gate. To comply, do not cancel test runs; set automation timeout to at least 5 minutes.
:::

:::rule id="BUILD-005" mandatory="false" category="testing"
Use `dotnet test test/ast-tests/ast_tests.csproj` only as a local smoke test. To comply, run full-solution tests before merging behavior changes.
:::

:::rule id="BUILD-006" category="generation"
After metamodel changes, regenerate AST code with `dotnet run --project src/ast_generator/ast_generator.csproj -- --folder src/ast-generated`. To comply, include regeneration in the same change set as the metamodel update.
:::

## Verification

:::rule id="BUILD-007" mandatory="false" category="verification"
Validate tool versions before diagnosing restore or build errors. To comply, ensure `.NET` reports `10.0.x` and Java reports 17 or newer.
:::

## Build Order Dependencies

:::rule id="BUILD-008" category="dependency"
Build through `fifthlang.sln` so dependency order is resolved correctly. To comply, avoid ad hoc partial builds when validating integration behavior.
:::

## Critical Rules

:::rule id="BUILD-009" category="workflow"
Do not cancel restore, build, test, or generation commands because long runtimes are expected. To comply, wait for completion unless the command is clearly hung.
:::

:::rule id="BUILD-010" mandatory="false" category="parser"
Do not add manual ANTLR generation to the normal workflow. To comply, rely on parser-project build targets for grammar compilation.
:::

:::rule id="BUILD-011" mandatory="false" category="generation"
Do not add manual AST generation to the normal workflow. To comply, use manual generation only for focused regeneration tasks.
:::

:::rule id="BUILD-012" mandatory="false" category="diagnostics"
Treat known baseline warnings as expected unless their pattern changes. To comply, ignore existing ANTLR `assoc`, nullable, and switch-exhaustiveness warnings unless new or altered.
:::

## Validation Protocol (after any change)

:::rule id="BUILD-013" category="validation"
Validate changes in order: build, full test, then runtime behavior. To comply, run `dotnet build fifthlang.sln`, `dotnet test fifthlang.sln`, then verify behavior manually or with integration tests.
:::

## Granular Test Targets

:::rule id="BUILD-014" mandatory="false" category="testing"
Use targeted `just` test commands for local iteration, not as the final gate. To comply, after `just test-ast`, `just test-runtime`, `just test-syntax`, or `just test-all-roslyn`, still run `dotnet test fifthlang.sln`.
:::