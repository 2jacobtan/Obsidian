
https://twitter.com/lorisdanto/status/1354128808740327425
![[Pasted image 20210601024528.png]]

![[Pasted image 20210601024702.png]]

![[Pasted image 20210601024734.png]]

```haskell
all' f = foldr ((&&) . f) True
any' f = foldr ((||) . f) False

main = do
  print $ all' (const True) <$> [[1],[]]
  print $ any' (const True) <$> [[1],[]]
  print $ all' (const False) <$> [[1],[]]
  print $ any' (const False) <$> [[1],[]]
```
https://replit.com/@JacobTan2/predicate-logic#main.hs

![[Pasted image 20210601025048.png]]