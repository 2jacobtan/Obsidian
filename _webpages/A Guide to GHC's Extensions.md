---
created: 2021-06-11T06:54:36 (UTC +08:00)
tags: []
source: https://limperg.de/ghc-extensions/
author: Jannis Limperg
---

# A Guide to GHC's Extensions - Jannis' Word Discounter

> ## Excerpt
> An unofficial guide to GHC's extensions for students of Haskell.

---
There are essentially two versions of the Haskell standard: [Haskell98](https://www.haskell.org/onlinereport/) and [Haskell2010](https://www.haskell.org/onlinereport/haskell2010/). Haskell2010 is a conservative evolution of Haskell98 which just makes the language better, so there’s no reason to limit yourself to Haskell98. Consequently, this guide only covers extensions that aren’t in Haskell2010, and when I say “standard Haskell”, I mean Haskell2010.

Some day, we will hopefully see a new version of the standard that incorporates GHC’s more well-established extensions. [Efforts in this direction](https://prime.haskell.org/) are underway, but they’re moving so slowly that it’s hard to tell whether they’re moving at all. In the meantime, the pragmatic choice is to treat Haskell as an implementation-defined language and to use a conservative subset of GHC’s extensions.

It is good practice to specify exactly which extensions you are using. That way, if someone tries to compile your code with a different compiler (like [GHCJS](https://github.com/ghcjs/ghcjs) or [Eta](https://eta-lang.org/)), the compiler can fail early if it doesn’t support all the extensions you rely on. There are two main ways to enable extensions:

1.  You can enable extensions in a specific source file by adding a `LANGUAGE` pragma at the top of the file:
    
    ```
    {-# LANGUAGE Extension1, Extension2 #-}
    module Mod where ...
    ```
    
    If the file is part of a Cabal project, you should also add the extensions to your Cabal file’s `other-extensions` field(s). The `other-extensions` field goes in a component block (`library`, `executable`, `test-suite` or `benchmark`) and should list all extensions used by the source files of that component.
    
2.  You can enable an extension globally in your project by adding it to a `default-extensions` field in your Cabal file:
    
    ```
    library lib
      default-extensions: Extension1, Extension2
      ...
    ```
    
    `default-extensions` works like `other-extensions`, but the listed extensions are enabled automatically for all source files belonging to the field’s component.
    

For some discussion on which style to prefer, see [here](https://www.reddit.com/r/haskell/comments/5l9w37/listing_extensions_per_hs_or_in_cabal_which_one/), [here](https://www.reddit.com/r/haskell/comments/94a8za/a_guide_to_ghcs_extensions/e3mtmcu) and [here](https://lexi-lambda.github.io/blog/2018/02/10/an-opinionated-guide-to-haskell-in-2018/#any-flavor-you-like).

## Syntactic Niceties

### `TupleSections`

Since 6.12 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-TupleSections) | [School of Haskell](https://www.schoolofhaskell.com/school/to-infinity-and-beyond/pick-of-the-week/guide-to-ghc-extensions/basic-syntax-extensions#tuplesections)

Write `(x,,)` instead of `\y z -> (x, y, z)`.

### `LambdaCase`

Since 7.6.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-LambdaCase) | [School of Haskell](https://www.schoolofhaskell.com/school/to-infinity-and-beyond/pick-of-the-week/guide-to-ghc-extensions/basic-syntax-extensions#lambdacase)

Instead of

```
\x -> case x of
  1 -> True
  _ -> False
```

write

```
\case
  1 -> True
  _ -> False
```

### `MultiWayIf`

Since 7.6.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-MultiWayIf) | [School of Haskell](https://www.schoolofhaskell.com/school/to-infinity-and-beyond/pick-of-the-week/guide-to-ghc-extensions/basic-syntax-extensions#multiwayif)

Instead of

```
if n == m
  then x
  else if n == k
    then y
    else z
```

write

```
if | n == m    -> x
   | n == k    -> y
   | otherwise -> z
```

### `BlockArguments`

Since 8.6.1 | Unstable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-BlockArguments)

If you want to apply a function to a `do` block, you usually need to use the `$` operator:

This extension allows you to omit the `$` and directly give the `do` block as an argument. This also works with lambdas, `case` statements and other blocks: `(f \x -> y)` is now equivalent to `(f (\x -> y))`.

### `TypeOperators`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-TypeOperators)

In Haskell2010, expressions like `(+)` in a type are parsed as type variables, which is not very useful. This extension allows you to use operators as names for type constructors, type synonyms etc.:

Note that starting with GHC 8.6, using `*` as a type operator may lead to trouble. See [`StarIsType`](https://limperg.de/ghc-extensions/#staristype).

### `NumericUnderscores`

Since 8.6.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-NumericUnderscores)

Write `100_000_001` instead of `100000001`.

## Overloaded Literals

### `OverloadedStrings`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-OverloadedStrings) | [School of Haskell](https://www.schoolofhaskell.com/school/to-infinity-and-beyond/pick-of-the-week/guide-to-ghc-extensions/basic-syntax-extensions#overloadedstrings) | [24 Days](https://ocharles.org.uk/posts/2014-12-17-overloaded-strings.html)

Overload string literals, similar to numeric literals. This means that `"a string"` has type `IsString a => a`, and you can define your own instances of `IsString`. For example, a library for regular expressions might define an instance `IsString Regex` that parses regular expressions, enabling you to write

### `OverloadedLists`

Since 7.8.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-OverloadedLists)

Overload list literals, similar to numeric literals (and string literals with [`OverloadedStrings`](https://limperg.de/ghc-extensions/#overloadedstrings)). This means that `[1, 2]` has type `(IsList l, Num (Item l)) => l`. (`Item` is an associated type of the `IsList` class; see [`TypeFamilies`](https://limperg.de/ghc-extensions/#typefamilies).) You can then define instances of `IsList` for list-like types like vectors and sets and write

## Patterns

### `ViewPatterns`

Since 6.10.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-ViewPatterns) | [School of Haskell](https://www.schoolofhaskell.com/school/to-infinity-and-beyond/pick-of-the-week/guide-to-ghc-extensions/pattern-and-guard-extensions#viewpatterns) | [24 Days](https://ocharles.org.uk/posts/2014-12-02-view-patterns.html)

A view pattern lets you apply a function as part of a pattern, then match against the result of that function. So instead of

```
toNonEmpty :: Set a -> Maybe (NonEmpty a)
toNonEmpty s = case toList s of
  []     -> Nothing
  (x:xs) -> x :| xs
```

write

```
toNonEmpty :: Set a -> Maybe (NonEmpty a)
toNonEmpty (toList -> []    ) = Nothing
toNonEmpty (toList -> (x:xs)) = x :| xs
```

### `PatternSynonyms`

Since 7.8.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-PatternSynonyms) | [School of Haskell](https://www.schoolofhaskell.com/school/to-infinity-and-beyond/pick-of-the-week/guide-to-ghc-extensions/pattern-and-guard-extensions#patternsynonyms) | [24 Days](https://ocharles.org.uk/posts/2014-12-03-pattern-synonyms.html)

Pattern synonyms allow you to define additional patterns for a type. For example, you can define a pattern that matches on the first two elements of a list:

```
pattern Head2 x y <- (x:y:_)

firstTwo :: [a] -> Maybe (a, a)
firstTwo (Head2 x y) = Just (x, y)
firstTwo _           = Nothing
```

Pattern synonyms are useful when you want to hide the representation of a datatype. For example, the `containers` package defines a type `Seq` representing finite lists. It is implemented as a special sort of tree, but the implementation is not exposed. Instead, the package defines pattern synonyms like `Empty` and `:<|` which allow you to match on a `Seq` as if it were a list:

```
head :: Seq a -> Maybe a
head Empty     = Nothing
head (x :<| _) = Just x
```

## Type System

### `ExplicitForAll`

Since 6.12.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-ExplicitForAll) | [School of Haskell](https://www.schoolofhaskell.com/school/to-infinity-and-beyond/pick-of-the-week/guide-to-ghc-extensions/explicit-forall#explicitforall)

The type signature `f :: a -> b -> b` means that _for all_ types `a` and `b`, we have a function from `a` and `b` to `b`. For example, `f` can be used as a function `Int -> Bool -> Bool`, where `a := Int` and `b := Bool`. `ExplicitForAll` allows us to make this “for all” explicit[1](https://limperg.de/ghc-extensions/#fn1) by writing

```
f :: forall a b. a -> b -> b
```

This is exactly the same type signature as above. Of course, that’s not very useful by itself, but the explicit `forall` becomes relevant in combination with the next three extensions.

### `TypeApplications`

Since 8.0.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-TypeApplications) | [Kwang Yul Seo](https://kseo.github.io/posts/2017-01-08-visible-type-application-ghc8.html)

A polymorphic function like

is really a function with _two_ arguments: One type, `a`, and one value of type `a`. `TypeApplications` allows you to give the type argument explicitly:

```
f :: Int -> Int
f x = id @Int x
```

This is particularly useful when combined with GHCi’s `:t` command, which you can use to view specialised type signatures:

```
> :t foldr
foldr :: Foldable t => (a -> b -> b) -> b -> t a -> b
> :t foldr @[] @Int @Bool
foldr @[] @Int @Bool :: (Int -> Bool -> Bool) -> Bool -> [Int] -> Bool
```

### `ScopedTypeVariables`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-ScopedTypeVariables) | [24 Days](https://ocharles.org.uk/guest-posts/2014-12-20-scoped-type-variables.html) | [School of Haskell](https://www.schoolofhaskell.com/school/to-infinity-and-beyond/pick-of-the-week/guide-to-ghc-extensions/explicit-forall#scopedtypevariables)

Consider the following slightly-contrived-for-demonstration-purposes implementation of the `minimum` function:

```
minimum :: forall a. Ord a => [a] -> Maybe a
minimum [] = Nothing
minimum xs = Just $ foldl1' go xs
  where
    go :: a -> a -> a
    go = min
```

GHC doesn’t accept this (even with [`ExplicitForAll`](https://limperg.de/ghc-extensions/#explicitforall)) because from its point of view, the `a` in `minimum`’s type signature and the `a` in `go`’s type signature are different variables that just happen to share the same name. Therefore, we don’t have a constraint `Ord a` in `go`, and so we can’t use `min` (a method of `Ord`).

With `ScopedTypeVariables`, our definition of `minimum` is accepted: Both `a`s are now considered the same type variable, and the `Ord a` constraint from `minimum` ‘trickles down’ to `go`.

Caveat: This only works because we quantify over `a` explicitly with `forall a`. If we omit the `forall a`, we get the same error as before.

### `RankNTypes`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-RankNTypes) | [School of Haskell](https://www.schoolofhaskell.com/school/to-infinity-and-beyond/pick-of-the-week/guide-to-ghc-extensions/explicit-forall#rankntypes--rank2types--and-polymorphiccomponents) | [24 Days](https://ocharles.org.uk/guest-posts/2014-12-18-rank-n-types.html) | [Gregor Riegler](http://sleepomeno.github.io/blog/2014/02/12/Explaining-Haskell-RankNTypes-for-all/) | [Chris Done](https://chrisdone.com/posts/rankntypes)

The [`ST` monad](http://hackage.haskell.org/package/base-4.11.1.0/docs/Control-Monad-ST.html) can be used to safely run stateful computations. To run an `ST` computation, you use the function `runST`, which has type

```
runST :: forall a. (forall s. ST s a) -> a
```

This function has one type argument, `a`, and one value argument of type `forall s. ST s a`. `RankNTypes` is required to allow this argument type, because it contains a `forall` – without `RankNTypes`, the only place where a `forall` may appear is at the beginning of a type signature.

The meaning of this argument type is that the argument must be polymorphic in `s`, meaning that the argument must have type `ST s a` for _any_ `s` and _some_ `a`. Contrast this with the different type signature

```
runST :: forall a s. ST s a -> a
```

This allows us to pass `m :: ST Int Bool` as an argument to `runST`, setting `a := Bool` and `s := Int`. With the correct signature of `runST`, on the other hand, `m` cannot be used as an argument because it would have to be of type `ST s Bool` for _any_ `s`.

`RankNTypes` is arguably an advanced extension, but it is required for `ST`, which is in `base`.

### `LiberalTypeSynonyms`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-LiberalTypeSynonyms) | [School of Haskell](https://www.schoolofhaskell.com/school/to-infinity-and-beyond/pick-of-the-week/guide-to-ghc-extensions/explicit-forall#liberaltypesynonyms)

Standard Haskell places a lot of restrictions on type synonyms; for example, you can’t use `forall` in a type synonym. `LiberalTypeSynonyms` lifts most of these restrictions, which can occasionally come in handy.

## Records

### `NamedFieldPuns`

Since 6.10.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-NamedFieldPuns) | [Wikibook](https://en.wikibooks.org/wiki/Haskell/More_on_datatypes#Named_Fields_(Record_Syntax))

Suppose we have a record storing some configuration:

```
data Configuration = Configuration
    { localHost :: String
    , localPort :: Int
    , ...
    }
```

To extract only some data out of a `Configuration`, we can use record pattern matching syntax:

```
localAddr :: Configuration -> String
localAddr Configuration { localHost = localHost, localPort = localPort }
    = localHost ++ ":" ++ show localPort
```

Obviously, writing `field = field` gets old quickly. `NamedFieldPuns` reduces the noise:

```
localAddr :: Configuration -> String
localAddr Configuration { localHost , localPort }
    = localHost ++ ":" ++ show localPort
```

### `RecordWildCards`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-RecordWildCards) | [24 Days](https://ocharles.org.uk/posts/2014-12-04-record-wildcards.html) | [Kwang Yul Seo](https://kseo.github.io/posts/2014-02-10-record-wildcards.html)

This extension takes the principle behind [`NamedFieldPuns`](https://limperg.de/ghc-extensions/#namedfieldpuns) one step further: We can now write (continuing the example):

```
localAddr :: Configuration -> String
localAddr Configuration { .. }
    = localHost ++ ":" ++ show localPort
```

The `{ .. }` is equivalent to writing one field pun for every field of `Configuration`.

## Classes

### `FlexibleInstances`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-FlexibleInstances)

Haskell2010 doesn’t allow you to write instances like

```
instance C (Maybe Int) where ...
```

because `Maybe` must only be applied to type variables. `FlexibleInstances` lifts this restriction. Additionally, it allows you to declare instances for type synonyms.

### `FlexibleContexts`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-FlexibleContexts)

Haskell2010 places some restrictions on the superclass constraints that can appear in a class declaration. `FlexibleContexts` lifts these restrictions.

### `DeriveFunctor`, `DeriveFoldable`, `DeriveTraversable`

Since 7.10.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-DeriveFunctor) | [24 Days](https://ocharles.org.uk/guest-posts/2014-12-15-deriving.html)

This adds `Functor`, `Foldable` and `Traversable` to the list of classes that GHC can derive for you automatically. So, if you ever need to define your own list type, you get almost every relevant function for free.

### `GeneralizedNewtypeDeriving`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-GeneralizedNewtypeDeriving) | [24 Days](https://ocharles.org.uk/guest-posts/2014-12-15-deriving.html#newtypes)

In Haskell land, we like to introduce lots of `newtype`s to prevent errors:

```
newtype Dollars = Dollars Int
```

Yet that also forces us to write lots of boilerplate instances:

```
instance Num Dollars where
  Dollars x + Dollars y = Dollars (x + y)
  Dollars x * Dollars y = Dollars (x * y)
  ...
```

All we do here is wrap and unwrap the `Dollar` constructor. With `GeneralizedNewtypeDeriving`, GHC can write these instances for us:

```
newtype Dollars = Dollars Int
  deriving (Num)
```

and this work for almost every class, not just the built-in `Eq`, `Ord` etc.

There is an unfortunate interaction between `GeneralizedNewtypeDeriving` and [type families](https://limperg.de/ghc-extensions/#typefamilies); see [`RoleAnnotations`](https://limperg.de/ghc-extensions/#roleannotations). However, that doesn’t need to concern you most of the time.

### `InstanceSigs`

Since 7.6.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-InstanceSigs)

This extension allows you to give type signatures in instances, which can be useful documentation:

```
data Result a = Success a | Failure

instance Functor Result where
  fmap :: (a -> b) -> Result a -> Result b
  fmap f (Success x) = Success $ f x
  fmap f Failure     = Failure
```

Without `InstanceSigs`, the signature for `fmap` would be illegal.

### `ConstrainedClassMethods`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-ConstrainedClassMethods)

Haskell2010 doesn’t allow you to have additional constraints on class methods, so the following is disallowed (since `f`, a class method, has an `Eq` constraint):

```
class Foo a where
  f :: Eq a => a -> a
```

`ConstrainedClassMethods` lifts this (quoting the User’s Guide) “pretty stupid” restriction.

### `MultiParamTypeClasses`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-MultiParamTypeClasses) | [24 Days](https://ocharles.org.uk/posts/2014-12-13-multi-param-type-classes.html) | [Wikibook](https://en.wikibooks.org/wiki/Haskell/Advanced_type_classes) | [Dennis Gosnell](https://functor.tokyo/blog/2015-03-14-multi-parameter-type-classes)

In standard Haskell, classes can only apply to a single type (here `a`):

`MultiParamTypeClasses` lifts this restriction, allowing us to write classes that apply to multiple types. For example, the popular [`mtl`](http://hackage.haskell.org/package/mtl) package defines a class

```
class MonadReader r m where ...
```

which denotes monads `m` that have access to a value of type `r`. (That’s not quite the whole story – `MonadReader` needs an additional [functional dependency](https://limperg.de/ghc-extensions/#functionaldependencies).)

[Type families](https://limperg.de/ghc-extensions/#typefamilies) provide a better alternative to `MultiParamTypeClasses` in many cases, but the latter are older and still used by many popular packages.

### `FunctionalDependencies`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-FunctionalDependencies) | [24 Days](https://ocharles.org.uk/posts/2014-12-14-functional-dependencies.html) | [Wikibook](https://en.wikibooks.org/wiki/Haskell/Advanced_type_classes) | [Dennis Gosnell](https://functor.tokyo/blog/2015-03-14-multi-parameter-type-classes)

With `MultiParamTypeClasses`, you’ll soon encounter situations where some of a class’s type parameters already determine another type parameter. For example, with `MonadReader`, if `m` is `Reader Int`, then `r` can only be `Int`, and this applies generally: For every `m`, there is at most one valid `r`.

Functional dependencies let you express precisely this fact:

```
class MonadReader r m | m -> r where ...
```

The `m -> r` is a functional dependency which tells GHC that for any `m`, there is at most one `r` with instance `MonadReader m r` (and GHC then won’t allow us to declare multiple such instances). This is useful because it allows GHC to infer `r` from `m`. Without the functional dependency, you would have to give a lot more type annotations because GHC would frequently fail to infer which `r` you mean.

### `DeriveGeneric`

Since 7.2.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-DeriveGeneric) | [24 Days](https://ocharles.org.uk/posts/2014-12-16-derive-generic.html) | [Mark Karpov](https://www.stackbuilders.com/tutorials/haskell/generics/) | [Danny Gratzer](https://jozefg.bitbucket.io/posts/2014-04-25-you-could-have.html) | [Haskell Wiki](https://wiki.haskell.org/GHC.Generics)

`DeriveGeneric` enables deriving of the `Generic` class, which provides support for the popular `GHC.Generics` flavour of generic programming. ‘Generic’ means that you can define functions which work for a variety of datatypes, without having to write separate code for each datatype.

The linked tutorials discuss how to write a library using generic programming, which is an advanced topic. However, to use these libraries (for example [Aeson](http://hackage.haskell.org/package/aeson)) you just need to put a `deriving Generic` next to your datatypes, which is why this extension is in the basic track.

## Typed Holes

### Typed Holes and Type Wildcards

Since 7.8.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#typed-holes) | [Chris Barrett](https://github.com/chrisbarrett/haskell-typed-holes-tutorial) | [Haskell Wiki](https://wiki.haskell.org/GHC/Typed_holes)

Basic typed holes don’t require an extension and are enabled by default. They can occur in terms and in types.

In terms, you can write an underscore, called a hole, and GHC will tell you what type it expects you to fill in instead of the hole. For example, if you write

then GHC will throw an error saying that it expects something of type `a` and that there is, conveniently, an `x :: a` in scope.

In type signatures, you can write an underscore, called a type wildcard, and GHC will tell you what type it inferred. Continuing the example:

In this case, GHC will throw an error telling you that the `_` must stand for `a`. Of course, typed holes and type wildcards are much more interesting with more complex functions and types.

### `NamedWildCards`

Since 7.10.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-NamedWildCards) | [School of Haskell](https://www.schoolofhaskell.com/user/thomasw/new-in-ghc-7-10-partial-type-signatures)

This extension allows you to add an arbitrary name to a [type wildcard](https://limperg.de/ghc-extensions/#typed-holes), so you can write

GHC will use the name in the error message. You can also use the same named wildcard multiple times in a signature, in which case GHC assumes that all its occurrences refer to the same type:

Without this extension, `_a` would be interpreted as a regular type variable.

### `PartialTypeSignatures`

Since 7.10.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-PartialTypeSignatures) | [School of Haskell](https://www.schoolofhaskell.com/user/thomasw/new-in-ghc-7-10-partial-type-signatures)

When you use a [type wildcard](https://limperg.de/ghc-extensions/#typed-holes) in a signature, you usually get an (informative) error. This extension turns these errors into warnings so you can use partial type signatures in your regular code. This lets you omit the boring parts of a complex type signature.

## Type System

### `ExistentialQuantification`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-ExistentialQuantification) | [24 Days](https://ocharles.org.uk/guest-posts/2014-12-19-existential-quantification.html) | [Wikibook](https://en.wikibooks.org/wiki/Haskell/Existentially_quantified_types) | [Arnaud Bailly](http://abailly.github.io/posts/existential-types.html) | [Jonathan Fischoff](https://medium.com/@jonathangfischoff/existential-quantification-patterns-and-antipatterns-3b7b683b7d71)

An existentially quantified type allows you to ‘forget’ the type of a value, typically remembering only that the type belonged to some class. For example, we can define a type for any value that can be `show`n:

```
data Showable = forall a. Show a => Showable a

intShowable :: Int -> Showable
intShowable n = Showable n

stringShowable :: String -> Showable
stringShowable s = Showable s

showShowable :: Showable -> String
showShowable (Showable x) = show x
```

As demonstrated by `intShowable` and `stringShowable`, a `Showable` can contain a value of any type, as long as that type implements the `Show` class. `showShowable` demonstrates that we can use this fact to show a `Showable`. Note that we can’t do anything else with a `Showable`: the existential type ‘forgets’ everything about the value contained in it except that it can be shown.

Existential types look like an obvious solution to various problems, but it often turns out that these problems are better solved in other ways. [Jonathan Fischoff’s article](https://medium.com/@jonathangfischoff/existential-quantification-patterns-and-antipatterns-3b7b683b7d71) lays out some alternatives to existential quantification.

### `GADTSyntax`

Since 7.2.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-GADTSyntax) | [School of Haskell](https://www.schoolofhaskell.com/user/PthariensFlame/guide-to-ghc-extensions/data-type-extensions#gadtsyntax)

Constructors can be understood as special functions whose return type is the datatype to which they belong. For example, the `Maybe` type has constructors

```
Just :: a -> Maybe a
Nothing :: Maybe a
```

`GADTSyntax` provides an alternative syntax for datatype declarations which reflects this understanding:

```
data Maybe a where
  Just    :: a -> Maybe a
  Nothing :: Maybe a
```

`GADTSyntax` lets you define exactly those datatypes which you can define normally, so there’s not much reason to enable this extension, but we need it for [`GADTs`](https://limperg.de/ghc-extensions/#gadts).

### `GADTs`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-GADTs) | [Wikibook](https://en.wikibooks.org/wiki/Haskell/GADT) | [Haskell Wiki](https://wiki.haskell.org/GADTs_for_dummies) | [Matt Parsons](http://www.parsonsmatt.org/2017/04/26/basic_type_level_programming_in_haskell.html)

When you write a regular datatype in [`GADTSyntax`](https://limperg.de/ghc-extensions/#gadtsyntax), the return type of all constructors must be the datatype applied to its type variables. In our example from above, the return type of every constructor is (and must be) exactly `Maybe a`:

```
data Maybe a where
  Just    :: a -> Maybe a
  Nothing ::      Maybe a
```

GADTs lift this restriction, so different constructors can instantiate the datatype’s type variables differently:

```
data RestrictedMaybe a where
  JustInt    :: Int    -> RestrictedMaybe Int
  JustString :: String -> RestrictedMaybe String
  Nothing    ::           RestrictedMaybe a
```

A `RestrictedMaybe` can contain either an `Int` or a `String`, but no other types. This is not particularly useful, but the same principle can be applied to define a wide variety of interesting types.

### `TypeFamilies`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-TypeFamilies) | [24 Days](https://ocharles.org.uk/posts/2014-12-12-type-families.html) | [mchaver](http://www.mchaver.com/posts/2017-06-21-type-families.html) | [Haskell Wiki](https://wiki.haskell.org/GHC/Type_families) | [Matt Parsons](http://www.parsonsmatt.org/2017/04/26/basic_type_level_programming_in_haskell.html) | [Kwang Yul Seo](https://kseo.github.io/posts/2017-01-16-type-level-functions-using-closed-type-families.html)

Type families are the cornerstone of contemporary type-level programming in Haskell, so if you want to write programmes with fancy types (or use certain libraries), you should probably learn about them. The basics are not so hard, though there are a fair few corner cases. A short description couldn’t really do the concept justice, so I just recommend the above tutorials. [Matt Parsons](http://www.parsonsmatt.org/2017/04/26/basic_type_level_programming_in_haskell.html) in particular gives a concise overview of type-level programming which also covers several other extensions.

### `TypeFamilyDependencies`

Since 8.0.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-TypeFamilyDependencies) | [Jan Stolarek](http://lambda.jstolarek.com/2015/05/injective-type-families-for-haskell/) | [Paper (pdf)](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/07/injective-type-families-acm.pdf)

Type family dependencies (aka injective type families) are the equivalent of [functional dependencies](https://limperg.de/ghc-extensions/#functionaldependencies) for type families.

### `AllowAmbiguousTypes`

Since 7.8.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-AllowAmbiguousTypes)

GHC has an ambiguity check which raises an error if a function looks like it could never be called without leading to an ambiguous constraint. However, the check is incomplete, meaning that it sometimes rejects functions which can, in fact, be called. If you’re doing fancy type-level programming, you may therefore need to disable the check with `AllowAmbiguousTypes` (but you probably don’t).

Despite the scary name, this extension does not compromise type safety. If a function call leads to an ambiguous constraint, this is still an error. The only difference is that the error is reported at the function call, not at the function definition.

## Kinds

### `KindSignatures`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-KindSignatures)

This extension allows you specify the kinds of type variables (wherever type variables may occur). This can be used for documentation, and it’s sometimes necessary when working with fancy kinds. An unnecessarily verbose example:

```
class Functor (f :: * -> *) where
  fmap :: forall (a :: *) (b :: *). (a -> b) -> f a -> f b
```

### `ConstraintKinds`

Since 7.4.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-ConstraintKinds) | [Kwang Yul Seo](https://kseo.github.io/posts/2017-01-13-constraint-kinds.html) | [Wolfgang Jeltsch](https://jeltsch.wordpress.com/2013/02/14/the-constraint-kind/)

A constraint is anything that can appear to the left of the `=>` arrow in a type signature (mostly type class constraints like `(Ord a, Eq b)`). Constraints are type-like things which have a special kind, `Constraint`. `ConstraintKinds` makes this kind available, so you can define, for example, constraint synonyms:

```
type Serializable a = (Show a, Read a)

roundtrip :: Serializable a => a -> a
roundtrip = read . show
```

### `DataKinds`

Since 7.4.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-DataKinds) | [Matt Parsons](http://www.parsonsmatt.org/2017/04/26/basic_type_level_programming_in_haskell.html) | [Kwang Yul Seo](https://kseo.github.io/posts/2017-01-16-type-level-functions-using-closed-type-families.html) | [Christian Marie](http://ponies.io/posts/2014-07-30-typelits.html)

With `DataKinds` enabled, when you define a data type like

```
data Nat = Zero | Succ Nat
```

you also get a kind `'Nat` corresponding to the type `Nat`, as well as types `'Zero` and `'Succ` corresponding to the constructors `Zero` and `Succ`. Hence

```
-- 'Nat is a kind
'Zero :: 'Nat
'Succ :: 'Nat -> 'Nat
```

Together with [type families](https://limperg.de/ghc-extensions/#typefamilies), this allows you to compute, for example, natural numbers at the type level, much like you would at the value level.

### `PolyKinds`

Since 7.4.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-PolyKinds)

This extension enables kind-polymorphic types like the type-level identity function (which of course requires [`TypeFamilies`](https://limperg.de/ghc-extensions/#typefamilies)):

```
type family Id (a :: k) :: k where
  Id a = a
```

We can now apply `Id` to types of different kinds:

```
type IdInt   = Id Int    -- kind *
type IdMonad = Id Monad  -- kind (* -> *)
```

## Empty Types

### `EmptyCase`

Since 7.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-EmptyCase)

Haskell2010 allows you to define empty data types, which have no constructors. The only value of such a type is `undefined`. `EmptyCase` allows you to match on such values with a `case` statement that has zero alternatives, corresponding to the zero constructors:

```
data Void

absurd :: Void -> a
absurd v = case v of {}
```

### `EmptyDataDeriving`

Since 8.4.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#ghc-flag--XEmptyDataDeriving)

Haskell2010 does not allow you to derive the four standard type classes `Eq`, `Ord`, `Read` and `Show` for empty types. This is a simple oversight, which this extension fixes.

## Classes

### `StandaloneDeriving`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-StandaloneDeriving)

`deriving` clauses must usually be attached to a datatype declaration. This extension allows you to derive classes ‘after the fact’, even in a different module. Standalone `deriving` clauses are also a little more liberal than attached `deriving` clauses; for example, they support some [GADTs](https://limperg.de/ghc-extensions/#gadts).

```
data Result a = Success a | Failure

deriving instance Eq a => Eq (Result a)
```

### `DefaultSignatures`

Since 7.2.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-DefaultSignatures) | [Mark Karpov](https://www.stackbuilders.com/tutorials/haskell/generics/) | [Danny Gratzer](https://jozefg.bitbucket.io/posts/2014-04-25-you-could-have.html) | [Haskell Wiki](https://wiki.haskell.org/GHC.Generics)

When declaring a class, you can give default implementations for some or all of the class methods:

```
class Eq a where
  (==) :: a -> a -> Bool
  a == b = not (a /= b)

  (/=) :: a -> a -> Bool
  a /= b = not (a == b)
```

With `DefaultSignatures`, you can give type signatures for default methods. These can be more specific than the implemented method’s signature, so you can provide a default implementation only if the type in question has instances of other classes. For example, one could define

```
class Eq a where
  (==) :: a -> a -> Bool
  default (==) :: Ord a => a -> a -> Bool
  a == b = compare a b == EQ

instance Eq Int

instance Ord Int where
  compare a b = ...
```

`DefaultSignatures` is useful primarily for generic programming with `GHC.Generics`.

### `DeriveAnyClass`

Since 7.10.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-DeriveAnyClass)

This extensions allows you to write a `deriving` clause for any class, and GHC will simply generate an empty instance declaration (except for those classes which it knows how to derive). This makes sense if there are default implementations for all of the class’s methods, which happens frequently when using `GHC.Generics`. For example, we may define a pretty-printing class which falls back to `Show` (using [`DefaultSignatures`](https://limperg.de/ghc-extensions/#defaultsignatures)):

```
class Pretty a where
  prettyPrint :: a -> String
  default prettyPrint :: Show a => a -> String
  prettyPrint = show

data Stuff = Stuff
  deriving (Show, Pretty)
```

### `DerivingStrategies`

Since 8.2.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-DerivingStrategies) | [Ryan Scott](https://ryanglscott.github.io/2017/04/12/improvements-to-deriving-in-ghc-82/)

With both [`DeriveAnyClass`](https://limperg.de/ghc-extensions/#deriveanyclass) and [`GeneralizedNewtypeDeriving`](https://limperg.de/ghc-extensions/#generalizednewtypederiving) enabled, it is unclear how to process the following declaration:

```
newtype N = N Int
  deriving Num
```

GHC could either use the `DeriveAnyClass` strategy and create an empty instance declaration, or it could use `GeneralizedNewtypeDeriving`. `DerivingStrategies` allows you to specify which strategy you want:

```
newtype N = N Int
  deriving newtype Num
```

### `DerivingVia`

Since 8.6.1 | Unstable | [User’s Guide](https://downloads.haskell.org/~ghc/8.6.1/docs/html/users_guide/glasgow_exts.html#extension-DerivingVia) | [Paper (pdf)](https://www.kosmikus.org/DerivingVia/deriving-via-paper.pdf)

With this extension, you can write

```
newtype N = N Int
  deriving Semigroup via (Product Int)
```

`Product Int` (from `Data.Monoid`) is another newtype for `Int` whose `Semigroup` instance uses multiplication. With the `deriving via` clause, our type `N` ‘inherits’ this `Semigroup` instance. Another possible choice would be `deriving Semigroup via (Sum Int)`, in which case we would get a `Semigroup` instance that uses addition.

A `deriving via` clause requires that the type via which we are deriving (here `Product Int`) and the type whose instance we are deriving (`N`) have the same runtime representation (a machine integer).

### `QuantifiedConstraints`

Since 8.6.1 | Unstable | [User’s Guide](https://downloads.haskell.org/~ghc/8.6.1/docs/html/users_guide/glasgow_exts.html#extension-QuantifiedConstraints) | [Ryan Scott](http://ryanglscott.github.io/2018/02/11/how-to-derive-generic-for-some-gadts/) | [Paper (pdf)](https://homepages.inf.ed.ac.uk/wadler/papers/quantcc/quantcc.pdf)

This extension allows you to universally quantify constraints (i.e. to say that a constraint `C x` must be fulfilled for _every_ `x`). Using this feature, the class of monad transformers, `MonadTrans`, could be rewritten like this:

```
class (forall m. Monad m => Monad (t m)) => MonadTrans t where
  ...
```

The `forall`\-quantified constraint expresses our expectation that applying the transformer `t` to any monad `m` should again result in a monad.

### `UndecidableInstances`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-UndecidableInstances) | [Dennis Gosnell](https://functor.tokyo/blog/2017-04-07-undecidable-instances)

GHC places some restrictions on instance declarations to ensure that it can always resolve instances in finite time. These restrictions are incomplete, meaning that they disallow some instance declarations that are perfectly fine. `UndecidableInstances` can therefore be used to disable GHC’s checks. You should not have to do this often.

### `UndecidableSuperClasses`

Since 8.0.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-UndecidableSuperClasses) | [Edward Kmett (video)](https://www.youtube.com/watch?v=ZL9ehIJhk98)

GHC usually does not allow a class to be a superclass of itself, to ensure that typeclass resolution terminates. It can occasionally be useful to disable this check.

### `RoleAnnotations`

Since 7.8.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-RoleAnnotations) | [Richard Eisenberg](https://typesandkinds.wordpress.com/2013/08/15/roles-a-new-feature-of-ghc/)

GHC assigns every type variable (in datatypes, classes, etc.) a _role_. If a type variable has the wrong role, `GeneralizedNewtypeDeriving` can be used to break type safety. GHC’s heuristics for role assignment mostly do the right thing, but it can be necessary to help them out by specifying roles explicitly. This extension allows you to do that.

Technically, you should think about roles whenever you write a type variable, but I’m not sure anyone actually does that.

## Records

### `DisambiguateRecordFields`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-DisambiguateRecordFields)

Say you have two records with the same field name,

```
module M where

data UserPrefs = UserPrefs
    { user :: Int
    , prefs :: String
    }

-----------------------------------

module N where

data Profile = Profile
    { user :: Int
    , profileData :: String
    }
```

Haskell2010 will only let you use the `user` record selector qualified, even where it’s entirely obvious which of the two record you mean. For example, the following is not accepted (with `M` imported):

```
updateProfile :: (Int -> Int) -> Profile -> Profile
updateProfile f Profile { user = oldUser, profile = profile }
    = Profile { user = f oldUser, profile = profile}
```

With `DisambiguateRecordFields`, GHC accepts this definition (if and only if `UserPrefs` and `Profile` are defined in different modules). However, this only works because both occurrences of `user` are under a `Profile` constructor.

### `DuplicateRecordFields`

Since 8.0.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-DuplicateRecordFields)

Extending [`DisambiguateRecordFields`](https://limperg.de/ghc-extensions/#disambiguaterecordfields), `DuplicateRecordFields` allows multiple records _in the same module_ to share a field name.

Moreover, GHC can now sometimes disambiguate uses of record fields based on type information. (`DisambiguateRecordFields`, in contrast, only looks at whether record fields occur under a constructor.) For example, the following is accepted:

```
updateProfile :: (Int -> Int) -> Profile -> Profile
updateProfile f p = p { user = f $ user (p :: Profile) }
```

Exactly when GHC can disambiguate record fields is a little tricky – for instance, `updateProfile` is not accepted without the type annotation on `p`.

### `OverloadedLabels`

Since 8.0.1 | Unstable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-OverloadedLabels)

With this extension, the new syntactic form `#foo` is desugared to `fromLabel @foo`, with `fromLabel` a class method of

```
class IsLabel (x :: Symbol) a where
  fromLabel :: a
```

(`Symbol` is a type-level string.) GHC by itself defines no instances of `IsLabel`. As I understand it, this mechanism was intended to be used for properly overloaded record fields, but it seems like there is no satisfying solution to this problem yet.

## Strictness

### `BangPatterns`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-BangPatterns) | [24 Days](https://ocharles.org.uk/posts/2014-12-05-bang-patterns.html) | [School of Haskell](https://www.schoolofhaskell.com/user/PthariensFlame/guide-to-ghc-extensions/pattern-and-guard-extensions#bangpatterns)

With this extension, you can prefix a pattern with a bang (`!`):

```
f :: Int -> Bool
f !n = True
```

When something is matched against the bang pattern, it will be evaluated to [weak head normal form](https://wiki.haskell.org/Weak_head_normal_form) before the body of `f` is evaluated. For example, consider the following programme:

```
fibonacci :: Int -> Int
fibonacci 0 = 0
fibonacci 1 = 1
fibonacci n = fibonacci (n - 1) + fibonacci (n - 2)

main :: IO ()
main = print $ f (fibonacci 100)
```

Running `main` will take a while because it has to (very inefficiently) compute `fibonacci 100` before `f` can return its result. Without the bang pattern, the programme terminates almost instantly because we don’t actually need the result of `fibonacci 100`.

### `Strict`, `StrictData`

Since 8.0.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-StrictData) | [Johan Tibell](http://blog.johantibell.com/2015/11/the-design-of-strict-haskell-pragma.html)

`StrictData` makes all fields of datatypes strict by default. You can then use `~` to make some fields lazy again. For example, with `StrictData` active, the declaration

is equivalent to the usual

`Strict` works like `StrictData`, but in addition to fields of datatypes, it also makes most other things (patterns, `let`/`where` bindings, …) strict by default.

## `Do` Notation

### `ApplicativeDo`

Since 8.0.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-ApplicativeDo) | [Paper (pdf)](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/08/desugaring-haskell-haskell16.pdf)

A definition like

```
f = do
  x <- mx
  y <- my
  return $ g x y
```

is equivalent to `f = g <$> mx <*> my`, which uses only `Applicative` combinators. Despite this, Haskell2010 will require a `Monad` constraint – and use the monadic combinators – as soon as you use `do` syntax. `ApplicativeDo` changes this, allowing you to use `do` notation with applicatives that aren’t also monads. Moreover, for some monads, the applicative combinators are more efficient than the monadic ones, in which case `ApplicativeDo` may improve performance.

### `RecursiveDo`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#recursive-do-notation) | [24 Days](https://ocharles.org.uk/posts/2014-12-09-recursive-do.html) | [Roman Cheplyaka](https://ro-che.info/articles/2015-09-02-monadfix) | [Will Fancher](https://elvishjerricco.github.io/2017/08/22/monadfix-is-time-travel.html)

Some monads (lists, `Maybe`, `ST`, `IO`, …) support a notion of cyclic computation, in which a data structure is built by using parts of it that have already been built. This is captured in the [`MonadFix`](http://hackage.haskell.org/package/base-4.11.1.0/docs/Control-Monad-Fix.html) class. `RecursiveDo` adds some syntactic sugar so you can more easily write recursive monadic computations.

## Literals

### `NegativeLiterals`

Since 7.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-NegativeLiterals)

The literal `-1` is usually desugared to `negate (fromInteger 1)`. With `NegativeLiterals`, it is instead desugared to `fromInteger (-1)`. This should not make a difference most of the time.

### `NumDecimals`

Since 7.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-NumDecimals)

Floating-point literals like `2.1e6` usually have type `Floating a => a`. With `NumDecimals`, floating point literals which denote integers, like `2e6`, have type `Num a => a` instead, so you can use scientific notation for integers.

### `BinaryLiterals`

Since 7.10.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-BinaryLiterals)

This extension allows you to write integer literals in binary: `0b10` desugars to `fromInteger 2`.

### `HexFloatLiterals`

Since 8.4.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#ghc-flag--XHexFloatLiterals)

This extension allows you to write floating point literals in hexadecimal, which corresponds closely to the underlying binary representation.

## Template Haskell

### `TemplateHaskell`, `TemplateHaskellQuotes`

Since 6.0/8.01 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-TemplateHaskell) | [24 Days](https://ocharles.org.uk/blog/guest-posts/2014-12-22-template-haskell.html) | [Mark Karpov](https://markkarpov.com/tutorial/th.html) | [Matt Parsons](http://www.parsonsmatt.org/2015/11/15/template_haskell.html) | [Haskell Wiki](https://wiki.haskell.org/A_practical_Template_Haskell_Tutorial)

Template Haskell is a major extension that allows you to generate code at compile time. Basically, you write Haskell programmes that generate Haskell code (i.e. abstract syntax trees). GHC then runs your programmes at compile time and combines the generated code with your regular code. This is very powerful because you can generate whatever code you want, but it’s also a bit of a sledgehammer with a bunch of rough edges.

`TemplateHaskellQuotes` enables a safer, but also considerably less useful subset of the `TemplateHaskell` functionality.

### `QuasiQuotes`

Since 6.10.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-QuasiQuotes) | [Edsko de Vries](https://www.well-typed.com/blog/2014/10/quasi-quoting-dsls/)

Quasi-quotes are an extension to [Template Haskell](https://limperg.de/ghc-extensions/#templatehaskell) which allow you to embed other languages into Haskell. For example:

```
query :: Query
query = [sql| select * from users |]
```

`sql` is, in effect, a function which parses an SQL query from a user-provided string at compile time, returning the Haskell type `Query`.

### `DeriveLift`

Since 7.2.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-DeriveLift)

[Template Haskell](https://limperg.de/ghc-extensions/#templatehaskell) makes use of a `Lift` class to convert expressions into abstract syntax trees. This extension allows you to have your `Lift` instances derived automatically.

## Low-Level Hacking

### `MagicHash`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-MagicHash)

GHC likes to give its primitives names that end in a hash (`#`). Haskell2010 disallows the hash in identifiers, so you have to enable `MagicHash` to be able to refer to these primitives.

### `UnboxedTuples`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-UnboxedTuples) | [Michael Snoyman](https://www.fpcomplete.com/blog/2015/02/primitive-haskell)

An unboxed tuple, written `(# Int, Bool #)`, allows you to return multiple values from a function without the overhead of a regular tuple (heap allocation, pointer dereferencing, etc.). When you return an unboxed tuple, its contents will be passed directly via registers or the stack.

### `UnboxedSums`

Since 8.2.1 | Unstable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-UnboxedSums) | [Ömer Sinan Ağacan](https://osa1.net/posts/2016-07-22-unboxed-sums-faq.html)

An unboxed sum, written `(# Int | Bool #)`, is like an anonymous sum type with multiple alternatives. Unlike regular sum types, GHC will try to represent unboxed sums as compactly as possible.

## Safe Haskell

### `Safe`, `Trustworthy`, `Unsafe`

Since 7.2.1/7.2.1/7.4.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#safe-imports) | [Kristen Kozak (video with notes by Joe Nelson)](https://begriffs.com/posts/2015-05-24-safe-haskell.html) | [Edward Z. Yang](http://blog.ezyang.com/2012/09/common-misconceptions-about-safe-haskell/) | [Paper (pdf)](https://www.microsoft.com/en-us/research/wp-content/uploads/2012/01/safe-haskell.pdf)

Haskell has various features, like [`unsafePerformIO`](http://hackage.haskell.org/package/base-4.11.1.0/docs/System-IO-Unsafe.html#v:unsafePerformIO), which can be (mis)used to circumvent the type system, module abstraction and other desirable properties. Safe Haskell allows you to control the use of these features to some degree, but I don’t think it ever really caught on.

## Miscellaneous

### `CPP`

Since forever | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/phases.html#options-affecting-the-c-pre-processor) | [Aelve](https://guide.aelve.com/haskell/cpp-vww0qd72)

The C preprocessor is (unfortunately) the most common and easiest way to do simple compile-time metaprogramming. A typical use case is supporting different GHC versions, which sometimes requires slightly different code.

CPP is not technically an extension, but it is enabled with a `{-# LANGUAGE CPP #-}` pragma (or a GHC flag).

### `NoImplicitPrelude`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-NoImplicitPrelude)

The module `Prelude` is usually imported implicitly in every module you write. `NoImplicitPrelude` disables this. This is useful if you want to use one of many alternative preludes developed by the community, whose exported names frequently clash with those from `Prelude`.

### `RebindableSyntax`

Since 7.0.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-RebindableSyntax) | [24 Days](https://ocharles.org.uk/blog/guest-posts/2014-12-06-rebindable-syntax.html)

Certain syntactic forms desugar to ordinary Haskell functions. For example, `do` syntax is desugared into applications of the `>>=` and `return` combinators; literals `N` are desugared into `fromInteger N`; etc. Usually, this desugaring uses fixed functions, so the `fromInteger` is really `Prelude.fromInteger`. With `RebindableSyntax`, the desugaring uses whatever functions are in scope, so you can define your own `fromInteger` and GHC will use that.

### `UnicodeSyntax`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-UnicodeSyntax) | [Haskell Wiki](https://wiki.haskell.org/Unicode-symbols)

This extension allows you to use Unicode symbols for certain keywords, like `∀` instead of `forall`. (Even without `UnicodeSyntax`, you can use Unicode for identifiers.)

### `NoMonomorphismRestriction`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-NoMonomorphismRestriction) | [Neil Mitchell](http://neilmitchell.blogspot.com/2009/02/monomorphism-and-defaulting.html) | [Stack Overflow](https://stackoverflow.com/questions/32496864/what-is-the-monomorphism-restriction) | [Haskell Wiki](https://wiki.haskell.org/Monomorphism_restriction)

The monomorphism restriction is a highly technical feature of Haskell’s type system which sometimes makes GHC infer unexpected types for definitions without an explicit type signature. Since GHC 7.8.1, it is on by default in compiled modules, but off by default in GHCi. `NoMonomorphismRestriction` allows you to switch it off even in compiled modules, but there are subtle disadvantages to this.

### `PostfixOperators`

Since 7.10.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-PostfixOperators) | [School of Haskell](https://www.schoolofhaskell.com/user/PthariensFlame/guide-to-ghc-extensions/basic-syntax-extensions#postfixoperators)

The operator section `(56 !)` is usually equivalent to `\y -> (!) 56 y`. With `PostfixOperators`, it is instead treated as `(!) 56`, so you can define and use postfix operators (though only with ugly parentheses):

```
(!) :: Int -> Int
(!) n = product [1..n]

test :: Int
test = (56 !)
```

### `PackageImports`

Since 6.10.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-PackageImports)

With this extension, an import can specify which package to import from:

```
import "network" Network.Socket
```

This can be used if two packages you depend on define the same module, though that’s very rare in practice.

## Comprehensions

The extensions in this section aren’t particularly bad; they just have very few users as far as I can tell, and I don’t give them much of a chance to get in a future Haskell standard (except maybe `ParallelListComp`).

### `ParallelListComp`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-ParallelListComp) | [24 Days](https://ocharles.org.uk/blog/guest-posts/2014-12-07-list-comprehensions.html)

This extension extends list comprehension syntax so you can write

```
[ x * y | x <- xs | y <- ys ]
```

instead of

```
[ x * y | (x, y) <- zip xs ys ]
```

### `TransformListComp`

Since 6.10.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-TransformListComp) | [24 Days](https://ocharles.org.uk/blog/guest-posts/2014-12-07-list-comprehensions.html)

This extension adds SQL-inspired constructs like `group by` and `using` to the list comprehension syntax. I’ve never seen this used in the wild.

### `MonadComprehensions`

Since 7.2.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-MonadComprehensions) | [24 Days](https://ocharles.org.uk/blog/guest-posts/2014-12-07-list-comprehensions.html)

As it turns out, list comprehension syntax can be sensibly used with any monad, not just lists. `MonadComprehensions` enables exactly that and includes the features from [`ParallelListComp`](https://limperg.de/ghc-extensions/#parallellistcomp) and [`TransformListComp`](https://limperg.de/ghc-extensions/#transformlistcomp). I would argue, however, that the comprehension syntax loses much of its intuitive appeal when used with other monads.

## Disabling Standard Features

### `NoTraditionalRecordSyntax`

Since 7.4.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-NoTraditionalRecordSyntax)

This disables the usual record syntax, like `C { f = x }`.

### `NoPatternGuards`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-NoPatternGuards)

This disables [pattern guards](https://wiki.haskell.org/Pattern_guard), which are a Haskell2010 feature similar to [`ViewPatterns`](https://limperg.de/ghc-extensions/#viewpatterns).

## Miscellaneous

### `Arrows`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-Arrows) | [24 Days](https://ocharles.org.uk/guest-posts/2014-12-21-arrows.html)

[Arrows](http://hackage.haskell.org/package/base-4.11.1.0/docs/Control-Arrow.html) are a generalisation of monads. The `Arrows` extension provides a notation for these constructs, similar to `do` notation for monads. Outside of the [Opaleye](http://hackage.haskell.org/package/opaleye) database library and certain ‘arrowised’ functional reactive programming libraries, arrows have not caught on in the community.

### `StaticPointers`

Since 7.10.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-StaticPointers) | [24 Days](https://ocharles.org.uk/guest-posts/2014-12-23-static-pointers.html)

Static Pointers allow you to get a reference to a closure (i.e. a computation, possibly with associated data) which can be serialised and deserialised. The intended use case is distributed programming, where computations can be turned into static pointers and then sent back and forth between nodes. However, as far as I know, the only user of this extension is [Cloud Haskell](http://haskell-distributed.github.io/).

### `ImplicitParams`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-ImplicitParams)

This extension allows you to have function parameters which are automatically propagated to calling functions. They have never caught on.

### `ExtendedDefaultRules`

Since 6.8.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/ghci.html#extension-ExtendedDefaultRules) | [Kwang Yul Seo](https://kseo.github.io/posts/2017-01-04-type-defaulting-in-haskell.html)

The type of

is ambiguous: `sum [1..100]` has type `(Num a, Enum a) => a` and `show` has type `Show a => a -> String` – so which `a` do we pick? `Int`, `Integer` and `Double` would all be candidates. For convenience, Haskell has a defaulting mechanism which kicks in here: `a` is defaulted to `Integer` based on its constraints. (With `-Wtype-defaults`, GHC warns you about this.) `ExtendedDefaultRules` applies this principle to more classes than standard Haskell. This is intended for GHCi, where `ExtendedDefaultRules` is enabled by default. In normal code, you usually want less defaulting, not more.

### `DeriveDataTypeable`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-DeriveDataTypeable) | [Chris Done](https://chrisdone.com/posts/data-typeable)

This extension allows you to derive instances of the [`Data`](http://hackage.haskell.org/package/base-4.11.1.0/docs/Data-Data.html) class automatically. This class supports “Scrap your Boilerplate”-style generic programming, which seems to have been mostly obsoleted by `GHC.Generics`\-style generic programming.

Recent GHCs derive instances of [`Typeable`](http://hackage.haskell.org/package/base-4.11.1.0/docs/Data-Typeable.html) without you even writing a `deriving` clause, so you don’t need `DeriveDataTypeable` for that anymore.

### `NPlusKPatterns`

Since 6.12.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-NPlusKPatterns)

n+k patterns are a misfeature of Haskell98 that has been removed for Haskell2010, so this extension is effectively deprecated.

### `ImpredicativeTypes`

Since 6.10.1 | Unstable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-ImpredicativeTypes)

Impredicative types have been effectively unsupported for a while. The User’s Guide says that “GHC has extremely flaky support for impredicative polymorphism”, which is probably a euphemism.

## Deprecated Extensions

### `DatatypeContexts`

Since 7.0.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-DatatypeContexts)

Haskell2010 allows you to put a context on datatype declarations:

```
data Ord a => Set a = ...
```

However, due to some weird design choices, this doesn’t work like you’d think and is pretty much useless.

### `NullaryTypeClasses`

Since 7.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-NullaryTypeClasses)

A special case of [`MultiParamTypeClasses`](https://limperg.de/ghc-extensions/#multiparamtypeclasses).

### `OverlappingInstances`, `IncoherentInstances`

Since 6.8.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-OverlappingInstances)

Consider the following situation:

```
class Foo a where
  f :: a -> a

instance Foo a where
  f = id

instance Foo [a] where
  f = reverse
```

The two instances overlap, meaning that if we look, say, for an instance `Foo [Int]`, both instances match. This is disallowed by default. With `OverlappingInstances`, it is allowed and the more specific instance (the second) will be chosen. With `IncoherentInstances`, GHC doesn’t complain even if there is no single most specific instance; it will choose an arbitrary one.

`OverlappingInstances` and `IncoherentInstances` are deprecated since GHC 7.10.1 because the same functionality is now provided by the `OVERLAPPING`, `OVERLAPPABLE`, `OVERLAPS` and `INCOHERENT` pragmas (also explained in the linked section of the User’s Guide). These are preferable because they apply to a single instance, whereas the extensions disable the overlap checks for an entire module.

### `TypeInType`

Since 8.0.1 | Unstable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-TypeInType)

In GHC versions prior to 8.6.1, `TypeInType` merges the type and kind languages of GHC, meaning that types and kinds become the same thing. This implies that the type `*` has kind `*`; hence the extension’s name. It also means that everything we can do at the type level (type synonyms, type families, etc.), we can also do at the kind level.

Since GHC 8.6.1, `TypeInType` is a deprecated alias of [`PolyKinds`](https://limperg.de/ghc-extensions/#polykinds), [`DataKinds`](https://limperg.de/ghc-extensions/#datakinds) and [`KindSignatures`](https://limperg.de/ghc-extensions/#kindsignatures). Its functionality has been integrated into these other extensions.

### `Rank2Types`

Since 6.8.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-Rank2Types)

A more restricted form of [`RankNTypes`](https://limperg.de/ghc-extensions/#rankntypes).

## Extensions That Only Make Sense With Other Extensions

### `ExplicitNamespaces`

Since 7.6.1 | Mostly stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-ExplicitNamespaces)

With [`TypeOperators`](https://limperg.de/ghc-extensions/#typeoperators), you can define the type `(++)`. If you want to export this type while the function `(++)` is also in scope, you need to be able to distinguish the two in the module export list. `ExplicitNamespaces` allows you to do so, by writing

```
module M ((++), type (++)) where ...
```

The same works when importing from `M`. `ExplicitNamespaces` also works with [pattern synonyms](https://limperg.de/ghc-extensions/#patternsynonyms), which can be prefixed with `pattern`. Both `TypeOperators` and `PatternSynonyms` imply `ExplicitNamespaces`, so you don’t need to enable it manually.

### `MonoLocalBinds`

Since 6.12.1 | Stable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-MonoLocalBinds) | [Simon Peyton-Jones](https://ghc.haskell.org/trac/ghc/blog/LetGeneralisationInGhc7)

`Let` and `where` bindings without an explicit type signature are usually generalised as much as possible. With `MonoLocalBinds`, they are generalised a little less. This leads to more predictable type inference when using [type families](https://limperg.de/ghc-extensions/#typefamilies) or [GADTs](https://limperg.de/ghc-extensions/#gadts), so these two extensions imply `MonoLocalBinds`.

## Temporary Extensions

### `MonadFailDesugaring`

Since 8.0.1 | Unstable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-MonadFailDesugaring)

When a pattern match fails in a `do` block, the `Monad` class’s `fail` method is called with an error message. For many monads, `fail` is simply `error`, which introduces implicit partiality, which is no good.

As part of the [`MonadFail` proposal](https://prime.haskell.org/wiki/Libraries/Proposals/MonadFail), `do` syntax will in future use a subclass of `Monad`, `MonadFail`, when a `do` block contains a pattern that may fail. `MonadFailDesugaring` enables this behaviour. The extension is temporary because it will eventually become standard. `MonadFailDesugaring` is turned on by default since GHC 8.6.1.

### `StarIsType`

Since 8.6.1 | Unstable | [User’s Guide](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-StarIsType)

In standard Haskell, `*` is the kind of all types (i.e. `Int :: *`, etc). This is an unfortunate choice of syntax because, among other things, it conflicts with [`TypeOperators`](https://limperg.de/ghc-extensions/#typeoperators): `Int * Bool` should probably be the type operator `(*)` applied to `Int` and `Bool`, rather than `Int` applied to `*` and `Bool`.

With `NoStarIsType`, `*` becomes a regular type operator with no special meaning. (To refer to the kind of all types, use `Data.Kind.Type` instead.) As of GHC 8.6.1, `StarIsType` is enabled by default, but this default will change in the future. Note that even with `StarIsType`, GHC 8.6.1 removes some special parsing rules for `*`, breaking backwards compatibility. See the [migration plan](https://ghc.haskell.org/trac/ghc/wiki/Migration/8.6#StarIsType).

___

1.  Don’t let anyone tell you that Haskellers are bad at naming.[↩︎](https://limperg.de/ghc-extensions/#fnref1)
