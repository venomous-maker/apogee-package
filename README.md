# apogee-package

How an [Apogee](https://github.com/venomous-maker) module connects itself to an
app. A module declares one class extending `ApogeePackage`; importing the
module is then enough. The app (apogee-framework) finds the class by
reflection, applies its default settings, registers and boots it, runs its
migrations with `migrate`, and adds its commands to the console. Nothing is
listed by hand.

```grav
import "std"
import "apogee-package"
use apogee.*

class SanctumPackage extends ApogeePackage {
    constructor() {}
    fn name() -> string { return "sanctum" }
    fn defaults() -> HashMap<string, any> { return {"sanctum.expiration": Env.getInt("SANCTUM_EXPIRATION", 0)} }
    fn after() -> Array<string> { return ["auth"] }                 // boot after these packages
    fn migrations() -> Array<any> { return [new CreatePersonalAccessTokensTable() as any] }
    fn commands() -> Array<any> { return [new PruneTokensCommand() as any] }
    fn register(app: PackageContext) { app.container.singleton::<TokenRepository>(...) }
    fn boot(app: PackageContext) {
        let router = app.make::<Router>()
        router.aliasMiddleware("abilities", fn(a: Array<string>) -> Middleware { return new CheckAbilities(a, true) })
    }
}
```

| Hook | When |
|---|---|
| `defaults()` | first: settings the app's configuration leaves out (`.env` and `config/*.json` win) |
| `register(app)` | bindings only; other packages may not be registered yet |
| `boot(app)` | after every package registered: routes, middleware, listeners, schedules |
| `migrations()` | run by `migrate` with the app's own (grav-orbit `Migration`s) |
| `commands()` | added to the console (apogee-console `ConsoleCommand`s) |
| `after()` | names of packages that must register and boot first |
| `enabled()` | `<name>.enabled` in the configuration (true by default) turns a package off |

The contract lives here, below the framework, so a module needs only this
package (with apogee-container and apogee-config) to describe itself.

## License

MIT
