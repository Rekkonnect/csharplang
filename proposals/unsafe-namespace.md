# Partial property declarations

## Summary

Allows the `unsafe` modifier to be used in front of a `namespace` declaration.

## Motivation

There are some cases where unsafe code across multiple types is bundled in a single file, requiring that `unsafe` be used for every single one of them. Almost all types are declared within a namespace, and not the global one. This permits the user to essentially control the code that's considered unsafe easier.

## Detailed Design

The syntax for supporting this feature requires allowing `unsafe` as a declaration modifier for namespaces.

### Examples

An unsafe namespace declaration enables unsafe code in the block the namespace is valid on (***not** the entire namespace*):
```csharp
// -- File0.cs
unsafe namespace N;

public class C
{
   public void M(void* ptr) { } // valid
}

// -- File1.cs
namespace N
{
    public class D
    {
        public void M(void* ptr) { } // error; requires unsafe modifier
    }
}
unsafe namespace N
{
    public class E
    {
        public void M(void* ptr) { } // valid
    }
}
```

## Drawbacks

The `unsafe` modifier in front of the namespace could confuse users into believing it applies to the whole namespace instead of just the single block it contains. This will be even more confusing for file-scoped namespace declarations.

However, `unsafe` already ensures that experienced users are getting their hands on it. Those said experienced users are already aware of specific details that language constructs result into. This should not be a problem other than beginners stumbling upon stranger code and being unable to figure out what permits its usage.

## Unresolved questions

- [ ] LDM review
- [ ] CS1671 will inherently be changed after this change. Should it be split into two parts, one for attributes, and another for modifiers?
