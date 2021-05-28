---
created: 2021-05-29T00:30:15 (UTC +08:00)
tags: []
source: https://www.haskellforall.com/2021/05/module-organization-guidelines-for.html
author: 
---

# Haskell for all: Module organization guidelines for Haskell projects

> ## Excerpt
> modules          This post collects a random assortment of guidelines I commonly share for how to organize Haskell projects.  ...

---
   modules 

This post collects a random assortment of guidelines I commonly share for how to organize Haskell projects.

#### Organize modules “vertically”, not “horizontally”

The glib summary of this rule is: don’t create a “Types” or “Constants” module.

“Vertically” organized modules are modules that group related functionality within the same module. For example, vertically-oriented modules for a simple interpreter might be:

-   A `Syntax` module
    
    … which contains the concrete syntax tree type and utilities for traversing, viewing, or editing that tree.
    
-   A `Parsing` module
    
    … which contains (or imports/re-exports) the parser type, a parser for the syntax tree, and error messages specific to parsing.
    
-   An `Infer` module
    
    … which contains the the type inference logic, exception types, and error messages specific to type-checking.
    
-   An `Evaluation` module
    
    … which logic for evaluating an expression, including possibly a separate data structure for a fully-evaluated abstract syntax tree.
    
-   A `Pretty` module
    
    … which specifies how to pretty-print or otherwise format expressions for display.
    

“Horizontally” organized modules mean that you organize code into modules based on which language features or imports the code relies upon. For example, horizontally-oriented modules for the same interpreter might be:

-   A `Types` module
    
    … which contains almost all types, including the concrete syntax tree, abstract syntax tree, parsing-related types, and exception types.
    
-   A `Lib` module
    
    … which contains almost all functions, including the parsers, type inference code, evaluator, and prettyprinter.
    
-   A `Constants` module
    
    … which contains almost all constants (including all error messages, timeouts, and help text).
    
-   An `App` module
    
    … which contains the `main` entry point for the program.
    

There are a few reasons I prefer vertical module organization over horizontal module organization:

-   Vertically-organized modules are easier to split into smaller packages
    
    For example, in a vertically-organized project I could separate out the `Syntax`, `Parser`, and `Pretty` modules into a separate standalone package, which could be used by other people to create an automatic code formatter for my language without having to depend on the type-checking or evaluation logic.
    
    Conversely, there would be little benefit in separating out a `Types` module from the equivalent horizontally-organized package. Typically, horizontal modules are so tightly coupled to one another that no subset of the modules is useful in isolation.
    
    The separability of vertical modules is an even bigger feature for proprietary projects that aspire to eventually open source their work. Vertically-organized projects are easier to open source a few modules at a time while keeping the proprietary pieces internal.
    
-   Vertically-organized modules tend to promote more granular and incremental build graphs
    
    In a horizontally-organized project, each new type you add to the `Types` module forces a rebuild of the entire package. In a vertically-organized project, if I completely rewrite the type-checking logic then only other modules that depend on type-checking will rebuild (and typically very few depend on type-checking).
    
-   Vertically-organized modules tend to group related changes
    
    A common issue in a horizontally-organized project is that every change touches almost every module, making new contributions harder and making related functionality more difficult to discover. In a vertically-organized project related changes tend to fall within the same module.
    

#### Naming conventions

I like to use the convention that the default module to import is the same as the package name, except replacing `-` with `.` and capitalizing words.

For example, if your package name is `foo-bar-baz`, then the default module the user imports to use your package is `Foo.Bar.Baz`.

Packages following this module naming convention typically do not have module names beginning with `Control.` or `Data.` prefixes (unless the package name happens to begin with a `control-` or `data-` prefix).

There are a few reasons I suggest this convention:

-   Users can easily infer which module to import from the package name
    
-   It tends to lead to shorter module names
    
    For example, the `prettyprinter` package recently switched to this idiom, which changed the default import from `Data.Text.Prettyprint.Doc` to `Prettyprinter`.
    
-   It reduces module naming clashes between packages
    
    The reasoning is that you are already claiming global namespace when naming a package, so should why not also globally reserve the module of the same name, too?
    
    However, this won’t completely eliminate the potential for name clashes for other non-default modules that your package exports.
    

#### The “God” library stanza

This is a tip **for proprietary projects only**: put all of your project’s code into one giant library stanza in your `.cabal` file, including the entrypoint logic (like your command-line interface), tests, and benchmarks. Then every other stanza in the `.cabal` file (i.e. the executables, test suites, and benchmarks) should just be a thin wrapper around something exported from one of your “library” modules.

For example, suppose that your package is named `foo` which builds an executable named `bar`. Your `foo.cabal` file would look like this (with only the relevant parts shown):

```
name: foo
…

library
  hs-source-dirs: src
  exposed-modules:
    Foo.Bar
  …

executable bar
  hs-source-dirs: bar
  main-is: Main.hs
  …
```

… where `src/Foo/Bar.hs` would look like this:

```
-- ./src/Foo/Bar.hs
module Foo.Bar where

…

main :: IO ()
main = … -- Your real `main` goes here
```

… and `bar/Main.hs` is a trivial wrapper around `Foo.Bar.main`:

```
-- ./bar/Main.hs

module Main where

import qualified Foo.Bar

main :: IO ()
main = Foo.Bar.main
```

This tip specifically works around `cabal repl`’s poor support for handling changes that span multiple project stanzas (both `cabal v1-repl` and `cabal v2-repl` appear to have the problem).

To illustrate the issue, suppose that you use `cabal repl` to load the executable logic for the project like this:

```
$ cabal repl exe:bar
…
*Main>
```

Now if you change the `Foo.Bar` module and `:reload` the REPL, the REPL will not reflect the changes you made. This is a pain whenever you need to test changes that span your library and an executable, your library and a test suite, or your library and a benchmark.

Also, if you load an executable / test suite / benchmark into the REPL that depends on a separate library stanza then `cabal repl` will force you to generate object code for the library stanza, which is slow. Contrast that with using `cabal repl` to only load the library stanza, which will be faster because it won’t generate object code.

Moreover, `ghcid` uses `cabal repl` to power its fast type-checking loop, which means that `ghcid` also does not work well if you need to quickly switch between changes to the library stanza and other project stanzas.

The fix to all of these problems is: put all of your project’s logic into the library stanza and use only the library stanza as basis for your interactive development. Everything else (your executables, your test suites, and your benchmarks) is just a trivial wrapper around something exported from the library.

I don’t recommend this solution for open source projects, though. If you do this for a public package then your end users will hate you because your package’s library section will depend on test packages or benchmarking packages that can’t be disabled. In contrast, proprietary codebases rarely care about gratuitous dependencies (in my experience).

#### Conclusion

Those are all of the tips I can think of at the moment. Leave a comment if you think I missed another common practice.
