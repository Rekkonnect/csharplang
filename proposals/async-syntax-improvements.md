# Async syntax improvements

* [x] Proposed
* [ ] Prototype: None
* [ ] Implementation: None
* [ ] Specification: Started, below

## Summary
[summary]: #summary

Syntax sugar around supporting background retrieval of the result of an awaited expression without blocking the thread.

## Motivation
[motivation]: #motivation

Usually, multiple expressions need to be awaited to use their results, but `await` blocks the thread on each expression. If the user wants multiple expressions be simultaneously awaited, they can invoke the expression that yields the `Task`, and have it run in the background without the `await` expression, until the result of the `Task` is needed. This clutters code with unnecessary variables, also causing confusion by needing to find a new variable name to store the task.

## Detailed design
[design]: #detailed-design

We add a new syntax rule to support a pending result retrieval out of an awaited expression:

```antlr
background_result_retrieval_expression
  : 'async await' expression
  ;
```

The `async await` operator will initialize execution of the expression, and assign its result to the assigned expression once execution is finished. Until then, that expression is not explicitly assigned. Upon request of the expression's value, the expression is awaited.

The `async await` operator may only be used on awaitable expressions that return a result, which includes `Task<T>`, `ValueTask<T>`, and task-like types that provide a `GetResult` method.

Here is a demonstration of the new feature, compared to the already existing approach:

```csharp
int result = async await WaitResult();
int otherResult = async await WaitOtherResult();

int total = result + otherResult;
```

```csharp
var resultTask = WaitResult();
var otherResultTask = WaitOtherResult();

int result = await resultTask;
int otherResult = await otherResultTask;

int total = result + otherResult;
```

The left hand side of the assignment expression must be a local variable, otherwise unexpected outdated value issues may occur. This restriction acts as a countermeasure for requiring too deep code analysis regarding usage of the final value.

There also is the ability to enforce awaiting the expression's result before using it at some place, with an `await get` statement.

The syntax for the `await get` statement is as follows:
```antlr
await_get_statement
  : 'await get' expression_list ';'
  ;
```

Where *expression_list* is a comma-separated list of expressions. The expression list must consist of at least one expression. Only local variables that are assigned with `async await` are permitted.

For the above example, assume the following modification:
```csharp
int result = async await WaitResult();
int otherResult = async await WaitOtherResult();

// Other operations
await get result, otherResult;
// Even more operations

int total = result + otherResult;
```

The equivalent code that is currently available is the following:
```csharp
var resultTask = WaitResult();
var otherResultTask = WaitOtherResult();

// Other operations
int result = await resultTask;
int otherResult = await otherResultTask;
// Even more operations

int total = result + otherResult;
```

## Drawbacks
[drawbacks]: #drawbacks

The feature could easily permit users to clutter the thread pool without noticing, due to executing to many tasks simultaneously.

## Alternatives
[alternatives]: #alternatives

As shown above, the alternatives are retrieving the task instance, and awaiting its result right before the result is used.

## Unresolved questions
[unresolved]: #unresolved-questions

- [ ] Requires LDM review

## Design meetings

None.
