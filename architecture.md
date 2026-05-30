---
id: steering-architecture
title: Architecture Rules
inclusion: always
---

# Architecture Rules

## Dependency & Module Boundaries

:::rule id="ARCH-002" category="dependency"
Project references under `src/` must follow this DAG and must not point backward:

```text
ast-model -> ast_generator -> ast-generated -> parser -> compiler -> tests
                                               ^
                                          fifthlang.system
```

To comply, reject any `<ProjectReference>` that targets an earlier node in this order.
:::

:::rule id="ARCH-004" category="ast"
Only `src/ast-model/AstMetamodel.cs` may define AST types and inheritance. To comply, never create classes inheriting `AstThing`, `Expression`, `Statement`, or `TypeRef` anywhere but there.
:::

:::rule id="ARCH-005" category="backend"
Only `LoweredAstToRoslynTranslator` may bridge to Roslyn types. To comply, do not use `Microsoft.CodeAnalysis` in `src/compiler/LanguageTransformations/`, `src/compiler/Pipeline/Phases/`, `src/ast-model/`, or `src/ast-generated/`.
:::

## Compiler Pipeline Rules

:::rule id="ARCH-006" category="pipeline" mandatory="true"
Each phase must declare every capability it consumes via `DependsOn`. To comply, in each `ICompilerPhase`, ensure `DependsOn` covers all required AST state used by its visitors or rewriters.
:::

:::rule id="ARCH-007" category="pipeline"
Each `ICompilerPhase` must have exactly one responsibility category. To comply, if a phase performs multiple categories, split it or justify the composition in its XML summary.
:::

:::rule id="ARCH-008" category="pipeline"
A phase must not mutate the input AST after returning from `Transform`. To comply, do not store the input `ast` parameter in instance or static fields.
:::

:::rule id="ARCH-009" category="lowering"
Lowering phases must move constructs toward forms directly handled by `LoweredAstToRoslynTranslator`. To comply, output node shapes must be equal-or-lower level than input node shapes.
:::

:::rule id="ARCH-010" category="diagnostics"
Phases must emit diagnostics only through `PhaseResult.Diagnostics` or `PhaseContext.Diagnostics`. To comply, avoid `Console.Out`; allow `Console.Error` only when `DebugHelpers.DebugEnabled` is true.
:::

:::rule id="ARCH-011" category="pipeline"
`TransformationPipeline.CreateDefault()` defines a fixed phase order. To comply, keep only direct `RegisterPhase` calls in that method, with no runtime reordering logic.
:::

:::rule id="ARCH-012" category="pipeline"
Phases must not communicate through static mutable state. To comply, pass data only through AST and `PhaseContext`; the sole static exception is read-only `DebugHelpers.DebugEnabled`.
:::

## Grammar & Parser Rules

:::rule id="ARCH-013" category="parser" mandatory="true"
Only `FifthLexer.g4` and `FifthParser.g4` may define parseable Fifth syntax. To comply, `AstBuilderVisitor` may map parse trees to AST, but must not introduce syntax not accepted by the grammar.
:::

:::rule id="ARCH-014" category="parser" mandatory="true"
Semantic parser rules in `FifthParser.g4` must map one-to-one with `Visit*` methods in `AstBuilderVisitor.cs`. To comply, keep rule-name and visitor-method sets aligned, allowing ANTLR labeled alternatives.
:::

## Visitor/Rewriter Pattern Selection

:::rule id="ARCH-015" category="visitor"
Visitor base class must match the operation type.

| Operation | Base class |
|---|---|
| Read-only analysis | `BaseAstVisitor` |
| Type-preserving edits | `DefaultRecursiveDescentVisitor` |
| Cross-type rewrites or prologue insertion | `DefaultAstRewriter` |

To comply, do not mutate AST in `BaseAstVisitor`, and use `DefaultAstRewriter` when returning non-empty prologue or changing node types.
:::
