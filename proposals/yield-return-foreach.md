# Yield return enumerable

## Summary

Allows yield returning an enumerable's iterated values.

## Motivation

Often times multiple values could be yield returned from an enumerable in the order they're iterated, which currently requires a hand-written `foreach` statement.

## Detailed Design

The syntax for the yield return enumerable statement is:
```antlr
yield_return_enumerable_statement
  : attribute_list* 'yield return foreach' expression ';'
  ;
```

This permits passing through an enumerable of arguments into a `yield return` statement.

Here is a demonstration of the feature:
```csharp
yield return foreach values;
```

is the equivalent of the currently available:
```csharp
foreach (var value in values)
    yield return value;
```

The statement `yield return foreach expr;`, where `expr` is the expression whose iterated elements to yield return, is valid only under the following constraints:

Assume `E`, the type of the expression, and `R` the return element type of the method.

- If `E` is `dynamic`:
  - if the runtime type of the expression exposes a `GetEnumerator()` method, returning `IEnumerator<T>`, where `T` is any type, and convertible to `R`, the operation will be successfully performed, by enumerating the result and yield returning every converted enumerated instance.
  - otherwise, a runtime error occurs.
- Otherwise, `E` must be convertible into a type that exposes a `GetEnumerator()` method, or have an applicable extension method under the given signature. The method should return `IEnumerator<T>`, with `T` being convertible to `R`.

It is a compiler error if `E` is a non-`dynamic` type and does not meet the above criteria.

## Drawbacks

As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.

## Alternatives

The current alternative is the syntax that this statement aims to replace, as shown above.

## Other Considerations

This feature may possibly, although unlikely, interfere with other enhancements on the aspect of `yield return`. Such enhancements could be merged, generalized, or restructured accordingly.

## Unresolved Questions

- [ ] Requires LDM review
