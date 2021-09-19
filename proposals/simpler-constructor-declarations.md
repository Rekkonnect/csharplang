# Simpler constructor declarations

* [x] Proposed
* [ ] Prototype: [In Progress](https://github.com/AlFasGD/roslyn/tree/features/simpler-ctor)
* [ ] Implementation: [In Progress](https://github.com/AlFasGD/roslyn/tree/features/simpler-ctor)
* [ ] Specification: Below

## Summary
[summary]: #summary

Constructor declartaions can be simplified by not requiring to fully type out the name of the constructed type, as it can be easily inferred.

## Motivation
[motivation]: #motivation

The current syntax for constructor declarations requires more information than necessary be typed out. The constructed type cannot be different than the type at which the constructor is being declared, rendering the requirement for explicitly specifying the constructed type unnecessary.

It also creates minor inconveniences, such as:
- longer lines
- one more change when copy-pasting constructors

## Detailed design
[design]: #detailed-design

<!-- Explaining the obvious for the sake of doing so -->

Assuming a type `T`, its constructors are declared like:
```csharp
public T() { }

public T(int a) { }

private T(int a, int b)
    : this(a) { }
```

The new syntax proposal allows substituting `T` with the keyword `new` when declaring the constructor, like so:
```csharp
public new() { }
```

This works for all modifiers that a constructor accepts (see [unresolved questions](#static-constructors)).

The grammar for the declaration of a constructor is modified to support the new syntax:

```diff
constructor_declaration
- : attribute_list* modifier* identifier_token parameter_list constructor_initializer? (block | (arrow_expression_clause ';'))
+ : attribute_list* modifier* (identifier_token | 'new') parameter_list constructor_initializer? (block | (arrow_expression_clause ';'))
  ;
```

## Drawbacks
[drawbacks]: #drawbacks

Upon implementing this feature, `new` cannot be used as a modifier in constructors like it can be used for hiding base members (if the language ever, mistakenly, chooses to support this).

Furthermore, this feature essentially removes the old constructor declaration syntax, since it's hardly preferable to use over its simplified version.

## Alternatives
[alternatives]: #alternatives

The current method of declaring constructors, `<modifiers> <type>(<arguments>)`.

## Unresolved questions
[unresolved]: #unresolved-questions

- [ ] [Static constructors](#static-constructors)
- [ ] [Destructors](#destructors)

### Static constructors
[static]: #static-constructors
Static constructors could use any of the following syntaxes:

```csharp
static new()
{
}
```

```csharp
static()
{
}
```

```csharp
static
{
}
```

Arguments over choosing which to follow are:
- The static constructor is not really an instance constructor, rather a static member initializer block (as Java refers to it). Therefore, the `new` keyword being used there doesn't fit much.
- The static constructor never takes any arguments anyway, so parentheses only serve the purpose of indicating that an invocable member is being declared.

### Destructors
[destructors]: #destructors

Destructors use a slightly different syntax. Their declaration is:
```csharp
~T()
{
}
```

They do not accept any modifiers, the type is followed by a `~`, and the arguments list is always empty.

Following the same pattern as above, simplifying the syntax could result in the following scenario:
```csharp
~new() { }
```

Although the `~` symbol is notorious for its negation, `~new` still feels like an expression that could be boiled down to something more concise. Ideally, it should be a keyword that denotes destruction, whilst attempting to avoid creating a new one.

A hardly satisfying idea that meets the criteria could be `finally`, which somewhat indicates a destructor, which is an object finalizer.

Another keyword that could take place could be `out`, which is short, and conveys the meaning of disassociating with something. However, seeing it placed where a destructor should be, could appear confusing.

## Design meetings

None.
