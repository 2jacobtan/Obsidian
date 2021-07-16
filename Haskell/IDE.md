
# Haskell for Visual Studio Code
https://github.com/haskell/vscode-haskell

This extension adds language support for [Haskell](https://haskell.org/), powered by the [Haskell Language Server](https://github.com/haskell/haskell-language-server).

---

https://github.com/ndmitchell/hlint

> ### Suggested usage
> 
> HLint usage tends to proceed in three distinct phases:
> 
> 1.  Initially, run `hlint . --report` to generate `report.html` containing a list of all issues HLint has found. Fix those you think are worth fixing and keep repeating.
> 2.  Once you are happy, run `hlint . --default > .hlint.yaml`, which will generate a settings file ignoring all the hints currently outstanding. Over time you may wish to edit the list.
> 3.  For larger projects, add [custom hints or rules](https://github.com/ndmitchell/hlint/blob/master/README.md#customizing-the-hints).

---

Auto generate a stack or cabal multi component hie.yaml file
https://github.com/Avi-D-coder/implicit-hie

---

https://hackage.haskell.org/package/retrie
> # [retrie](https://hackage.haskell.org/package/retrie): A powerful, easy-to-use codemodding tool for Haskell.
> 
> \[ [development](https://hackage.haskell.org/packages/tag/development), [library](https://hackage.haskell.org/packages/tag/library), [mit](https://hackage.haskell.org/packages/tag/mit) \]
> 
> Retrie is a tool for codemodding Haskell. Key goals include:
> 
> -   Speed: Efficiently rewrite in large (>1 million line) codebases.
>     
> -   Safety: Avoids large classes of codemod-related errors.
>     
> -   Ease-of-use: Haskell syntax instead of regular expressions. No hand-rolled AST traversals.
>     
> 
> This package provides a command-line tool (`retrie`) and a library ([Retrie](https://hackage.haskell.org/package/retrie-1.0.0.0/docs/Retrie.html)) for making equational edits to Haskell code.

---

IDE errors like "cannot find module" are often solved by having a `hie.yaml` file in the base folder, containing:

```yaml
cradle:
  stack:
```

---

https://marketplace.visualstudio.com/items?itemName=haskell.haskell
> ## Requirements
> 
> -   For standalone `.hs`/`.lhs` files, [ghc](https://www.haskell.org/ghc/) must be installed and on the PATH. The easiest way to install it is with [ghcup](https://www.haskell.org/ghcup/) or [Chocolatey](https://www.haskell.org/platform/windows.html) on Windows.
> -   For Cabal based projects, both ghc and [cabal-install](https://www.haskell.org/cabal/) must be installed and on the PATH. It can also be installed with [ghcup](https://www.haskell.org/ghcup/) or [Chocolatey](https://www.haskell.org/platform/windows.html) on Windows.
> -   For Stack based projects, [stack](http://haskellstack.org/) must be installed and on the PATH.