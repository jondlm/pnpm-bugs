# Summary (friendly bug name: bus-watt)

Pnpm throws the following error when running `pnpm --filter b add is-negative` (the `is-negative` can be any new package):
```
.                                        |  WARN  Package name mismatch found while reading {"integrity":"sha512-nTnJ528pbqxYanhpDYsi4Rd8MAeaBA67+RZ10CM1m3bTAVFEDcd5AuA4a6W5YkGZ1iNXHzZz8T6TBKLeBuNriQ==","registry":"https://registry.npmjs.org/","tarball":"https://registry.npmjs.org/graceful-fs/-/graceful-fs-4.2.6.tgz"} from the store. This means that the lockfile is broken. Expected package: graceful-fs@4.2.5. Actual package in the store by the given integrity: graceful-fs@4.2.6.
 ERR_PNPM_ERR_PNPM_UNEXPECTED_PKG_CONTENT_IN_STORE  The lockfile is broken! A full installation will be performed in an attempt to fix it.
.                                        |   +1 +
 ERR_PNPM_UNEXPECTED_PKG_CONTENT_IN_STORE  Package name mismatch found while reading {"integrity":"sha512-9ByhssR2fPVsNZj478qUUbKfmL0+t5BDVyjShtyZZLiK7ZDAArFFfopyOTj0M05wE2tJPisA4iTnnXl2YoPvOA==","registry":"https://registry.npmjs.org/","tarball":"https://registry.npmjs.org/graceful-fs/-/graceful-fs-4.2.10.tgz"} from the store. This means that the lockfile is broken. Expected package: graceful-fs@4.2.6. Actual package in the store by the given integrity: graceful-fs@4.2.10.
```
The error indicates there's a problem with the lockfile but that's incorrect.

The bug first popped up in `6.32.2` (also present in `v7.0.0-rc.3`) and
presents with the following conditions:
- have an optional dep in the package that is being `add`ed to
- have that optional dep be pinned to a particular lockfile version

pnpm will incorrectly attempt to resolve the optional depdency and throw the
error about a broken lockfile.
