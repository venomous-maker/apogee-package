# Changelog

Every release of apogee-package, newest first. Each is a `vX.Y.Z` tag with a GitHub
release carrying its section; the `grav` line in `grav.toml` says which
compiler versions it builds with.

## Unreleased

## 0.1.2 (2026-11-28)

### Fixes

- A Redis connection shared by several threads (the cache, session and queue-dashboard stores, used by a multi-threaded server or queue worker) no longer mixes up replies or stalls: every command is one locked round trip (grav-redis 0.2.2). Uses the new releases of apogee-auth.

## 0.1.1 (2026-11-27)

### Fixes

- Database connections that the server dropped while idle (MySQL's `wait_timeout`, a restart, a proxy's idle limit) reconnect on their next use instead of failing with "server has gone away" (grav-db 0.2.1). Uses the new releases of grav-db.

## 0.1.0 (2026-11-25)

- `ApogeePackage` (name, defaults, after, register, boot, migrations,
  commands, enabled), `PackageContext`, and `Packages.discover` (by
  reflection, ordered by after()).

### Dependencies

- Uses apogee-http 0.3.0, apogee-routing 0.3.0, apogee-auth 0.2.2 (from their GitHub tags).
