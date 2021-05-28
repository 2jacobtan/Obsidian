https://docs.haskellstack.org/en/stable/GUIDE/

---

[quote]
Target syntax

In addition to a number of options (like the aforementioned --test), stack build takes a list of zero or more targets to be built. There are a number of different syntaxes supported for this list:

  •  package, e.g. stack build foobar, is the most commonly used target. It will try to find the package in the following locations: local packages, extra dependencies, snapshots, and package index (e.g. Hackage). If it's found in the package index, then the latest version of that package from the index is implicitly added to your extra dependencies.
[/quote]
https://docs.haskellstack.org/en/stable/build_command/