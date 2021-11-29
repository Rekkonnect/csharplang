# Memberwise tuple operators

* [x] Proposed
* [ ] Prototype: None
* [ ] Implementation: None
* [ ] Specification: Started, below

## Summary
[summary]: #summary

Extend support for applying operators to tuples

## Motivation
[motivation]: #motivation

There already is support for memberwise equality check through usage of the `==` and `!=` operators between tuples, which standarizes behavior for memberwise operations between the tuples' contents.

## Detailed design
[design]: #detailed-design

The supported operators for tuples are:

```csharp
+
-
*
/
%
&
|
^
&&
||
!
~
++
--
```

along with all their compound variants. Applying any such operator is the equivalent of individually performing that operator to each respective member of the tuple(s). Specifically:

### Unary Operators

A unary operator (`-`, `!`, `~`, `^`, `++`, `--`) can be used on a tuple `t` of type ``System.ValueTuple`N`` only if all of its respective members support that unary operator.

### Binary Operators

Consider the tuples:
- `t`, of type `System.ValueTuple<T[]>`
- `u`, of type `System.ValueTuple<U[]>`

The notation `T[]` represents the array containing the type arguments.

A binary operator (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `&&`, `||`) is allowed between the tuples `t` and `u`, only if:
- `T.Length == U.Length`
- The operator is supported between the types `T[i]` and `U[i]`, in the encountererd order, for all valid `i`.

The result of the expression is a tuple whose individual types are the types of the result of applying the operator to the single/pair of values from each tuple. In other words, the result tuple `r` is of type `System.ValueTuple<R[]>`, where:

- `R.Length == T.Length == U.Length`
- `R[i]` is the type of the result of applying the operator between `T[i]` and `U[i]`, for all `i`.

In the case of unary operators, the above rule still applies, disregarding `u` and `U`.

## Examples

Here is demonstrated the equivalence of the current and the proposed solutions:
```csharp
var (a, b) = (5, 6);
var (c, d) = (7, 8);
var (e, f) = (a, b) + (c, d);
// equivalent to
e = a + c;
f = b + d;
```

---

Regarding testing operator support:
```csharp
(int, int) t
(string, string) u
```

`t + u` is allowed since `int + string` can be used, for each individual member pair of the tuples. The resulting tuple `r` is of type `(string, string)`, since `int + string` yields a `string`.

---

```csharp
(int, int, string) t
(int, int, int) u
```

`t - u` is not valid, as `string - int` is not valid.

## Drawbacks
[drawbacks]: #drawbacks

It must be questioned whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.

## Alternatives
[alternatives]: #alternatives

The current alternatives are either creating static/extension methods emulating that behavior hidden under a method name like `AddTuple`, or inline the operation itself and perform it manually. Doing so results in:

```csharp
// Assuming generic math is available
public static (R1, R2) Add<T1, T2, U1, U2, R1, R2>(this (T1, T2) t, (U1, U2) u)
    where T1 : IAddable<T1, U1, R1>
    where U1 : IAddable<T1, U1, R1>
    where R1 : IAddable<T1, U1, R1>
    where T2 : IAddable<T2, U2, R2>
    where U2 : IAddable<T2, U2, R2>
    where R2 : IAddable<T2, U2, R2>
{
    return (t.Item1 + u.Item1, t.Item2 + u.Item2);
}

// usage:
var (a, b) = (4, 5).Add((6, 7));
```

Or, like shown in the earlier example:
```csharp
var (a, b) = (5, 6);
var (c, d) = (7, 8);
var e = a + c;
var f = b + d;
```

## Unresolved questions
[unresolved]: #unresolved-questions

- [ ] Requires LDM review
- [ ] Should `??` and `??=` be supported?
- [ ] Does it make sense for some of the above operators to be supported for tuples?
  - They all appear convenient to have, and would play nicely and consistently with the other operators' support. There doesn't appear to be a reason against their existence.

## Design meetings

None.
