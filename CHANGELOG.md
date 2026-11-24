# Changelog

Every release of apogee-package, newest first. Each is a `vX.Y.Z` tag with a GitHub
release carrying its section; the `grav` line in `grav.toml` says which
compiler versions it builds with.

## Unreleased

- `ApogeePackage` (name, defaults, after, register, boot, migrations,
  commands, enabled), `PackageContext`, and `Packages.discover` (by
  reflection, ordered by after()).
