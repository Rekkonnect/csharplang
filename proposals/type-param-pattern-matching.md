# Type pattern matching for type parameters

* [x] Proposed
* [ ] Prototype: None
* [ ] Implementation: None
* [ ] Specification: Started, below

## Summary
[summary]: #summary

This proposal enables type parameter pattern matching, using existing syntax with type pattern matching.

## Motivation
[motivation]: #motivation

Although not so frequent, the existence of `is` pattern matching applying for types has common grounds with matching type parameters, which currently doesn't have any special syntax or semantics.

## Detailed design
[design]: #detailed-design

Type parameter pattern matching enables using type parameters as the matched expression, with the matched expressions only being types. This means that usage of the `typeof` operator can be reduced, or even eliminated during comparison of types.

Here is an example of a type parameter pattern matching: 
```csharp
public static bool IsUnsignedNumericType<TOther>()
{
    return TOther is byte or ushort or uint or ulong;
}
```
The currently available that the above is equivalent to is:
```csharp
public static bool IsUnsignedNumericType<TOther>()
{
    return typeof(TOther) == typeof(byte)
        || typeof(TOther) == typeof(ushort)
        || typeof(TOther) == typeof(uint)
        || typeof(TOther) == typeof(ulong);
}
```

Note that the above usage of `is` translates to equality between two types, only because the types are sealed. In the case of unsealed types, this would translate to:
```csharp
typeof(TOther) == typeof(NotSealed) ||
typeof(TOther).IsSubclassOf(typeof(NotSealed));
```

The `and` and `not` keywords are also available to use, applying the same semantics in the pattern matching. `not` would invert the above behavior of `is`, meaning that the type must be neither the same type, nor a derived type. In other words, `A is not B` means that `A != B` and `not A : B`, translated into the following code:
```csharp
typeof(A) != typeof(B) &&
!typeof(A).IsSubclassOf(typeof(B));
```

The pattern matching is available to the following types:
- simple types (class, struct, interface, delegate, enum)
- bound generic types (see expansion notes below)
- array types
- nullable structs (using the nullable type syntax)
- tuple types (using the tuple type syntax)

In the above order, here are examples of applying those types:
```csharp
return TOther
    is SomeClass
    or int
    or IEnumerable
    or Action
    or DayOfWeek
    or List<int>
    or int[]
    or int?
    or (int, int);
```

Pointer and function pointer types are not supported, since as of the time being, pointer types are not valid type arguments, thus the proposal would never touch that area.

## Expansions

### Generic type matching using open generic types
[generics]: #generics

A type could only be evaluated to match a given unbound generic type, disregarding its type parameters. For example, such a pattern could be:
```csharp
T is List<>
```
indicating that the type `T` would have to be any type that inherits from `List<T>`, without regarding its type parameter.

Additionally, this could open up using the `{ ... }` syntax for constraints around the type parameters of the matched type itself. In the above example, if we wanted to exclude `List<int>` and `List<long>`, we could write:
```csharp
T is List<> { T: not int and not long }
```
For clarification, the left `T` refers to the type parameter in the scope, and the right `T` refers to the type parameter of the `List<T>` type.

### Direct equality of types

The `is` operator does not actually mean equality in this case, just as it does with normal type pattern matching. Ideally, there should be some way to simulate the `typeof(T) == typeof(Other)` expression.

An easily ruled out proposing syntax for this is to permit inverting the ordering of the types and write:
```csharp
T is Other && Other is T
```
indicating that both `Order` and `T` would have to extend each other, which is reduced to simple equality. Such a workaround is inconvenient and defeats the purpose of this proposal.

The idea of `T` being a base type of a more specialized one, excluding the above case of equality, does not have a place, since:
- the base type of a specified type is available to statically type
- the `not` keyword can be used to exclude types deriving from the given type (`T is not Order`)

As we're referring to equality, the `==` operator could also be used to indicate type equality.

### `switch` statements and expressions

From the introduction of this proposal, type parameter matching could also be expanded to apply to `switch` statements and expressions, much like other language features. Such a proposal will have its own issue and spec file, as this current one is focused on the pattern matching aspect.

## Drawbacks
[drawbacks]: #drawbacks

As already hinted in the motivation section, this feature will not be beneficial for everyday purposes. It's a rather niche case, and the cost of implementing it is estimated to be high.

## Alternatives
[alternatives]: #alternatives

The current alternative involves directly using the `typeof` operator and a series of comparisons, like the following:
```csharp
public static bool IsUnsignedNumericType<TOther>()
{
    return typeof(TOther) == typeof(byte)
        || typeof(TOther) == typeof(ushort)
        || typeof(TOther) == typeof(uint)
        || typeof(TOther) == typeof(ulong);
}
```

## Unresolved questions
[unresolved]: #unresolved-questions

- [ ] Requires LDM review

## Design meetings

None.
