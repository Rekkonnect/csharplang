# `&&=` and `||=` compound assignment operators

* [x] Proposed
* [ ] Prototype: None
* [ ] Implementation: None
* [ ] Specification: Started, below

## Summary
[summary]: #summary

Supports the ability to perform compound assignemnts with the `&&` and `||` short-circuit operators.

## Motivation
[motivation]: #motivation

There already exist `&=` and `|=` that do not short-circuit. With the addition of more compound operator assignment expressions, it would feel nice to improve consistency around supported operators for compound assignment.

## Detailed design
[design]: #detailed-design

The *assignment_expression* is modified such that the new compound assignment operators are supported:

```diff
assignment_expression
- : expression ('=' | '+=' | '-=' | '*=' | '/=' | '%=' | '&=' | '^=' | '|=' | '<<=' | '>>=' | '??=') expression
+ : expression ('=' | '+=' | '-=' | '*=' | '/=' | '%=' | '&=' | '^=' | '|=' | '<<=' | '>>=' | '??=' | '&&=' | '||=') expression
  ;
```

Both the operators will be the equivalent of assigning the result of the short-circuiting logical operators. In the example:
```csharp
a = false;
a &&= ExpensiveOperation();
```

`ExpensiveOperation` will not be invoked, since `a` is already `false`, short-circuiting the operation.

The operators will short-circuit in the same fashion `&&` and `||` already do, for `&&=` and `||=` respectively.

## Drawbacks
[drawbacks]: #drawbacks

As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.

## Alternatives
[alternatives]: #alternatives

The current alternatives are to use the non-compound version of the operators, as with other operators that support compound assignment:
```csharp
a = a && b;
a = a || b;
```

Another solution is to use the non-short-circuiting versions of the operators that do support compound assignment, which could induce a high performance penalty in expensive operations:
```csharp
a &= b;
a |= b;
```

## Unresolved questions
[unresolved]: #unresolved-questions

- [ ] Requires LDM review

## Design meetings

None.
