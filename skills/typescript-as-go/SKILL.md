---
name: typescript-as-go
description: Enforce agents to write TypeScript as if it were Go; simple, explicit, boring.
---

# TypeScript As Go

## Task

Write TypeScript as if it were Go: simple, explicit, boring. When you are unsure how to write
something, ask what the dullest Go programmer would do, and do that. The goal is code that is fast to
read and review, not clever to write. No magic.

## Rules

As an agent working in this project, you must follow the following allowed and banned rules when
writing Typescript.

### Allowed

1. Pure functions first. Reach for a class only where a framework contract demands one, and keep it
   a shell: methods delegate immediately to module level functions, no logic on the class. Prefer
   composition (holding collaborators) over inheritance.
2. Named exports only. Use a default export solely when a framework requires it.
3. Small files, one concern each. File names are kebab-case (`settings-tab.ts`). Graduate to package
   folders (`settings/tab.ts`) only once a concern outgrows a single file.
4. Framework code stays thin glue. Logic lives in pure modules that never import the framework.
5. Names are short and evocative, scaled to scope: terse for locals, descriptive for exported
   symbols; a doc comment beats a longer name. Never prefix a getter with `get` (`owner()`, not
   `getOwner()`); pair it with `setOwner(value)`. Name a single method type for its role
   (`Validator`, `Formatter`). Identifiers are camelCase or PascalCase, never snake_case.
6. `type` over `interface` for data shapes. They are structs, not contracts, and a `type` cannot be
   reopened by declaration merging. Use `interface` only when implementing a framework contract
   leaves no choice.
7. String literal unions for closed sets of values (`type Status = "open" | "closed"`), so the
   syntax stays erasable at build time.
8. Keep types small, one or two members, and accept the narrowest shape a function actually uses;
   structural typing means callers need not name the type.
9. Model variant data as a discriminated union with a shared literal tag, and branch on the tag with
   a `switch` that ends in a `default` asserting the union is exhausted (`assertNever(x)`).
10. Use `satisfies` to check a value conforms to a type at compile time without widening it, the
    equivalent of Go's compile time interface check.
11. Explicit zero values: every type ships a complete default (a `DEFAULT_X` constant) that is usable
    as is without further initialization, never undefined shaped holes.
12. Distinguish absent from zero with an explicit presence check (`map.has(k)`, `k in obj`,
    `x === undefined`); never read a falsy default as "missing".
13. Errors are values. Domain logic returns a result the caller inspects (a tuple or
    `{ ok, value }`), never a sentinel like `-1`, `NaN`, `null`, or `""` folded into the normal
    return.
14. Give a modeled error structure, not just a string: carry the operation, the offending input, and
    any cause as fields. Messages are lowercase, have no trailing period, and are prefixed with their
    origin (`settings: unknown theme "$name"`), so they stay useful far from the call that produced
    them.
15. Guard clauses and early returns. Flat beats nested.
16. Braces on every `if`, even a one line body.
17. Release resources with `try/finally` or a `using` declaration placed right where the resource is
    acquired, so cleanup sits beside setup and cannot be forgotten on an early return.
18. One sentence `//` doc comment above every exported symbol, Go style
    (`// normalizeSettings returns ...`).
19. Table driven tests with `node:test` and `node:assert/strict`, no test framework dependency. Each
    test sits beside the code it covers as `name.test.ts`, mirroring Go's `_test.go` pattern.
20. Reach for a small, focused dependency when hand rolling something fiddly to get right (request
    signing, a mock server). Hand roll the moment that dependency drags in a framework or SDK you do
    not need.

### Banned

1. No enums. Use string literal unions instead.
2. No ternary expressions. Write the `if`. Cover the defaulting cases with a file local helper such
   as `stringOr(v, fallback)`.
3. No `else` after a `return`.
4. No `switch` case fallthrough. Every case ends in `return` or `break`; stack labels to share a
   branch.
5. No throwing for expected failures. Return the error as a value and convert to a result at the
   framework boundary. Reserve `throw` (Go's `panic`) for impossible states and programmer errors,
   and never expose it across a module boundary as an API.
6. No swallowed errors and no floating promises. Never discard an error or an unhandled rejection you
   could handle; always propagate or handle it.
7. No sentinel or in band return values (`-1`, `null`, `NaN`, `""`) to signal failure or absence.
8. No `interface` for plain data shapes.
9. No JSDoc `/** */` blocks. Use `//` comments.
10. No barrel files, and no `utils.ts` (or any grab bag dumping ground).
11. No inheritance for code reuse (`extends` chains). Compose instead.
12. No clever generics.
13. No decorators.
14. No magic.
