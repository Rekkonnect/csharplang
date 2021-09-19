# Null-conditional yield return

* [x] Proposed
* [ ] Prototype: None
* [ ] Implementation: None
* [ ] Specification: Started, below

## Summary
[summary]: #summary

Support an expression of the form `yield return? e`, which, in yield returning functions, yields the value `e` if not null, otherwise performs no action.

## Motivation
[motivation]: #motivation

This is a common coding pattern, and this feature would have nice synergy with the existing null-propagating and null-coalescing operators.

## Detailed design
[design]: #detailed-design

The grammar rule *yield_statement* is adjusted accordingly:

```diff
yield_statement
-   : attribute_list* 'yield' ('return' | 'break') expression? ';'
+   : attribute_list* 'yield' ('return' '?'? | 'break') expression? ';'
    ;
```

The null-conditional `yield return` operator executes a `yield return` operation only if the given value is non-null. In general, `yield return? e;` is equivalent to:
```csharp
if (e is not null)
    yield return e;
```

The type of `e`, which we will refer to as `T`, must be a value that accepts the literal `null` value. This means that `T` must be one of the following types:
- reference type
- nullable value type (`Nullable<>`)

By the current design, `T` cannot be a (function) pointer type, since it's an invalid type argument type.

It is a compiler error to use `yield return?` on an expression whose type does not meet the above criteria.

## Drawbacks
[drawbacks]: #drawbacks

As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.

## Alternatives
[alternatives]: #alternatives

Although it requires some boilerplate code, uses of this operator can often be replaced by a statement like `if (e != null) yield return e`.

## Unresolved questions
[unresolved]: #unresolved-questions

- [ ] Requires LDM review

## Design meetings

None.
