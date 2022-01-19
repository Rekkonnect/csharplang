# Implementation Tracking

The features are (mostly) ordered by discovery date, and partly grouped by relevance. There is no fixed implementation order schedule.

| Feature Description | Issue(s) | Prototype | Spec | Importance | Relevance | Expected Difficulty |
|---------------------|----------|:---------:|:----:|:----------:|:---------:|:-------------------:|
| Null-conditional operators on pointers | [#398](https://github.com/dotnet/csharplang/issues/398)<br/>[#3619](https://github.com/dotnet/csharplang/issues/3619) | [Completed](https://github.com/AlFasGD/roslyn/tree/conditional-access-pointers) | [Started](https://github.com/AlFasGD/csharplang/blob/conditional-access-pointers-spec/proposals/pointer-conditional-member-access.md) | 2.44 | 2.67 | 2.80 |
| Simplified ctor declaration | [#1928](https://github.com/dotnet/csharplang/discussions/1928) | [Completed](https://github.com/AlFasGD/roslyn/tree/features/simpler-ctor) | [Started](https://github.com/AlFasGD/csharplang/blob/simpler-ctor-spec/proposals/simpler-constructor-declarations.md) | 2.02 | 10.78 | 1.15 |
| `async get`, `await get`, `async await` | [#4912](https://github.com/dotnet/csharplang/discussions/4912) | None | [Started](https://github.com/AlFasGD/csharplang/blob/async-syntax-improvements/proposals/async-syntax-improvements.md) | 0.64 | 1.17 | 5.32 |
| Null-conditional invocation operator | [#3257](https://github.com/dotnet/csharplang/issues/3257) | None | [Started](https://github.com/AlFasGD/csharplang/blob/null-conditional-invocation/proposals/null-conditional-invocation.md) | 2.55 | 8.30 | 1.85 (4.98) |
| `&&=` and `\|\|=` assignment operators | [#1718](https://github.com/dotnet/csharplang/issues/1718) | [Completed](https://github.com/AlFasGD/roslyn/tree/features/compound-logical-operators) | [Started](https://github.com/AlFasGD/csharplang/blob/short-circ-logical-assignment-ops/proposals/short-circuit-logical-assignment-operators.md) | 1.06 | 3.76 | 1.27 |
| Partial enums | [#2669](https://github.com/dotnet/csharplang/discussions/2669) | [Completed](https://github.com/AlFasGD/roslyn/tree/features/partial-enums) | [Started](https://github.com/AlFasGD/csharplang/blob/partial-enums-spec/proposals/partial-enums.md) | 0.97 | 0.12 | 1.65 |
| Cached properties | [#5180](https://github.com/dotnet/csharplang/discussions/5180) | [In progress](https://github.com/AlFasGD/roslyn/tree/features/cached-properties) | [Started](https://github.com/AlFasGD/csharplang/blob/init-get-property-accessor/proposals/cached-properties.md) | 1.48 | 3.38 | 2.14 |
| `yield return?` | [#5063](https://github.com/dotnet/csharplang/discussions/5063) | [Completed](https://github.com/AlFasGD/roslyn/tree/conditional-yield-return) | [Started](https://github.com/AlFasGD/csharplang/blob/conditional-yield-return-spec/proposals/null-conditional-yield-return.md) | 1.18 | 2.40 | 1.56 |
| `yield return` multiple expressions | [#5098](https://github.com/dotnet/csharplang/discussions/5098) | [Completed](https://github.com/AlFasGD/roslyn/tree/features/yield-return-arglist) | [Started](https://github.com/AlFasGD/csharplang/blob/yield-return-exprlist/proposals/yield-return-expression-list.md) | 1.51 | 1.75 | 1.42 |
| `yield return foreach` | [#378](https://github.com/dotnet/csharplang/discussions/378) | None | [Started](https://github.com/AlFasGD/csharplang/blob/yield-return-foreach/proposals/yield-return-foreach.md) | 1.33 | 2.54 | 1.71 |
| Labelled loops | [#1597](https://github.com/dotnet/csharplang/issues/1597) | [Completed](https://github.com/AlFasGD/roslyn/tree/features/labelled-loops) | [Started](https://github.com/AlFasGD/csharplang/blob/labelled-loops/proposals/labelled-loops.md) | 1.73 | 2.28 | 1.97 |
| `typeof(this)`<br/>`sizeof(this)`<br/>`nameof(this)` | [#5261](https://github.com/dotnet/csharplang/discussions/5261) | [Completed](https://github.com/AlFasGD/roslyn/tree/features/this-type-operator-argument) | [Started](https://github.com/AlFasGD/csharplang/blob/this-type-operator-arguments/proposals/this-type-operator-arguments.md) | 1.11 | 3.65 | 1.50 |
| Unmanaged struct declaration | [#2036](https://github.com/dotnet/csharplang/discussions/2036) | [Completed](https://github.com/AlFasGD/roslyn/tree/features/unmanaged-struct-declarations) | [Started](https://github.com/AlFasGD/csharplang/blob/unmanaged-struct-declarations/proposals/unmanaged-struct-declarations.md) | 1.37 | 1.54 | 1.78 |
| Partial properties | [#3412](https://github.com/dotnet/csharplang/discussions/3412) | TBD | [Started](https://github.com/AlFasGD/csharplang/blob/parial-properties/proposals/partial-properties.md) | 1.78 | 0.56 | 2.02 |
| `unsafe namespace` | [#5343](https://github.com/dotnet/csharplang/discussions/5343) | [Started](https://github.com/AlFasGD/roslyn/tree/features/unsafe-namespace) | [Started](https://github.com/AlFasGD/csharplang/blob/unsafe-namespace/proposals/unsafe-namespace.md) | 1.23 | 2.89 | 0.84 |
| Memberwise operators for tuples | [#5154](https://github.com/dotnet/csharplang/discussions/5154) | None | [Started](https://github.com/AlFasGD/csharplang/blob/memberwise-tuple-operators/proposals/memberwise-tuple-operators.md) | 2.29 | 4.16 | 1.90 |
| Ref assignment for switch expressions | [#3326](https://github.com/dotnet/csharplang/issues/3326) | None | [Started](https://github.com/AlFasGD/csharplang/blob/ref-assignment-switch-expressions/proposals/ref-assignment-switch-expressions.md) | 2.78 | 5.31 | 1.63 |
| `is` pattern matching for type parameters | [#5565](https://github.com/dotnet/csharplang/discussions/5565) | None | None | 1.66 | 4.95 | 3.51 |



### Metrics Legend
All metric values are based on an exponential (base 1.5) score, and are uncapped. Bases are estimated considering existing language features, normalized such that the average metric value is 6.40 for existing language features.

- ***Importance*** refers to how impactful and desired a feature's implementation is, to its intended target users
- ***Relevance*** refers to the size of the target user group, including ones that will use it due to its availability
- ***Expected Difficulty*** refers to the difficulty of the implementations, including its estimated implementation time
