# asser-ts

An assertion declares that a condition be `true` before executing subsequent code.

- If the condition resolves to `true` the code continues running.
- If the condition resolves to `false` an error will be thrown.

## Exports

### `assert()`

Asserts that a condition is `true`, ensuring that whatever condition is being checked must be true for the remainder of the containing scope.

> **Note**: The narrow type of `boolean` is an intentional design decision.

```ts
function assert(condition: boolean, message?: string): asserts condition;
```

Other assertion functions may accept ["truthy"](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) and ["falsy"](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) conditions, while `assert` only accepts conditions that resolve to `boolean`.

The goal is to promote consideration from consumers when dealing with potentially ambiguous values like `0` or `''`, which can introduce subtle bugs.

#### Messages

The `assert` function has a default message:

```ts
let falsyValue = '';
assert(typeof falsyValue === 'string' && falsyValue.length > 0);
// → TypeError: Assert failed
```

Or you can provide a custom message:

```ts
let falsyValue = -1;
assert(falsyValue >= 0, `Expected a non-negative number, but received: ${falsyValue}`);
// → TypeError: Expected a non-negative number, but received: -1
```

### `assertNever()`

Asserts that allegedly unreachable code has been executed.

```ts
function assertNever(condition: never): never;
```

Use `assertNever` to catch logic forks that shouldn't be possible.

```ts
function doThing(type: 'draft' | 'published') {
  switch (type) {
    case 'draft':
      return; /*...*/
    case 'published':
      return; /*...*/

    default:
      assertNever(type);
  }
}
```

> **Warning**: Regardless of the condition, this function **always** throws.

```ts
doThing('archived');
// → Error: Unexpected call to assertNever: 'archived'
```

## Debugging

In **development** both `assert` and `assertNever` will include a [debugger statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger), which will pause execution to aid debugging.
