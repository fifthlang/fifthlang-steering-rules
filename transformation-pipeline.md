---
id: steering-transformation-pipeline
title: Language Transformation Pipeline
inclusion: fileMatch
fileMatchPattern: "src/compiler/LanguageTransformations/**,src/compiler/ParserManager.cs"
---

# Language Transformation Pipeline

:::rule id="PIPE-010" category="pipeline"
Transformation passes MUST use rewriters rather than visitors. To comply, derive from `DefaultAstRewriter` or a class derived from it.
:::

:::rule id="PIPE-011" category="pipeline"
Never use Recursive descent visitors unless the AST is not going to change.  To Comply, don't derive from DefaultRecursiveDescentVisitor or a class derived from it.
:::

## Transformation Pass Order

:::rule id="PIPE-001" category="pipeline"
Apply transformation passes in the canonical order defined by `ParserManager.cs`. To comply, keep pass registration in this sequence: TreeLinkage, BuiltinInjector, ClassCtorInserter, SymbolTableBuilder, PropertyToFieldExpander, OverloadGathering, OverloadTransforming, DestructuringVisitor, DestructuringLoweringRewriter, TypeAnnotation.
:::

## Design Principles

:::rule id="PIPE-002" category="design"
Each transformation pass must have one well-defined responsibility. To comply, split passes that mix unrelated concerns.
:::

:::rule id="PIPE-003" category="dependency"
A pass may depend only on invariants established by earlier passes. To comply, place each pass after the pass that creates its prerequisites.
:::

:::rule id="PIPE-004" mandatory="false" category="design"
Prefer multiple simple passes over one mixed pass. To comply, introduce a new focused pass instead of extending an unrelated pass.
:::

:::rule id="PIPE-005" mandatory="false" category="documentation"
Document pass dependencies when one pass relies on another. To comply, record dependency assumptions in code comments or phase summaries.
:::

:::rule id="PIPE-006" category="correctness"
Every pass must preserve AST validity and type safety. To comply, add tests that fail if the pass creates invalid or untyped AST states.
:::

:::rule id="PIPE-007" mandatory="false" category="design"
Prefer AST transformations over adding complexity to code generation. To comply, lower language constructs before `LoweredAstToRoslynTranslator`.
:::

## Adding a New Transformation

:::rule id="PIPE-008" category="workflow"
A new transformation is complete only after implementation, registration, and tests. To comply, add the pass in `src/compiler/LanguageTransformations/`, register it in `src/compiler/ParserManager.cs`, add tests, then run build and full tests.
:::

## Code Generation

:::rule id="PIPE-009" category="code-generation"
Emit Roslyn syntax from lowered AST through `LoweredAstToRoslynTranslator` with debug-accurate sequence points. To comply, preserve full line-and-column mapping in generated PDB data.
:::
