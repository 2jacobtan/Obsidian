
https://softwarefoundations.cis.upenn.edu/
> **The Software Foundations series is a broad introduction to the mathematical underpinnings of reliable software.**
> 
> The principal novelty of the series is that every detail is one hundred percent formalized and machine-checked: the entire text of each volume, including the exercises, is literally a "proof script" for the Coq proof assistant.
> 
> The exposition is intended for a broad range of readers, from advanced undergraduates to PhD students and researchers. No specific background in logic or programming languages is assumed, though a degree of mathematical maturity is helpful. A one-semester course can expect to cover _Logical Foundations_ plus most of _Programming Language Foundations_ or _Verified Functional Algorithms_, or selections from both.

---

(https://softwarefoundations.cis.upenn.edu/lf-current/Tactics.html)
# Review

We've now seen many of Coq's most fundamental tactics. We'll introduce a few more in the coming chapters, and later on we'll see some more powerful _automation_ tactics that make Coq help us with low-level details. But basically we've got what we need to get work done.

Here are the ones we've seen:

-   intros: move hypotheses/variables from goal to context
    
-   reflexivity: finish the proof (when the goal looks like e \= e)
    
-   apply: prove goal using a hypothesis, lemma, or constructor
    
-   apply... in H: apply a hypothesis, lemma, or constructor to a hypothesis in the context (forward reasoning)
    
-   apply... with...: explicitly specify values for variables that cannot be determined by pattern matching
    
-   simpl: simplify computations in the goal
    
-   simpl in H: ... or a hypothesis
    
-   rewrite: use an equality hypothesis (or lemma) to rewrite the goal
    
-   rewrite ... in H: ... or a hypothesis
    
-   symmetry: changes a goal of the form t\=u into u\=t
    
-   symmetry in H: changes a hypothesis of the form t\=u into u\=t
    
-   transitivity y: prove a goal x\=z by proving two new subgoals, x\=y and y\=z
    
-   unfold: replace a defined constant by its right-hand side in the goal
    
-   unfold... in H: ... or a hypothesis
    
-   destruct... as...: case analysis on values of inductively defined types
    
-   destruct... eqn:...: specify the name of an equation to be added to the context, recording the result of the case analysis
    
-   induction... as...: induction on values of inductively defined types
    
-   injection... as...: reason by injectivity on equalities between values of inductively defined types
    
-   discriminate: reason by disjointness of constructors on equalities between values of inductively defined types
    
-   assert (H: e) (or assert (e) as H): introduce a "local lemma" e and call it H
    
-   generalize dependent x: move the variable x (and anything else that depends on it) from the context back to an explicit hypothesis in the goal formula
    
-   f\_equal: change a goal of the form f x \= f y into x \= y

---