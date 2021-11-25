# Partial property declarations

## Summary

Enables flexibility around designing properties from source generators for various common patterns instead of using other members for their designation.

## Motivation

Some source generators are purposed to define properties based on some patterns, and the current workarounds include declaring private fields annotated with attributes the source generator discovers. Properties will have to be generated in one go, and it usually doesn't give the programmer the flexibility to only have a property being declared by hand.

## Detailed Design

The syntax for supporting this feature requires allowing `partial` as a declaration modifier for properties.

### Examples

A partial property may include declarations that only define its accessor definitions:
```csharp
public partial int Property { get; }
partial int Property { set; }
public partial int Property
{
    get => 1;
    private set { }
}

// Equivalent to the full declaration
public int Property
{
    get => 1;
    private set { }
}
```

Accessor and property accessibility modifiers, as well as other declaration modifiers (`static`, `abstract`, `virtual`, `override`, `sealed`) are optional for the partial declarations, much like partial type declarations. Accessiblity will be defined from the merged version of the partial declarations. Merging the declarations will result in the concatenation of all the declared modifiers for both the property itself and its accessors.

Partial properties with auto implementation may never provide an implementation declaration. This will imply that the merged version of the declartaion is intended to be an auto-property.

The following code provides an example of a partial auto-property:

```csharp
public partial int AutoProperty { get; }
partial int AutoProperty { set; }
partial int AutoProperty { get; }
partial int AutoProperty { private set; }
partial int AutoProperty { get; set; }

// Equivalent to the full declaration
public int AutoProperty { get; private set; }
```

Indexer properties can also be partially declared, with all the same rules as above applying to them.

Explicit interface implementations can be partially declared under the constraint of requiring that all accessors will be successfully declared with the proper accessibility modifiers.

```csharp
public interface I
{
    int Property { get; set; }
}

public partial class C : I
{
    private int field;
    
    partial int I.Property { get; }
    partial int I.Property { set; }
}

// in another file
partial class C
{
    partial int I.Property => field;
    partial int I.Property { set => field = value; }
}
```

### Real world example

Taken from a request, assuming an example source generator, the following code:
```csharp
[RegisterDirectProperty]
public partial int BorderThickness { get; set; }
```

would cause the generator to generate:

```csharp
public static DirectProperty BorderThicknessProperty = ...;

public partial int BorderThickness
{
    get => field;
    set => this.SetAndRaise(BorderThicknessProperty, ref field, value)
}
```

## Drawbacks

The declaration of partial properties could introduce users to more complex code which splits definition of the same component into several multiple parts, causing confusion.

## Unresolved questions

- [ ] LDM review
- [ ] Should events also be capable of being partially declared?
- [ ] There also is the case of overridable properties with only one accessor. An abstract/virtual property that only defines one accessor, cannot define another accessor in an override. Should this limitation be evaded with the introduction of partial property declarations?
