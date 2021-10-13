# Self type operators `typeof`, `sizeof`, `nameof`

## Summary

Allows the self type to be used as an argument in the `typeof`, `sizeof`, `nameof` operators.

## Motivation

The self type is used in certain scenarios where its type, size or name is requested. The inability to simplify using `this` to refer to the type often results in longer code, and referral to the self type is less apparent.

## Detailed Design

The syntax for supporting this feature requires allowing `this` as an argument for the operators `typeof`, `sizeof` and `nameof`.

The feature effectively replaces the following code:
```csharp
struct SomeStruct
{
    void M()
    {
        var thisType = typeof(this);
        var thisSize = sizeof(this);
        var thisName = nameof(this);
    }
}
```

with the following:
```csharp
struct SomeStruct
{
    void M()
    {
        var thisType = typeof(SomeStruct);
        var thisSize = sizeof(SomeStruct);
        var thisName = nameof(SomeStruct);
    }
}
```

Specifically in the case of `nameof`, passing `this` as its argument no longer produces the "expression has no name" error. Additionally, being in static context does not prohibit usage of the operator, the `this` type substitution is permitted in that case.

For the needs of `typeof` and `sizeof`, a new syntax node of kind `ThisTypeSyntax` is created. This ensures that `this` refers to the self type in that context, and is not an expression.

## Drawbacks

The `nameof(this)` case could confuse some users due to the operator indicating retrieving the name of the expression, if available. While `this` doesn't have a name as an expression, enabling its usage could hint that `this` is returned instead of the name of the containing type.
