
### ghcup

available for Linux (including WSL2 on Windows), and macOS
https://www.haskell.org/ghcup/

especially useful command: `ghcup tui`

---

### GF installation using Cabal

http://www.grammaticalframework.org/download/index-3.10.html

To use `cabal install`, the full instruction would be to use `ghcup` to install GHC 8.4.4, then do `cabal install gf-3.10 -w "/home/jt2/.ghcup/ghc/8.4.4/bin/ghc"`

Alternatively:

```bash
ghcup install ghc 8.4.4
ghcup set ghc 8.4.4
cabal install gf-3.10
```

### useful commands
E.g. `cabal get gf-3.10` 

> The `cabal get` command allows to access a package’s source code - either by unpacking a tarball downloaded from Hackage (the default) or by checking out a working copy from the package’s source repository.

https://cabal.readthedocs.io/en/3.4/cabal-package.html?highlight=cabal%20get#downloading-a-package-s-source

---

`cabal check`

> `cabal check` is the lint-like command `cabal` has. I can think of various situations where things don't work, but `cabal check` is able to point them out.
> 
> Q: Why not `cabal check` _always_ when one `cabal v2-build`, `cabal v2-repl`?  
> A: Because `cabal check` isn't always right, and is on purpose quite verbose. It is also slow (e.g. it hits file system to check whether files exist)

https://github.com/haskell/cabal/issues/7091#issuecomment-699659546