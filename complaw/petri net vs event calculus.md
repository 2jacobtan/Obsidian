#SMT, [[SMT (satisfiability modulo theories)]]
#event_calculus
[[petri nets]]

\
I think graph-based formats might be key to human-friendly UI/UX (visualisation and interaction). E.g. \[insurance company\] used railroad diagrams to make it easy for non-technical staff to build a contract.

---

While it's true that laws use logical language extensively, they also refer to real-world "concepts" or "theories" which are not so conveniently modelled via logic programming.This is analogous to the relationship between SAT solvers and SMT solvers. SMT = satisfiability modulo theories. "Satisfiability" is the logic portion, while "theories" is the non-logic portion. The "theories" used by SMT solvers have to be programmed using a general-purpose programming language. (In my not-so-informed opinion.)

[https://people.eecs.berkeley.edu/~sseshia/pubdir/SMT-BookChapter.pdf](https://people.eecs.berkeley.edu/~sseshia/pubdir/SMT-BookChapter.pdf)  

> Applications in artificial intelligence and formal methods for hardware and software development have greatly benefited from the recent advances in SAT. Often, however, applications in these fields require determining the satisfiability of formulas in more expressive logics such as first-order logic. **Despite the great progress made in the last twenty years, general-purpose first-order theorem provers (such as provers based on the resolution calculus) are typically not able to solve such formulas directly.** The main reason for this is that many applications require not general first-order satisfiability, but rather satisfiability with respect to some background theory, which fixes the interpretations of certain predicate and function symbols. For instance, applications using integer arithmetic are not interested in whether there exists a nonstandard interpretation of the symbols <, +, and 0 that makes the formula x < y ∧ ¬(x < y + 0) satisfiable. Instead, they are interested in whether the formula is satisfiable in an interpretation in which < is the usual ordering over the integers, + is the integer addition function, and 0 is the additive identity. General-purpose reasoning methods can be forced to consider only interpretations consistent with a background theory T , but only by explicitly incorporating the axioms for T into their input formulas. **Even when this is possible,^1 the performance of such provers is often unacceptable. For some background theories, a more viable alternative is to use reasoning methods tailored to the theory in question.** This is particularly the case for quantifier-free formulas, first-order formulas with no quantifiers but possibly with variables, such as the formula above.

^b1be40


> ^1 Some background theories such as the theory of real numbers or the theory of finite trees, cannot be captured by a finite set of first-order formulas, or, as in the case of the theory of integer arithmetic (with multiplication), by any decidable set of first-order formulas.

> The main advantage of theory-specific solvers is that one can use whatever specialized algorithms and data structures are best for the theory in question, which typically leads to better performance. The common practice is to write theory solvers just for conjunctions of literals—i.e., atomic formulas and their negations. These pared down solvers are then embedded as separate submodules into an efficient SAT solver, allowing the joint system to accept quantifier-free formulas with an arbitrary Boolean structure.

---

That said, event calculus (which just depends on Horn clauses, with negation as failure) seems to be useful for dealing with processes/events, directly in Prolog / sCASP.

[https://www.researchgate.net/publication/235736569\_Monitoring\_Business\_Constraints\_with\_the\_Event\_Calculus](https://www.researchgate.net/publication/235736569_Monitoring_Business_Constraints_with_the_Event_Calculus)
> Since the majority of EC-based approaches rely on the Horn clause fragment of first order logic with negation as failure \[5\], we will use Prolog as the specification language.

\
[https://user.ceng.metu.edu.tr/~nihan/pub/ECandWFs.pdf](https://user.ceng.metu.edu.tr/~nihan/pub/ECandWFs.pdf)

---

https://www.doc.ic.ac.uk/~rak/papers/event%20calculus.pdf
> A Logic-based Calculus of Events
> ROBERT KOWALSKI and MAREK SERGOT

> In this paper, we shall outline a treatment of time, based on the notion of event, formalized in the Horn clause subset of classical logic augmented with negation as failure. The resulting formalization is executable as a logic program.