# Unmanaged (blittable) struct declarations

## Summary

Hints and limits the user to declare an unmanaged struct.

## Motivation

Some structs are designed to be unmanaged in nature. The user will have to know whether their declared structs are unmanaged by knowing what fields it contains, which isn't a reliable measure. There isn't any good way to tell what makes the struct unable to be considered unmanaged, other than knowing what each contained field's type is, and how to determine managed-ness.

## Detailed Design

The syntax for supporting this feature requires allowing `unmanaged` as a declaration modifier.

<!-- TODO: Add grammar modification -->

When a struct is declared `unmanaged`, any contained fields or auto-properties whose types are not unmanaged will report a compiler error. This modifier specifically requires that all contained fields and auto-properties are of unmanaged types.

Additionally, when a struct type declared with the `unmanaged` modifier is used as a type argument with the `unmanaged` constraint, or in context where an unmanaged struct is requested, it is considered valid without caring about its composition. Errors will be reported at the evaluation of the contained members, and in context outside of that, the struct is defined as unmanaged.

### `unsafe`

The `unsafe` keyword will not be required for using the `unmanaged` declaration modifier. Doing so would unnecessarily increase the verbosity of such a declaration, and `unmanaged` already is a long word. Besides, its usage doesn't require `unsafe` even in context where it currently is valid (generic type parameter constraints).

### Examples

```csharp
unmanaged struct S
{
    private int value;
    private List<int> l; // error: cannot store a managed member in an unmanaged struct
}

class Program
{
    unsafe static void Main()
    {
        var s = new S();
        S* sp = &s; // valid due to having declared the struct as unmanaged
        M<S>(); // valid for the above reason
    }
    
    static void M<T>()
        where T : unmanaged
    {
    }
}
```

## Drawbacks

As the proposal is targeted for users that already have hands-on experience with unmanaged structs, the additional minor complexity is not a concern, considered the benefits of using it.

## Unresolved questions

- [ ] Will `record struct` types support being declared with the `unmanaged` modifier?
- [ ] Requires LDM review