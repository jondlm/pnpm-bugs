# Summary (friendly bug name: air-idiom)

The following error is thrown when trying to run `pnpm --filter a add <pkg>`:
```
Package name mismatch found while reading {"integrity":"sha512-v2kDEe57lecTulaDIuNTPy3Ry4gLGJ6Z1O3vE1krgXZNrsQ+LFTGHVxVjcXPs17LhbZVGedAJv8XZ1tvj5FvSg==","registry":"https://registry.npmjs.org/","tarball":"https://registry.npmjs.org/lodash/-/lodash-4.17.21.tgz"} from the store. This means that the lockfile is broken. Expected package: lodash@4.17.20. Actual package in the store by the given integrity: lodash@4.17.21.
```

The error indicates there's a problem with the lockfile but that's incorrect.

The bug first popped up in `6.32.2` (also present in `v7.0.0-rc.3`) and
presents with the following conditions:
- `link-workspace-packages=false` in `.npmrc`
- have a workspace package named `lodash`, even though we're not actually linking to it
- have multiple versions of lodash resolved in the lockfile

When pnpm attempts to run through all the packages it [incorrectly determines][0]
that `lodash` needs to be re-resolved, even though it actually should be a
locked version. When the version resolution runs pnpm complains that it doesn't
match what was expected.

# Repro

Steps to reproduce:
`pnpm --filter a add is-negative`






[0]: https://github.com/pnpm/pnpm/blob/v7.0.0-rc.3/packages/resolve-dependencies/src/resolveDependencies.ts#L284-L290
