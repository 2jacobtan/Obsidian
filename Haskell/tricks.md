
https://twitter.com/chris__martin/status/1401242486702428164

```hs
traverseSet' :: (Foldable t, Monoid (f (Set a1)), Functor f) =>
  (a2 -> f a1) -> t a2 -> f (Set a1)
traverseSet' f = foldMap (fmap Set.singleton . f)
```

Note: the constraints `Monoid (f (Set a1)), Functor f` are actually satisfied by `Applicative f` because of the mechanically derivable `instance (Applicative f, Monoid a) => Monoid (Ap f a)`.

Cf. https://hackage.haskell.org/package/base-4.15.0.0/docs/Data-Monoid.html#t:Ap

I.e. `f a` is automatically a monoid if `f` is applicative and `a` is monoid.


E.g.
```hs
-- | @since 4.10.0.0
instance Semigroup a => Semigroup (IO a) where
    (<>) = liftA2 (<>)

-- | @since 4.9.0.0
instance Monoid a => Monoid (IO a) where
    mempty = pure mempty
```

https://hackage.haskell.org/package/base-4.15.0.0/docs/src/GHC-Base.html

---