# Null-conditional invocation

* [x] Proposed
* [ ] Prototype: None
* [ ] Implementation: None
* [ ] Specification: Started, below

## Summary
[summary]: #summary

Support an expression of the form `invocable?()`, where `invocable` is a nullable invocable instance (delegate or function pointer).

## Motivation
[motivation]: #motivation

It is common practice to invoke instances (usually through events), after checking if they're `null`. There already exists special shorthand syntax for invoking the instance, without needing to do `.Invoke`, which however the user resorts to doing if they want to perform a nullity check in-place.

## Detailed design
[design]: #detailed-design

We add a new syntax rule to support null-conditional invocations:

```antlr
null_conditional_invocation_expression
  : expression '?' argument_list
  ;
```

The null-conditional invocation operator invokes its operand only if that operand is non-null.

More specifically, an expression of the form `invocable?(args)` is the equivalent of the following:
- if `invocable` returns `void` upon invocation:
```csharp
if (invocable is not null)
    invocable(args); 
```
- otherwise:
```csharp
invocable is null ? null : invocable(args);
```

The null-conditional invocation operation's operand must be an instance of an invocable type, either a delegate, or a function pointer. It must not be a method group, since, by definition, they cannot be `null`.

## Important: Ambiguity Issues
[ambiguity]: #ambiguity-issues

The proposed syntax, due to introducing the `?` symbol before a `(` symbol, interferes with the conditional operator `a ? b : c`. Expressions like the following are ambiguous:

```csharp
a?(b)?(c):d
```

In the above example, the expression could be either of the following:

```csharp
a ? ((b)?(c)) : d
(a?(b)) ? (c) : d
```

These kinds of expressions will require extra checking on the parser's end to ensure lack of ambiguity. In the case of ambiguity, a respective compiler error is emitted to hint the user to further (un-)parenthesize expressions appropriately to resolve the syntax. Trivia will **not** be taken into account for ambiguity resolution.

The general rule of causing an ambiguity is:<br/>
Consider the span S of the expression, defined as the span between the first occurrence of a `?(`, and following all subsequent `:` without encoutering a `?(`, picking the last one. In other words, it is the match of the pseudo-regex:
```
open = ?\w*(
(?'openings'open(^\:)*)+(?'closings'(^open)*\:)+
```
Ambiguity is recognized if the number of captures of `openings` and `closings` are not equal.

Here are some examples regrading ambiguity recognition:
```csharp
// Not ambiguous
a?(b)?(c)
(a?(b))?(c)

// Ambiguous
a?(b)?(c):d

// Ambiguous
a?(b)?(c):d?(e)

// Not ambiguous
a?(b)?(c):d:e
a ? (b ? c : d) : e

// Ambiguous
a?(b):c?(d)?(e):f

// Not ambiguous
a?(b):c?(d):e
a ? b : (c ? d : e)
```

## Drawbacks
[drawbacks]: #drawbacks

The ambiguity issue, as described above, could be considered a major drawback, due to requiring extra parsing effort to determine the expression, and the intricate details over resolution of the actual expression.

## Alternatives
[alternatives]: #alternatives

As it currently stands, uses of this operator can often be replaced by an expression like `e?.Invoke(...)`, or an expression something like `(e == null) ? null : e(...)` or a statement like `if (e != null) e(...)`.

## Unresolved questions
[unresolved]: #unresolved-questions

- [ ] Requires LDM review
- [ ] Should there be any ambiguous syntax resolutions in the binder itself?
  - The interference issue is only during syntax parsing, the binder can resolve this ambiguity since there are no type collisions between a `bool` type and an invocable type.

## Design meetings

None.
