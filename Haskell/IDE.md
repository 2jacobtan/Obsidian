
# Haskell for Visual Studio Code
https://github.com/haskell/vscode-haskell

This extension adds language support for [Haskell](https://haskell.org/), powered by the [Haskell Language Server](https://github.com/haskell/haskell-language-server).

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