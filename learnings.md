# TypeScript Learnings

This is not a comprehensive list. It is a personal list
of things that I either found surprising or interesting
when taking a deep dive into TypeScript.

## Transpilation

### Transpile vs Compile

- Typescript documentation refers to running `tsc` as **compilation**.

- Most often I think compilation refers to creation of a (_significantly?_) lower-level language.

- I consider `tsc` does **transpilation** because it translates to a similarly
  high level language - JavaScript.

- For compilation, I think of things like from C to assembly language (or
  machine code) or from Java to bytecode.

### TypeScript as Superset of JavaScript

- `tsc` will output targets even if compilation fails.

- This is the default because TypeScript is a superset of JavaScript and must
  therefore support legacy JS.

- `tsc --noEmitOnError` overrides the default. With or without this switch,
  `tsc` compilation failures will still report the failure reasons and return
  an error code to the caller.

## Static vs Dynamic Typing

- Think of TypeScript as "anotating" dynamic types so that they can be
  statically analysed by `tsc`.

- TypeScript does not add any run-time type checking that isn't already
  available in JavaScript. It emits normal JavaScript code with JavaScript's
  normal dynamic typing behavior at runtime.

## Types

- Normal JS core types are available in TS. For example: number (always
  a float and covering all other types), string and boolean.

- TypeScript adds additional core types.

## Function Signatures & Implicit Returns

- TS does not only check types it checks also compliance with a function
  signature.

- It checks that the right number and type of arguments is passed.

- It checks that there is at least one return in the function if a function
  return type is declared.

- **However**, by default there is an implicit return value
  (standard JS behavior) of undefined. For example, the following code compiles
  even though not all paths return a number:

  ```typescript
  (a: number): number => {if (a) console.log(a) else return a;}
  ```

- Use `--noImplicitReturns` flag to fail if any function paths do not
  return a value.

## Type Inference

- Like C#'s `var` syntax, TS can infer the type of a variable from the
  initially assigned value.

- For example `var x: number = 5` is equivalent to `var x = 5`.

- Once inferred TS will not allow type to change later. So in JS
  `var x = 1; x = 'a';` will work. In TS it will fail.

- To force type without a value to be inferred, but to ensure it is
  enforced later, use `var x: number; x=2`.

- Same inference works for object. Both their type and structure are inferred.
  For example, the following will work in JS but not in TS.

  ```typescript
  const o = { n: 0, s: 'a' };
  o.n = 'b';
  o.z = 'c';
  ```

- Overriding object inference to a more general type is possible. For example `const o: object = { n: 0, s: 'a' };`
now treats this as a generic object. There will be no
intellisnse on members and any attempt to access a member
will fail because the transpiler only knows this is a pointer
to some object but not the object's type. It would need to
be cast to a compatible real object type to use it.

- Explicit object types can be defined like this (but better
to use inference where possible otherwise both content and
type definition would need to be refactored together):

    ```typescript
    const o: {n: number; s: string} 
        = { n: 0, s: 'a' };
    ```
