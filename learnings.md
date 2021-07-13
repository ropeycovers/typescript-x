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
