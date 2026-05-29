---
id: steering-code-generation
title: AST Code Generation Rules
inclusion: fileMatch
fileMatchPattern: "src/ast-model/**,src/ast-generated/**,src/ast_generator/**"
---

# AST Code Generation Rules

## Generator-as-Source-of-Truth

:::rule id="GEN-001" category="generation"
Treat `src/ast-generated/` as generator output only. To comply, never hand-edit files in `src/ast-generated/`.
:::

:::rule id="GEN-002" mandatory="false" category="generation"
Keep generated outputs in their canonical files. To comply, emit builders to `builders.generated.cs`, visitors to `visitors.generated.cs`, rewriters to `rewriter.generated.cs`, and type inference helpers to `typeinference.generated.cs`.
:::

## How to Change Generated Code

:::rule id="GEN-003" category="workflow"
Change generated output by editing source inputs, then regenerating. To comply, update `src/ast-model/AstMetamodel.cs` or templates, run `just run-generator`, then run `dotnet build fifthlang.sln`.
:::

## AST Design

:::rule id="GEN-004" mandatory="false" category="design"
Keep `AstMetamodel.cs` focused on high-level language constructs. To comply, represent source-level features in the metamodel and lower them in transformation passes.
:::

## Visitor/Rewriter Pattern Selection

:::rule id="GEN-005" category="visitor"
Use `BaseAstVisitor` only for read-only analysis. To comply, use it for symbol collection, diagnostics, or validation, and do not mutate AST nodes.
:::

:::rule id="GEN-006" category="visitor"
Use `DefaultRecursiveDescentVisitor` only for type-preserving AST edits. To comply, keep input and output node types the same and avoid statement hoisting.
:::

:::rule id="GEN-007" category="visitor"
Use `DefaultAstRewriter` for lowering that changes node shape or inserts statements. To comply, choose this pattern when returning `RewriteResult` with prologue, adding temporaries, or doing cross-type rewrites.
:::

:::rule id="GEN-008" mandatory="false" category="reference"
Use `src/ast_generator/README.md` as the detailed reference for pattern selection. To comply, verify the selected visitor or rewriter pattern against that guide before adding a new pass.
:::

## PR Requirements

:::rule id="GEN-009" category="review"
A pull request that changes `src/ast-generated/` must show the source change that produced it. To comply, include metamodel or template edits, the regeneration command, and confirmation that generated files were not hand-edited.
:::