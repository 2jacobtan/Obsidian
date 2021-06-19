Been reading this paper, and getting a better picture of the fundamentals of Answer Set Programming (ASP).

In preparation for the presentation later by the s(CASP) author.

>Answer Set Programming: A Primer

http://www.kr.tuwien.ac.at/staff/tkren/pub/2009/rw2009-asp.pdf

---

### stable models do not cover some possible models (for some programs)

![[Pasted image 20210617202910.png]]
(from Answer Set Programming: A Primer, p22)

```
s :- not q.
q :- not s.
p :- q, not s.
f :- s, not f.
```

![[Pasted image 20210617224009.png]]

\
On the other hand, SAT solver shows more models that are incompatible with the stable model.

Solutions #1 #4 #5 cover the possible models described by the stable model {p, q}.

Solutions #2 #3 #6 are incompatible with the stable model {p, q}.

Also, notice that the stable model does not inform that {s = True, f = False} is not compatible with the program. I.e. {p = True, q = True} alone is not sufficient to satisfy the program, and ASP does not reveal this in its answer.

```
*Rule34_SBV PPrint> mainAllSat 

[ stable model {p, q} is covered by the following 3 SAT solutions ]

Solution #1:
  p =  True :: Bool
  q =  True :: Bool
  s = False :: Bool
  f = False :: Bool
Solution #4:
  p = True :: Bool
  q = True :: Bool
  s = True :: Bool
  f = True :: Bool
Solution #5:
  p =  True :: Bool
  q =  True :: Bool
  s = False :: Bool
  f =  True :: Bool

[ {s = True, f = False}, is not possible due to ( f <- s, not f ) ]

Solution #2:
  p = False :: Bool
  q = False :: Bool
  s =  True :: Bool
  f =  True :: Bool
Solution #3:
  p = False :: Bool
  q =  True :: Bool
  s =  True :: Bool
  f =  True :: Bool
Solution #6:
  p =  True :: Bool
  q = False :: Bool
  s =  True :: Bool
  f =  True :: Bool
Found 6 different solutions.
```
Haskell code using SBV (an SMT-LIB api): https://github.com/smucclaw/sandbox/blob/89347c4aad76570cfd4984816a6ebd010eab8c42/jacobtan/Rule34-logic-gates/rule34-haskell/src/ASP_SBV.hs#L10-L10

\
Furthermore, sCASP produces results different from ASP and SMT (see below).

I'm unsure of why or how sCASP's semantics differs so much from ASP.

```
#abducible s.
#abducible q.
#abducible p.
#abducible f.

s :- not q.
q :- not s.
p :- q, not s.
f :- s, not f.

?- true.
```

```
┌─[20210617-21:54:38] ‡ [jt2@JACOBTAN039FL:~/repos/smucclaw/sandbox/jacobtan/Rule34-logic-gates/rule34-haskell/src]
└─[0] <git:(default ac1bd36✱✈) > scasp -s0 ASP_sCASP.pl
QUERY:?- true.

        ANSWER: 1 (in 0.229 ms)

MODEL:
{ not s,  q }

BINDINGS:


        ANSWER: 2 (in 0.407 ms)

MODEL:
{ not s,  q }

BINDINGS:


        ANSWER: 3 (in 0.731 ms)

MODEL:
{ s,  not q,  f }

BINDINGS:


        ANSWER: 4 (in 0.751 ms)

MODEL:
{ s,  f }

BINDINGS:

```

---

stable models ⊆ least models ⊆ (all possible) models