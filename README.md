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

- `async get`, `await get`, `async await` ([#4912](https://github.com/dotnet/csharplang/discussions/4912))
- Null-conditional invocation operator ([#3257](https://github.com/dotnet/csharplang/issues/3257))
- `&&=` and `||=` assignment operators ([#1718](https://github.com/dotnet/csharplang/issues/1718))
- Partial enums ([#2669](https://github.com/dotnet/csharplang/discussions/2669))
- Cached properties ([#5180](https://github.com/dotnet/csharplang/discussions/5180))
