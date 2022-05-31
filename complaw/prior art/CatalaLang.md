Catala: A Programming Language for the Law  
[https://arxiv.org/abs/2103.03198](https://arxiv.org/abs/2103.03198)

\_\_summary\_\_

- (p3) neatly translate legal text (that is computational in nature) into a computer-executable specification

> In this work, we propose a new language, Catala, tailored specifically for the purpose of faithfully, crisply translating computational law into executable specifications. Catala is designed to follow the existing structure of legal statutes, enabling a one-to-one correspondence between a legal paragraph and its corresponding translation in Catala. Under the hood, Catala uses prioritized default logic \[5\]; to the best of our knowledge, Catala is the first instance of a programming language designed with this logic as its core system. Catala has clear semantics, and compiles to a generic lambda-calculus that can then be translated to any existing language.

- (p4) legal statutes appear to be similar to pattern matching in functional programming, but with cases ordered from general to specific

> In a functional programming language, a pattern-match first lists the most specific matching cases, and catches all remaining cases with a final wildcard. A text of law proceeds in the exact opposite way: the general case in (a) above is listed first, then followed by limitations that modify the general case under certain conditions. This is by design: legal statutes routinely follow a “general case first, special cases later” approach which mirrors the legislator’s intentions.

- (p21) formally specified compiler
![[Pasted image 20210803131710.png]]


- (p23) in a style of pure functional programming (i.e. lambda calculus with syntax sugar)

> We don’t expect adoption difficulties from the programmers’ side, since Catala is morally a pure functional language with a few oddities that makes it well-suited to legal specifications


- (p27) formal methods is not addressed

> With its clear and simple semantics, we hope for Catala formalizations of statutes to provide ideal starting point for future formal analyses of the law, enabling legal drafting, interpretation, simplification and comparison using the full arsenal of modern formal methods.

My guess is that Catala is designed with computer execution in mind, and does not have explainability / symbolic execution / formal methods features built in.

E.g. no mention of automata / petri nets.

---

\_\_extras\_\_

- (p10) no general recursion; legal text does not have circular reasoning

> While Catala at first glance resembles a functional language with heavy syntactic sugar, diving into the subtleties of the law exhibits the need for two features that are not generally found in lambda-calculi. First, we allow the user to define context variables through a combination of an (optional) default case, along with an arbitrary number of special cases, either prioritized or non-overlapping. The theoretical underpinning of this feature is the prioritized default calculus. Second, the out-of-order nature of definitions means that Catala is entirely declarative, and it is up to the Catala compiler to compute a suitable dependency order for all the definitions in a given program. Fortunately, the law does not have general recursion, meaning that we do not need to compute fixed points, and do not risk running into circular definitions. Hence, our language is not Turing-complete, purposefully.


\_\_random\_\_

> Are Circular Arguments Necessarily Vicious?

[https://www.jstor.org/stable/20014107](https://www.jstor.org/stable/20014107)