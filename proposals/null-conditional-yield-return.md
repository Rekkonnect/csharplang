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
+   : attribute_list* 'yield' ('return' | 'break') '?'? expression? ';'
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
- `dynamic`

By the current design, `T` cannot be a (function) pointer type, since it's an invalid type argument type.

It is a compiler error to use `yield return?` on an expression whose type does not meet the above criteria. For example,

```csharp
IEnumerable<int> Get()
{
    const int a = 1;

    // All of the following expressions will emit the error
    yield return? 1;
    yield return? default;
    yield return? a;
}
```

It is a compiler warning for the expression `e` to have a constant value, with the reasoning being that constant values are known to be null or not, rendering usage of the `yield return?` redundant, redirecting the programmer to use the traditional `yield return` on the provided expression. For example,

```csharp
IEnumerable<object> Get()
{
    const string s = nameof(s);

    // All of the following expressions will emit the warning
    yield return? null;
    yield return? default;
    yield return? default(object);
    yield return? "value";
    yield return? "value" + s + "s";
    yield return? $"value{s}";
}
```

If the return type is `IEnumerable<R>` and the type of the returned expression is `T`, it is valid for `T` to be equal to `Nullable<R>`, since the value will only be returned if not null. For example,
```csharp
IEnumerable<int> Get(int? a)
{
    yield return? a; // valid
}
```

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
