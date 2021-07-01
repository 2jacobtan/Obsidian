
### thoughts on logic programming's limitations

Like other paradigms, logic programming has no first-class support for the non-monotonic reasoning commonly used in law.

> Yes non-monotonicity (NM) is needed and people have used workarounds in all kind of languages but my point is that  no programming language used in industry (to my knowledge) has first-class support for it.
>
> You can embed NM in logic programming but also in other paradigms

https://twitter.com/DMerigoux/status/1397981013784018944

Functional programming can do logic too (perhaps even better; curry-howard correspondence).

E.g. https://arxiv.org/abs/2103.03198

---

Domain modelling, the elephant in the room.

15m57s, Michael Genesereth:
> Developing a good domain ontology is the hardest part of writing a good computable contract. Yet when we have meetings of folks who are interested in computable contracts, we spend more time talking about the other components and less time talking about domain ontologies. Yet I think it is the most important part and the most difficult part of writing a good computable contract.

https://www.youtube.com/watch?v=cc5C3FPABAQ&t=957s

Logic programming has limited domain modelling capabilities.

Functional programming, on the other hand, is excellent (algebraic data types).

---

Logic programming's primary strength is in having a built-in search algorithm for finding models that satisfy some set of constraints (within a highly restrictive language).

But functional programming is perfectly capable of doing the same, and much more (with a strong type system, data structures, etc.).

E.g. https://www.cs.yale.edu/homes/piskac/papers/2019HallahanETALquasiquoter.pdf

There's symbolic execution, SMT solving, fuzzing, output a program trace for explainability, etc., all kinds of formal methods readily available.

---

As mentioned elsewhere, logic programming has complicated and unintuitive semantics.

Meanwhile, functional programming has clear semantics that is easy to understand and also transpile to other programming languages.