
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