---
created: 2021-06-22T00:40:08 (UTC +08:00)
tags: []
source: https://stackoverflow.com/questions/52367608/differences-gadt-data-family-data-family-that-is-a-gadt
author: AntC
---

# haskell - differences: GADT, data family, data family that is a GADT - Stack Overflow

> ## Excerpt
> What/why are the differences between those three? Is a GADT (and regular data types) just a shorthand for a data family? Specifically what's the difference between:

data GADT a  where
  MkGADT :: ...

---
I think the difference becomes clear if we use `PatternSynonyms`\-style type signatures for data constructors. Lets start with Haskell 98

```
data D a = D a a
```

You get a pattern type:

```
pattern D :: forall a. a -> a -> D a
```

it can be read in two directions. `D`, in "forward" or expression contexts, says, "`forall a`, you can give me 2 `a`s and I'll give you a `D a`". "Backwards", as a pattern, it says, "`forall a`, you can give me a `D a` and I'll give you 2 `a`s".

Now, the things you write in a GADT definition are not pattern types. What are they? Lies. Lies lies lies. Give them attention only insofar as the alternative is writing them out manually with `ExistentialQuantification`. Let's use this one

```
data GD a where
  GD :: Int -> GD Int
```

You get

```

pattern GD :: forall a. () => (a ~ Int) => Int -> GD a
```

This says: `forall a`, you can give me a `GD a`, and I can give you a proof that `a ~ Int`, plus an `Int`.

Important observation: The return/match type of a GADT constructor is _always_ the "data type head". I defined `data GD a where ...`; I got `GD :: forall a. ... GD a`. This is also true for Haskell 98 constructors, and also `data family` constructors, though it's a bit more subtle.

If I have a `GD a`, and I don't know what `a` is, I can pass into `GD` anyway, even though I wrote `GD :: Int -> GD Int`, which seems to say I can only match it with `GD Int`s. This is why I say GADT constructors lie. The pattern type never lies. It clearly states that, `forall a`, I can match a `GD a` with the `GD` constructor and get evidence for `a ~ Int` and a value of `Int`.

Ok, `data family`s. Lets not mix them with GADTs yet.

```
data Nat = Z | S Nat
data Vect (n :: Nat) (a :: Type) :: Type where
  VNil :: Vect Z a
  VCons :: a -> Vect n a -> Vect (S n) a 

data family Rect (ns :: [Nat]) (a :: Type) :: Type
newtype instance Rect '[] a = RectNil a
newtype instance Rect (n : ns) a = RectCons (Vect n (Rect ns a))
```

There are actually _two_ data type heads now. As @K.A.Buhr says, the different `data instance`s act like different data types that just _happen_ to share a name. The pattern types are

```
pattern RectNil :: forall a. a -> Rect '[] a
pattern RectCons :: forall n ns a. Vect n (Rect ns a) -> Rect (n : ns) a
```

If I have a `Rect ns a`, and I don't know what `ns` is, _I cannot match on it_. `RectNil` only takes `Rect '[] a`s, `RectCons` only takes `Rect (n : ns) a`s. You might ask: "why would I want a reduction in power?" @KABuhr has given one: GADTs are closed (and for good reason; stay tuned), families are open. This doesn't hold in `Rect`'s case, as these instances already fill up the entire `[Nat] * Type` space. The reason is actually `newtype`.

Here's a GADT `RectG`:

```
data RectG :: [Nat] -> Type -> Type where
  RectGN :: a -> RectG '[] a
  RectGC :: Vect n (RectG ns a) -> RectG (n : ns) a
```

I get

```

pattern RectGN :: forall ns a. () => (ns ~ '[]) => a -> RectG ns a
pattern RectGC :: forall ns' a. forall n ns. (ns' ~ (n : ns)) =>
                  Vect n (RectG ns a) -> RectG ns' a


```

If I have a `RectG ns a` and don't know what `ns` is, I can still match on it just fine. The compiler has to preserve this information with a data constructor. So, if I had a `RectG [1000, 1000] Int`, I would incur an overhead of one million `RectGN` constructors that all "preserve" the same "information". `Rect [1000, 1000] Int` is fine, though, as I do not have the ability to match and tell whether a `Rect` is `RectNil` or `RectCons`. This allows the constructor to be `newtype`, as it holds no information. I would instead use a different GADT, somewhat like

```
data SingListNat :: [Nat] -> Type where
  SLNN :: SingListNat '[]
  SLNCZ :: SingListNat ns -> SingListNat (Z : ns)
  SLNCS :: SingListNat (n : ns) -> SingListNat (S n : ns)
```

that stores the dimensions of a `Rect` in `O(sum ns)` space instead of `O(product ns)` space (I think those are right). This is also why `GADT`s are closed and families are open. A GADT is just like a normal `data` type except it has equality evidence and existentials. It doesn't make sense to add constructors to a GADT any more than it makes sense to add constructors to a Haskell 98 type, because any code that doesn't know about one of the constructors is in for a _very_ bad time. It's fine for families though, because, as you noticed, once you define a branch of a family, you cannot add more constructors in that branch. Once you know what branch you're in, you know the constructors, and no one can break that. You're not allowed to use any constructors if you don't know which branch to use.

Your examples don't really mix GADTs and data families. Pattern types are nifty in that they normalize away superficial differences in `data` definitions, so let's take a look.

```
data family FGADT a
data instance FGADT a where
  MkFGADT :: Int -> FGADT Int
```

Gives you

```
pattern MkFGADT :: forall a. () => (a ~ Int) => Int -> FGADT a

```

But

```
data family DF a
data instance DF Int where
  MkDF :: Int -> DF Int
```

gives

```
pattern MkDF :: Int -> DF Int

```

Here's a proper mixing

```
data family Map k :: Type -> Type
data instance Map Word8 :: Type -> Type where
  MW8BitSet :: BitSet8 -> Map Word8 Bool
  MW8General :: GeneralMap Word8 a -> Map Word8 a
```

Which gives patterns

```
pattern MW8BitSet :: forall a. () => (a ~ Bool) => BitSet8 -> Map Word8 a
pattern MW8General :: forall a. GeneralMap Word8 a -> Map Word8 a
```

If I have a `Map k v` and I don't know what `k` is, I can't match it against `MW8General` or `MW8BitSet`, because those only want `Map Word8`s. This is the `data family`'s influence. If I have a `Map Word8 v` and I don't know what `v` is, matching on the constructors _can_ reveal to me whether it's known to be `Bool` or is something else.
