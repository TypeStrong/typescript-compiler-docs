TypeScript exposes many top-level APIs, each intended for different situations.  It can be daunting to choose one.

This page attempts to list and explain each API as best we understand. Each should include:
* use-cases: browser-based demos, CLI compilers, editor experiences, typechecking, JSON schema generators, etc
* API flow: `*Host` behavior, how data flow in/out of the compiler
* Assumptions: does the host need to implement caching?  Does the API assume an immutable filesystem?
* performance: caching, speed, etc.  When are `SourceFile`s reused, when it typechecking redone
* API entrypoints: interfaces, factory functions

## `LanguageService`

- `ts.createLanguageService`, `ts.LanguageServiceHost`
- Accepts a `LanguageServiceHost` which gives it info about files on disk, config options.  The `Host` might be a code editor or browser-based sandbox.
- Operates on a "pull" model.
  - You query the `LanguageService` with requests, e.g "give me semantic diagnostics for this file" or "what quick-fixes are available for this file?"
  - It, in turn, queries the Host for the state of the project: filesystem calls, the contents of .ts files.
  - Every time you ask the `LanguageService` anything, it queries the Host to make sure its internal state is up-to-date.
  - When it's not up-to-date, the program is rebuilt, and typechecking is redone, on-demand.

## `BuilderProgram`

- `ts.createEmitAndSemanticDiagnosticsBuilderProgram`, `ts.createSemanticDiagnosticsBuilderProgram`

## `SolutionBuilder`

## `WatchProgram`

## `IncrementalProgram`

## `executeCommandLine`

* Implements the `tsc` CLI
* Accepts a `sys` implementation (default use `ts.sys`), an argv array, and a callback to fire when it's finished.
* Not exported from `require('typescript')`; I'm not sure how to get access to it.
