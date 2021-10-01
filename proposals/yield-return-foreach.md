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

The `yield return foreach` statement may only be applied on an object of type `IEnumerable<T>`, or `dynamic`. In the case of `dynamic`:
- if the runtime type of the object is `IEnumerable<T>`, the operation will be successfully performed, by iterating through the object's contents and yield returning every iterated instance of it
- otherwise, a runtime error occurs

Assuming that the iterated enumerable contains elements of type `T`, and the returned element type is `R`, the same type conversion rules apply for `T` into `R`.

## Drawbacks

As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.

## Alternatives

The current alternative is the syntax that this statement aims to replace, as shown above.

## Other Considerations

This feature may possibly, although unlikely, interfere with other enhancements on the aspect of `yield return`. Such enhancements could be merged, generalized, or restructured accordingly.

## Unresolved Questions

- [ ] Requires LDM review
