# Cached Properties

## Summary
This proposal provides a simplified syntax for caching property values that are only meant to be initialized once.

## Motivation
It has been a frequent case of needing to cache the result of an operation that a property performs, which is currently available through the usage of a nullable value, or a `Lazy<T>` initializer.

## Detailed Design

### Cached property declaration syntax
A property may be declared as having its result cached with an `init get` accessor.

```csharp
public int Property
{
    init get
    {
        return ExpensiveOperation();
    }
}
```

The `init get` accessor declares that a property's value is cached, if it has been fetched. It's the equivalent to `Lazy<T>` but without having to initialize a new instance of the type, neither allocating a delegate for its calculation. Instead, a new `internal` runtime type is being used for the backing field.

### Backing Field Type
In all of the following examples, the produced code will have special generated names, instead of the shown. The given names are used for clarity.

The backing field type is assumed to be declared as follows:
```csharp
internal struct CachedPropertyValue<T>
{
    private T cached;

    public bool HasValue { get; private set; }
    public T Value
    {
        get 
        {
            Debug.Assert(HasValue);
            return cached;
        }
        set
        {
            HasValue = true;
            cached = value;
        }
    }
}
```

### Equivalence
The above example is compiled down to the following:
```csharp
private readonly CachedPropertyValue<int> cached = new();

public int Property
{
    get
    {
        if (cached.HasValue)
            return cached.Value;
        
        return cached.Value = GetProperty();
    }
}

private int GetProperty()
{
    return ExpensiveOperation();
}
```

For properties with pointer types, the results are cast from and into `nint`, which is a permitted type argument.

```csharp
public int*** RewrittenProperty
{
    get
    {
        if (cached.HasValue)
            return (int***)cached.Value;
        
        // temporary variable declaration and method name for value type clarity
        int*** result = GetPointer();
        return (int***)(cached.Value = (nint)result);
    }
}
```
In the event of a property being `abstract`, `virtual`, `override` or `sealed`, the accessibility of the equivalent backing method to calculate the cached value (`GetProperty` from the above example) will become `protected`, and have the respective modifiers upon declaring the cached property. However, the backing field and the property will not have those modifiers, they are strictly passed into the backing method.

For example, the property
```csharp
public abstract int Property { init get; }
```

will become:
```csharp
private readonly CachedPropertyValue<int> cached = new();

public int Property
{
    get
    {
        if (cached.HasValue)
            return cached.Value;
        
        return cached.Value = GetProperty();
    }
}

protected abstract int GetProperty();
```

Then, the implementation of the property, as in example:
```csharp
public override int Property { init get => 1; }
```

will be transformed into:
```csharp
protected override int GetProperty() => 1;
```

without creating any new cached value field.

For `static` properties, `init get` is also supported, but is treated slightly differently to support that. All generated members (the backing field, the actual property, and the method that retrieves the value to be cached) are added the `static` modifier. Other modifiers (`abstract`, `virtual`, `override`, `sealed`), and accessiblity, are unaffected.

Additionally, every type inheriting a static property contains a new backing field, overrides the base property, and the base retrieval method.

For example:
```csharp
public static abstract int MaxValue { init get; }
```

will become:
```csharp
public static abstract int MaxValue { get; }

public static abstract int GetMaxValue();
```

And a type inheriting that property like so:
```csharp
public static override int MaxValue { init get => int.MaxValue; }
```

produces:
```csharp
private static readonly CachedPropertyValue<int> cached = new();

public static override int MaxValue
{
    get
    {
        if (cached.HasValue)
            return cached.Value;
        
        return cached.Value = GetMaxValue();
    }
}

public static override int GetMaxValue() => int.MaxValue;
```

### Usage Restrictions
- Properties with an `init get` accessor may not have any other accessors declared (`get`, `set`, `init`).
- `init get` accessors must declare a body (unless `abstract`).
- Indexer properties cannot contain `init get` accessors.
  - This decision was made due to the complication of needing to cache the argument values and map them to the returned values. Overall, the induced overhead of mapping values does not easily justify the cached operation result's complexity, and thus it's better to not provide the programmer the ability to take a risk of reducing performance from a feature that intends to shorten the process of improving performance.

## Alternatives
The same can be achieved through usage of `Lazy<T>`, or a `null` value to represent uninitialized cache. An example alternative can be:

```csharp
private T cached;
public T Property => cached ??= GetProperty();

private T GetProperty() => ...;
```

## Drawbacks
This new feature mixes usage of the `init` and the `get` accessor. Seeing `init get` could be confusing as its intention is not clear on its own.

## Unresolved Questions

- Should the `init` part of the `init get` be considered a modifier keyword?