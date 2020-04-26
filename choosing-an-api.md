TypeScript exposes many APIs, each intended for different situations.  It can be daunting to choose one.

This page attempts to list them all and explain, as best as we understand, the APIs, their use-cases, how they behave, and how they perform.

## `LanguageService`

- `ts.createLanguageService`, `ts.LanguageServiceHost`
- Accepts a `LanguageServiceHost` which gives it info about files on disk, config options.  The `Host` might be a code editor or browser-based sandbox.
- Operates on a "pull" model.
  - You query the `LanguageService` with requests, e.g "give me semantic diagnostics for this file" or "what quick-fixes are available for this file?"
  - It, in turn, queries the Host for the state of the project: filesystem calls, the contents of .ts files.
  - Every time you ask the `LanguageService` anything, it queries the Host to make sure its internal state is up-to-date.
  - When it's not up-to-date, the program is rebuilt, and typechecking is redone, on-demand.
