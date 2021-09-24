# Implementation Tracking

- Null-conditional operators on pointers ([#398](https://github.com/dotnet/csharplang/issues/398), [#3619](https://github.com/dotnet/csharplang/issues/3619))
  - [Prototype](https://github.com/AlFasGD/roslyn/tree/conditional-access-pointers) (completed)
    - [x] `?->`
    - [x] `?[`
    - [x] `??`
    - [x] `??=`
    - [x] `?.` (`notPointer?.pointer`)
  - [Spec](https://github.com/AlFasGD/csharplang/blob/conditional-access-pointers-spec/proposals/pointer-conditional-member-access.md) (started)
- Simplified ctor declaration ([#1928](https://github.com/dotnet/csharplang/discussions/1928))
  - [Prototype](https://github.com/AlFasGD/roslyn/tree/features/simpler-ctor) (in progress)
  - [Spec](https://github.com/AlFasGD/csharplang/blob/simpler-ctor-spec/proposals/simpler-constructor-declarations.md) (started)
- `yield return?` ([#5063](https://github.com/dotnet/csharplang/discussions/5063))
  - [Prototype](https://github.com/AlFasGD/roslyn/tree/conditional-yield-return) (completed)
  - [Spec](https://github.com/AlFasGD/csharplang/blob/conditional-yield-return-spec/proposals/null-conditional-yield-return.md) (started)

## Pending

| Feature Description | Issue | Importance | Relevance | Expected Difficulty |
|---------------------|-------|:----------:|:---------:|:-------------------:|
| `async get`, `await get`, `async await` | [#4912](https://github.com/dotnet/csharplang/discussions/4912) | 0.64 | 1.17 | 5.32 |
| Null-conditional invocation operator | [#3257](https://github.com/dotnet/csharplang/issues/3257) | 2.55 | 8.30 | 4.98 |
| `&&=` and `\|\|=` assignment operators | [#1718](https://github.com/dotnet/csharplang/issues/1718) | 1.06 | 3.76 | 1.27 |
| Partial enums | [#2669](https://github.com/dotnet/csharplang/discussions/2669) | 0.97 | 0.12 | 1.65 |
| Cached properties | [#5180](https://github.com/dotnet/csharplang/discussions/5180) | 1.48 | 3.38 | 2.14 |
| Params `yield return` | [#5098](https://github.com/dotnet/csharplang/discussions/5098) | 1.51 | 1.75 | 1.42 |

### Metrics Legend
All metric values are based on an exponential (base 1.5) score, and are uncapped. Bases are estimated considering existing language features, normalized such that the average metric value is 6.40 for existing language features.

- ***Importance*** refers to how impactful and desired a feature's implementation is, to its intended target users
- ***Relevance*** refers to the size of the target user group, including ones that will use it due to its availability
- ***Expected Difficulty*** refers to the difficulty of the implementations, including its estimated implementation time
