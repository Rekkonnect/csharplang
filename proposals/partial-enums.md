# Partial enums

* [x] Proposed
* [ ] Prototype: None
* [ ] Implementation: None
* [ ] Specification: Started, below

## Summary
[summary]: #summary

Supports the ability to declare enums as `partial`.

## Motivation
[motivation]: #motivation

Enums are types that are declared with a body and multiple members, like classes, structs, interfaces and records. This means that enums could be declared as `partial`, like the others do, mainly for consistency purposes. Generally, partial enum declarations could help splitting up grouped fields in the case of large enums.

## Detailed design
[design]: #detailed-design

Enums declared as `partial` benefit in the same way as other `partial`-able types benefit. This also includes access to defined fields in other declarations, and requiring that the same identifier may only be used once. Enum fields cannot be declared `partial`.

However, there is the feature in enums where the first field is automatically assigned the value 0. With multiple declarations, such behavior would introduce more harm than good. For that purpose, `partial` enums will require that at least the first field will explicitly declare a value.

Only one declaration must declare the underlying type of its contained values. Multiple declarations specifying the same underlying type is permitted. It is an error to specify a different underlying type across different declarations of the same type. Other declaration modifiers may be also omitted, if already specified by another declaration, following the same rules as declaring other `partial` types.

For example,
```csharp
public partial enum E : byte
{
    First, // error, must have assigned a value
    Second,
}

public partial enum E : short // error, underlying type does not match across declarations
{
    Third = 0,
}

public partial enum F : int { }
public partial enum F : int { } // good, underlying type matches all other partial declarations
partial enum F { } // underlying type is F, accessibility is public
```

## Drawbacks
[drawbacks]: #drawbacks

As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.

## Alternatives
[alternatives]: #alternatives

Currently, only a singled declaration for each enum type is permitted, in which case splitting up and grouping the contained fields will require usage of comments, regions, or other indicators.

## Unresolved questions
[unresolved]: #unresolved-questions

- [ ] Requires LDM review

## Design meetings

None.
