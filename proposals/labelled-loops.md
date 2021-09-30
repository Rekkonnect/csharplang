# Labelled loops

* [x] Proposed
* [ ] Prototype: None
* [ ] Implementation: None
* [ ] Specification: Started, below

## Summary
[summary]: #summary

Support using labels on `break` and `continue` statements for loops.

## Motivation
[motivation]: #motivation

`goto` has a prevalent use case, which is imitating `break` and `continue` on outer loops, which is not currently supported. Generally, there has been discussion over whether `goto` should have its use cases reduced further, as it's been historically called out for encouraging bad practices.

## Detailed design
[design]: #detailed-design

The grammar rules `break` and `continue` statements will be adjusted as follows:

```diff
break_statement
- : attribute_list* 'break' ';'
+ : attribute_list* 'break' identifier_token? ';'
  ;

continue_statement
- : attribute_list* 'continue' ';'
+ : attribute_list* 'continue' identifier_token? ';'
  ;
```

`break` and `continue` may accept a label identifier, which would be a loop statement, on which the control statement will have an effect. It is an error for the referred label to not label a loop statement.

Regardless of whether the control statement is `continue`, or `break`, inner loops and other statements will be immediately broken.

There is no constraint regarding the kind of the loop statement that the control statements are used on. Neither anything about mixing kinds of loop statements.

### Break

The `break` control statement will break the labelled loop statement. This is the equivalent of placing a label after the end of the target loop, before any other following statement, and using `goto` to that label. For example,

```csharp
outer:
foreach (var x0 in x)
{
    foreach (var y0 in y)
    {
        break outer;
    }
}
```
is the equivalent of:

```csharp
foreach (var x0 in x)
{
    foreach (var y0 in y)
    {
        goto endOuter;
    }
}
endOuter:; // the ; is mandatory
```

### Continue

The `continue` control statement will force performing the next iteration of the target loop. This is the equivalent of placing a label right before the end of the target loop, after every other statement in the loop, and before its braces, and using `goto` to that label. For example,

```csharp
outer:
foreach (var x0 in x)
{
    foreach (var y0 in y)
    {
        continue outer;
    }
}
```
is the equivalent of:

```csharp
foreach (var x0 in x)
{
    foreach (var y0 in y)
    {
        goto beforeEndOuter;
    }
    beforeEndOuter:; // the ; is mandatory
}
```

This design ensures that the next iteration of the loop will properly perform the operations between iterations. This includes:
- iterating to the next element in the collection in `foreach`
- performing the operations in the last statement in a `for`
- evaluating the execution continuation expression in a `while` or `do`-`while`

## Drawbacks
[drawbacks]: #drawbacks

`goto` is being overshadowed by this feature, as this is a main use case, as already described in the motivation section. We have to ensure that we agree on following this path.

## Alternatives
[alternatives]: #alternatives

Many common practices involve using a temporary "flag" variable to determine whether a loop should be early exited. This, however, induces a slight performance degradation, by requiring a runtime check of whehter the loop must be forcefully broken, and also pollutes the code with mock feature implementations.

Another common practice is to use `goto`, where the respective target is explicitly labelled. While this approach seems cleaner to use by being more explicit about the target redirected line, many wish to avoid it only because of using `goto`.

## Unresolved questions
[unresolved]: #unresolved-questions

- [ ] Should this feature also become available to `switch`, or even more statements?
- [ ] Requires LDM review

## Design meetings

None.
