# Yield return expression list

## Summary

Allows yield returning multiple expressions in one `yield return` statement.

## Motivation

Often times multiple values could be yield returned in succession, which currently requires equally as many different statements. 

## Detailed Design

The syntax for a `yield` statement is adjusted as follows:
```diff
yield_statement
- : attribute_list* 'yield' ('return' | 'break') expression? ';'
+ : attribute_list* 'yield' ('return' | 'break') (expression (',' expression)*) ';'
  ;
```

This permits passing through multiple arguments into a `yield return` statement, separated by commas. This enables merging multiple successive `yield return` statements into a single one.

Here is a demonstration of the feature:
```csharp
yield return 0, 1, 2, 3;
```

is the equivalent of the currently available:
```csharp
yield return 0;
yield return 1;
yield return 2;
yield return 3;
```

It is still required that there is at least one expression in the `yield return` statement.

## Other Considerations

This feature may possibly, although unlikely, interfere with other enhancements on the aspect of `yield return`. Such enhancements could be merged, generalized, or restructured accordingly.
