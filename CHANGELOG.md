# Changelog

All notable changes to this skill are recorded here, newest first, following semantic versioning.

## v0.4.0

- Clarified that only a *production* file outgrowing itself triggers a package folder split; tests
  always sit beside their code and never trigger it alone
- Added rule: a package folder's primary file repeats the package name (`vault/vault.ts`), never an
  `index.ts` barrel
- Added rule: never stutter the enclosing package name in an exported symbol (`vault.Reader`, not
  `vault.VaultReader`)

## v0.3.0

- Added allowed rule: the formatter (e.g. Biome), if configured, is law and decides output

## v0.2.0

- Added allowed rule: separate a trailing `return` from the block above it with a blank line
- Banned defaulting through `??` and `||`; use an `if` or a file local helper (`stringOr`,
  `numberOr`), keeping `||` only inside boolean conditions
- Banned chained array pipelines (`.filter().map()`) and `.reduce`; write an explicit `for...of`

## v0.1.0

- Initial release of the `typescript-as-go` skill, with allowed and banned rules for writing
  TypeScript in a Go style
