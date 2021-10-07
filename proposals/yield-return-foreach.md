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

The statement `yield return foreach expr;`, where `expr` is the expression whose iterated elements to yield return, is only valid if the equivalent `foreach` statement can be valid. This excludes `ref`-type elements in the enumerable expression, since `ref` types are not valid type arguments. It is a compiler error if any of the constraints are not met.

## Drawbacks

As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.

## Alternatives

The current alternative is the syntax that this statement aims to replace, as shown above.

## Other Considerations

This feature may possibly, although unlikely, interfere with other enhancements on the aspect of `yield return`. Such enhancements could be merged, generalized, or restructured accordingly.

## Unresolved Questions

- [ ] Requires LDM review
