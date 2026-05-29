---
id: steering-fifth-language-syntax
title: Fifth Language Syntax Reference
inclusion: manual
---

# Fifth Language Syntax Reference

## Basic Syntax

:::rule id="SYN-001" mandatory="true" category="syntax"
Use canonical Fifth syntax in examples. To comply, use forms such as:

```fifth
class Person {
    Name: string;
    Height: float;
}

main(): int
{
  myprint(5 + 6);
  return 0;  
}
myprint(int x): int
{
  std.print(x);
}
```
:::

## Variable Declarations

:::rule id="SYN-002" category="syntax"
Declare variables in `name: type = value` form. To comply, use declarations such as:

```fifth
x: int = 42;
g: graph = KG.CreateGraph();
```
:::

## Function Definitions

:::rule id="SYN-003" category="functions"
Always define functions with block bodies. To comply, use forms such as:

```fifth
greet(string name) {
    std.print("Hello " + name);
}
```
:::

## Parameter Constraints (Guards)

:::rule id="SYN-004" category="guards"
Write guards as parameter constraints, not `when` clauses. To comply, write guards like:

```fifth
myprint(int x | x == 0) { std.print(x); }
```
:::

## Knowledge Graph Constructs

:::rule id="SYN-005" category="knowledge-graph"
Use canonical store and graph forms for knowledge-graph code. To comply, use forms such as:

```fifth
myStore: store = sparql_store(<http://example.org/store>);
g: graph = KG.CreateGraph();
```
:::

## TriG and SPARQL Literals

:::rule id="SYN-006" category="knowledge-graph"
Use canonical literal syntax for TriG and SPARQL values. To comply, use `<{...}>` for TriG, `?<...>` for SPARQL, and supported scalar types only for object literals.
:::

## Sample Files

:::rule id="SYN-007" category="reference"
Use repository sample paths as canonical syntax references. To comply, prefer `test/ast-tests/CodeSamples/*.5th`, `src/parser/grammar/test_samples/*.5th`, and `docs/Getting-Started/`.
:::