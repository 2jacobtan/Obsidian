---
created: 2021-05-29T04:21:28 (UTC +08:00)
tags: []
source: https://www.cis.upenn.edu/~plclub/blog/2020-10-09-hs-to-coq/
author: 
---

# Tutorial: Verify Haskell Programs with hs-to-coq

> ## Excerpt
> University of Pennsylvania Programming Languages Club

---
Tags: [haskell](https://www.cis.upenn.edu/~plclub/blog/tags/haskell.html "All pages tagged 'haskell'."), [coq](https://www.cis.upenn.edu/~plclub/blog/tags/coq.html "All pages tagged 'coq'.")

___

Oct 9 2020  
By **Li-yao Xia**  

## Preface

Programming is the power of automation at our fingertips. A task, a program, can be run over and over, billions of instructions per second. And with great power comes great responsibility. A program can also fail, over and over, billions of instructions per second. Many solutions have been devised to reduce the likelihood and impact of human error in programming, testing and type systems being prominent examples. Another approach is formal verification.

Proving programs correct is possible, but it is also very hard. Writing a rigorous proof requires knowledge and discipline which take years of expensive training and experience to acquire. Even with the necessary skills in hand, applying them requires sustained effort and perseverance to succeed.

Automating logic can reduce the cost of formal reasoning. Proof assistants create an objective standard of rigor: if a proof type-checks, it is correct. The objective is to codify reasoning that people are already doing in their heads. Even a relatively untrained programmer must have some intuitive idea of why their program is correct; a proof assistant helps users fill the gaps in their reasoning, and point out parts where more attention is required. Proof assistants mechanize reasoning like compilers mechanize programming, translating high-level programs to execute on machines which understand low-level instructions.

Much work remains to be done to realize that vision practically, but there are good reasons to be hopeful. Proof assistants such as Isabelle, Coq, Agda, and Lean are already available, and they are here to stay. Rather than tools for experts, they can be vessels to [teach](http://wwwf.imperial.ac.uk/~buzzard/xena/natural_number_game/) [formal](https://softwarefoundations.cis.upenn.edu/) [methods](https://plfa.github.io/), and bridges to bring cutting-edge knowledge into the hands of non-specialists.

One of perhaps many barriers to the adoption of proof assistants is that they are most commonly used to formalize models that are nevertheless disconnected from the programs that eventually run. Projects such as [VST](https://github.com/PrincetonUniversity/VST) (for C) and [_hs-to-coq_](https://github.com/antalsz/hs-to-coq) aim to bring down that barrier, allowing programs in widely used programming languages to be the actual objects of study.

This post is a practical introduction to using the _hs-to-coq_ tool to verify Haskell programs in [Coq](https://coq.inria.fr/). You may already be curious about interactive theorem proving and its [close ties](https://en.wikipedia.org/wiki/Curry%E2%80%93Howard_correspondence) to functional programming. This tutorial shows a glimpse of interactive theorem proving in practice. It is a skill you can pick up like another programming language, and you can start to apply it to verify libraries and programs.

### Haskell to Coq

Coq and Haskell are both pure functional languages, and thus closely related both syntactically and semantically. The [_hs-to-coq_](https://github.com/antalsz/hs-to-coq) project leverages that similarity to translate Haskell programs into Coq, so they can then be subjected to mechanized formal specification and proof.

_hs-to-coq_ has already been used to verify subsets of the libraries _base_ (Haskell’s standard library) and _containers_ (in particular, `Map`, `Set`, and `IntSet`). A [verified version of containers](https://hackage.haskell.org/package/containers-verified) is available on Hackage.

## Tutorial

In this tutorial, we will walk through the verification of a simple data structure: a purely functional queue.

Familiarity with basic concepts of functional programming is assumed (algebraic data types, pattern-matching, and recursive functions) and some determination to decipher the explanations of Coq code is recommended.

This tutorial can be followed by just reading along or by replaying the examples on your own.

Instructions and code to copy will be framed like this.

With some additional experience using Coq—for instance after completing the first one or two volumes of [Software Foundations](https://softwarefoundations.cis.upenn.edu/)—you can use this tutorial as a template to prove properties of Haskell programs in Coq.

### Plan

To start, create the following directories and clone the _hs-to-coq_ repository:

```
mkdir src src-coq theories
git clone https://github.com/antalsz/hs-to-coq
```

The complete files produced in this tutorial can be found in [this repository](https://gitlab.com/lysxia/hs-to-coq-tutorial). It also contains additional setup information, in particular instructions to install Coq and details of software versions.

This tutorial will go through these steps in detail:

1.  Write a Haskell module `src/Queue.hs`.
2.  Use _hs-to-coq_ to translate it to a Coq module `src-coq/Queue.v`.
3.  Formalize a specification and prove that the Coq module conforms to that specification in `theories/QueueSpec.v`.

The directories and files in order of apparition:

```
                    ### Haskell ###
src/
    Queue.hs        # Haskell implementation of a Queue

                    ### hs-to-coq ###

hs-to-coq/          # The hs-to-coq repository

edits               # "Edit file" adjusting the translation of Queue.hs

                    ### Coq ###
src-coq/
    Queue.v         # Coq-ified translation of Queue.hs

_CoqProject         # Coq project configuration

theories/
    QueueSpec.v     # Specification and proof
```

### Implementation of a queue

In this section, we implement a queue in Haskell, in the file `Queue.hs`.

A queue is a data structure with two main operations:

-   we can _push_ a new element to the end a queue, which yields a longer queue;
-   we can _pop_ an element from the front of a queue, which shortens the queue and gives us the removed element; popping an empty queue gives us `Nothing`.

Thus, elements are _popped_ in the order they were _pushed_, in “first in, first out” order. We also need an empty queue to initialize things. Put together, the interface we will be working with is given by the following type signatures:

```
empty :: Queue a
push :: a -> Queue a -> Queue a
pop :: Queue a -> Maybe (a, Queue a)
```

There are many ways to implement a queue. Here, we will use a classical implementation using two lists.

The main idea is to maintain two parts of the queue separately: the _front_ and the _back_.

```
data Queue a = Queue { front :: [a], back :: [a] }

empty :: Queue a
empty = Queue [] []
```

We _push_ an element `x` with a simple _cons_ `(:)` to the back.

```
push :: a -> Queue a -> Queue a
push x (Queue front back) = Queue front (x : back)
```

Elements can be _popped_ by pattern-matching on the front. We remove the head `y` from the front if it is not empty. If the front is empty, we reverse the back, and it becomes the new front, from which we remove the first element. We fail with `Nothing` if both the front and back are empty.

```
pop :: Queue a -> Maybe (a, Queue a)
pop (Queue (y : front) back) = Just (y, Queue front back)
pop (Queue [] back)
  = case reverse back of
      [] -> Nothing
      y : front -> Just (y, Queue front [])
```

This implementation ensures that the expensive cost of reversing lists in `pop` is _amortized_ across many operations, so the average cost per operation is constant.

A detailed analysis of this structure can be found in Chris Okasaki’s thesis ([_Purely Functional Data Structures_](https://www.cs.cmu.edu/~rwh/theses/okasaki.pdf) (1996), Section 3.1.1), which includes many more examples of purely functional data structures.

For more convenient testing, let’s define an auxiliary function to `pop` all elements from a queue, until it comes out empty.

```
-- A function to use pop many times.
toList :: Queue a -> [a]
toList q
  = case pop q of
      Nothing -> []
      Just (y, q') -> y : toList q'
```

Put all of that Haskell code in a file `src/Queue.hs`. We can give it a quick try in ghci:

```
ghci src/Queue.hs                             -- Start ghci
ghci> let q = push 3 (push 2 (push 1 empty))  -- Use push
ghci> let Just (x, q') = pop q                -- Use pop once
ghci> x                                       -- x should be the first element
1
ghci> let q'' = push 4 q'                     -- push once more
ghci> toList q''                              -- Use pop until the end
[2,3,4]                                       -- Output
```

Complete Haskell module `Queue.hs`

```
module Queue where

data Queue a = Queue { front :: [a], back :: [a] }

empty :: Queue a
empty = Queue [] []

push :: a -> Queue a -> Queue a
push x (Queue front back) = Queue front (x : back)

pop :: Queue a -> Maybe (a, Queue a)
pop (Queue (y : front) back) = Just (y, Queue front back)
pop (Queue [] back)
  = case reverse back of
      [] -> Nothing
      y : front -> Just (y, Queue front [])

toList :: Queue a -> [a]
toList q
  = case pop q of
      Nothing -> []
      Just (y, q') -> y : toList q'
```

### Translate Haskell to Coq

Let’s use the _hs-to-coq_ tool to translate our Haskell module into a Coq module.

#### Installation

_hs-to-coq_ can be installed from its source repository using cabal or stack. It takes a couple of minutes to build—including all dependencies.

```
cd hs-to-coq
stack install
make -C base
make -C base-thy
```

#### Usage

We will now translate `src/Queue.hs`, which we got from the previous section, into `src-coq/Queue.v` (`.v` is the extension for Coq files).

The tool will need a bit of help to process our Haskell source, in the form of an _edits file_.

Create a file `edits` containing the following two lines:

```
rename value Queue.Queue = Queue.MkQueue
skip Queue.toList
```

Explanation:

-   we rename the `Queue` data constructor (not the type constructor) to `MkQueue`, so it has a different name from the type `Queue`, as they live in the same namespace in Coq;
-   we won’t translate `toList`, so it will not be verified: the problem is that in Coq, all functions must terminate, and that is ensured by restricting the syntax of recursive functions; translating `toList` requires more advanced features than discussed in this tutorial (see also [termination edits](https://hs-to-coq.readthedocs.io/en/latest/edits.html#termination-edits)).

Remark that names in edits files must be fully qualified to prevent ambiguity.

Invoke _hs-to-coq_ with the following command:

```
hs-to-coq -e hs-to-coq/base/edits --iface-dir hs-to-coq/base -e edits src/Queue.hs -o src-coq
```

That produces a file `src-coq/Queue.v`. Our first Coq module!

Explanation:

-   `hs-to-coq`: the translation program we just installed;
-   `-e hs-to-coq/base/edits --iface-dir hs-to-coq/base`: those options tell the program where to find edits for standard types and functions, and the Coq-ified version of the standard library (these will hopefully go away once _hs-to-coq_ gets properly packaged);
-   `-e edits`: this adds our own edits to the translation;
-   `src/Queue.hs`: the file we want to translate;
-   `-o src-coq`: where to put the translated file `Queue.v`.

Compile the Coq module `src-coq/Queue.v`.

```
coqc -R hs-to-coq/base "" -Q src-coq/ Src src-coq/Queue.v
```

That produces a binary file `src-coq/Queue.vo`. `.vo` is the extension for Coq object files. It will be needed to build new modules depending on the `Queue` module.

Explanation:

-   `coqc`: the Coq compiler;
-   `-R hs-to-coq/base "" -Q src-coq/ Src`: these options
    1.  specify additional paths where Coq can find modules to be imported;
    2.  sets the paths of modules currently being compiled: `src-coq/Queue.vo` defines the module `Src.Queue`.
-   `src-coq/Queue.v`: our Coq module.

Complete Coq module `Queue.v`

(\* Default settings (from HsToCoq.Coq.Preamble) \*)

Generalizable All Variables.

Unset Implicit Arguments.
Set Maximal Implicit Insertion.
Unset Strict Implicit.
Unset Printing Implicit Defensive.

Require Coq.Program.Tactics.
Require Coq.Program.Wf.

(\* Converted imports: \*)

Require Coq.Lists.List.

(\* Converted type declarations: \*)

Inductive Queue a : Type
  := | MkQueue (front : list a) (back : list a) : Queue a.

Arguments MkQueue {\_} \_ \_.

Definition back {a} (arg\_0\_\_ : Queue a) :=
  let 'MkQueue \_ back := arg\_0\_\_ in
  back.

Definition front {a} (arg\_0\_\_ : Queue a) :=
  let 'MkQueue front \_ := arg\_0\_\_ in
  front.

(\* Converted value declarations: \*)

Definition push {a} : a \-> Queue a \-> Queue a :=
  fun arg\_0\_\_ arg\_1\_\_ \=>
    match arg\_0\_\_, arg\_1\_\_ with
    | x, MkQueue front back \=> MkQueue front (cons x back)
    end.

Definition pop {a} : Queue a \-> option (a \* Queue a)%type :=
  fun arg\_0\_\_ \=>
    match arg\_0\_\_ with
    | MkQueue (cons y front) back \=> Some (pair y (MkQueue front back))
    | MkQueue nil back \=>
        match GHC.List.reverse back with
        | nil \=> None
        | cons y front \=> Some (pair y (MkQueue front nil))
        end
    end.

Definition empty {a} : Queue a :=
  MkQueue nil nil.

(\* External variables:
 None Some cons list nil op\_zt\_\_ option pair Coq.Lists.List.rev
\*) 

#### Overview

Let’s quickly walk through the code produced by _hs-to-coq_.

Coq files are sequences of _commands_ starting with a keyword and terminated by a dot.

After some minor language settings (`Set`, `Unset`) and some imports (`Require`, which roughly corresponds to `import` in Haskell), we have the translation of our Haskell module `Queue.hs`.

Here is a condensed summary of the main differences:

-   Haskell `data` declarations become Coq `Inductive` declarations;
-   Record field accessors `front` and `back` become plain functions;
-   The `=` separator for definitions in Haskell becomes `:=` in Coq;
-   The `::` separator for type annotations becomes `:`;
-   Pattern-matching: `case ... of ...` becomes `match ... with ... end`, using vertical bars `|` to explicitly separate branches;
-   Anonymous functions `\x -> ...` become `fun x => ...`;
-   The type `[a]` becomes `list a`, with constructors `nil` and `cons`, also denoted by `[]` and `::`;
-   The type `(a, b)` becomes `a * b`, with constructor `pair`;
-   The type `Maybe a` becomes `option a`, with constructors `None` and `Some`.

The replacements for `[]`, `(,)` and `Maybe` are guided by the edits file `hs-to-coq/base/edits` mentioned earlier, to use types from the Coq standard library which use different names.

##### In more detail

A `data` declaration in Haskell becomes an `Inductive` declaration in Coq.

-   The `Queue a` type is annotated with its kind (aka. _sort_) `Type`.
-   The syntax of inductive types in Coq generalizes both the usual syntax of data types and “GADT syntax” in Haskell. The fields can be annotated with their types, and they can also be named, which is useful for defining dependently typed constructors (not the case here).

Inductive Queue a : Type
  := | MkQueue (front : list a) (back : list a) : Queue a.

In Coq, identifiers for functions (which includes data constructors) can have some of their arguments implicit. The following command makes the first argument of `MkQueue` implicit. Note that `MkQueue` actually has three arguments: the type `a`, and the two lists. The type `a` can be inferred from the other two, hence it is desirable to make it implicit.

Arguments MkQueue {\_} \_ \_.

The rest are definitions.

-   Type parameters such as `a` are actually regular parameters to the function.
-   `match` expressions can match on multiple values simultaneously, which makes it convenient to translate multi-clause definitions.
-   Some generated names `arg_0__` and `arg_1__` stand for the arguments of the function just before we pattern-match on them. Each branch reuses the names from the Haskell source.

Definition push {a} : a \-> Queue a \-> Queue a :=
  fun arg\_0\_\_ arg\_1\_\_ \=>
    match arg\_0\_\_, arg\_1\_\_ with
    | x, MkQueue xs ys \=> MkQueue (cons x xs) ys
    end.

### Verification

Correctness is relative to a particular _specification_: the implementation is “correct” when it conforms to the specification. In this section, we will define a specification for the queue, and prove that the queue implements that specification.

For a data structure such as a queue, a common approach is a _model-based specification_, relating the data structure to a simpler _model_, or _reference implementation_. This entails defining a _conformance relation_ between the implementation and the specification, making precise the idea that the implementation has the same behavior as the reference implementation, which is _a priori_ easier to reason about. Finally, _verification_ is the act of proving that the relation holds. Ideally, it should be carried out in a well-defined proof language, such as Coq.

The purpose of using a proof assistant is not merely to prevent mistakes, but also to make the very task of verification more tractable by automating the most bureaucratic reasoning steps.

#### Set up a Coq project

If you’re ready to follow along the proof in your own editor, there are a few nitty-gritty details to take care of.

We need some settings to tell the Coq interactive environment where to find _hs-to-coq_ dependencies.

Create a `_CoqProject` file at the root of the directory, with the following contents:

```
-R hs-to-coq/base ""
-Q hs-to-coq/base-thy Proofs
-Q src-coq Src
-Q theories Proving
```

#### Verification: Specification and Proof

##### Import dependencies

In the first few lines, we load the required dependencies.

-   The `List` module from the standard library contains general facts about lists;
-   The `GHC.List` module from _hs-to-coq_ contains facts about Coq-ified functions from the Haskell standard library (we will only need one fact relating Haskell’s `reverse` to Coq’s `rev` function);
-   The `Queue` module we just translated and compiled.

From Coq Require Import List. From Proofs Require Import GHC.List. From Src Require Import Queue.

The following `Import` command makes some extra _notations_ available. Notations are user-defined syntactic sugar; in particular, infix operators are defined as notations in Coq. The module `ListNotations` contains the square bracket notations for lists: `[]`, `[x]`, `[x;y]`, `[x;y;z]`, etc. Two other notations for lists, `::` and `++`, are already available by default.

##### Define the model

A queue can be modelled as a plain `list` of the values it contains. A list `xs` _models_—is a model for—a queue `q` when `xs` is equal to the concatenation of the front and the reversed back of the queue:

front q ++ rev (back q)   \=   xs

In Coq, the proposition “`xs` models `q`” will be written as `models xs q`, and it is defined in terms of the above equation as follows:

Definition models {a} (xs : list a) (q : Queue a) : Prop :=
  front q ++ rev (back q) = xs.

Having defined the model and the conformance relation `models`, the correctness of the implementation consists of the following three _theorems_, one for each operation `empty`, `push`, `pop`, relating them to a corresponding operation on lists.

Because this model is so simple, we can use standard list operations directly in the specification. For more complex data structures, it is often useful to define the model in its own module.

##### Verify `empty`

###### Specification

The following theorem states that the `empty` queue is modelled by the empty list `[]`.

Theorem models\_empty {a} : models (a := a) \[\] empty.

A theorem has a name, `models_empty`, used to refer to it, to apply it in a later proof for instance. Theorems may have parameters, such as the type parameter `a` in this case. The specification of empty queues is applicable with any type of elements.

To the right of the colon is the statement of the theorem: “the empty list `[]` models the `empty` queue”. The expression `models (a := a)` is how we specialize the `models` relation to the type parameter `a`; it must be explicit here to avoid ambiguity, since it cannot be inferred from merely `[]` or `empty`.

So far, we have only stated the theorem. We must now prove it.

###### Proof

Overview

A proof in Coq starts with the command `Proof.`[1](https://www.cis.upenn.edu/~plclub/blog/2020-10-09-hs-to-coq/#fn1) and ends with `Qed.`. Those commands delimit a sequence of _tactics_, which are instructions to modify the _goal_ until it is _solved_.[2](https://www.cis.upenn.edu/~plclub/blog/2020-10-09-hs-to-coq/#fn2)

The _goal_ is the current state of the proof.

In this section, hover over a tactic (or the command `Proof`) to see the resulting goal. Click on the tactic to pin or unpin its goal; this can be useful to show multiple goals at once and understand how tactics modify them.

The goal consists of two parts separated by a horizontal line: above the line, parameters and assumptions, also called the _context_; below the line, the proposition to prove, also called _goal statement_ or just _goal_.

At the beginning of the proof, right after the `Proof` command, we have the type variable `a : Type` in our context, and the goal is our statement of the theorem `models [] empty`.

In this case, the proof is immediate by unfolding the definition of `models`, and checking that the two sides of the underlying equation reduce to the same term. That is all taken care of automatically by the `reflexivity` tactic.

###### A more detailed proof

A one-line proof is not much material to present for a tutorial. Moreover, it can be useful to take more manual steps to better understand the details of a proof. Below is an alternate proof of the same theorem as above. We must give it another name, because top-level names must be unique in Coq.

We use the tactics `unfold` and `cbn` to simplify the goal step by step, before finishing the proof with `reflexivity`,

If you’ve never seen a Coq proof before, you might remark something odd. One usually thinks of “deduction” as a process going “forward”, from assumptions to conclusions. In Coq, we start with the conclusion as the goal, and we reason “backwards” until the goal becomes trivial. While forward reasoning is also possible in Coq, we prefer backward reasoning because it tends to make proofs much shorter. Indeed, tactics can infer a lot of information from the goal; in contrast, it’s much more difficult to guess what part of the context is relevant to each step of reasoning. Indeed, pen-and-paper proofs tend to repeat relevant facts in order to improve clarity. Here, the state of the proof at every step is automatically maintained by the proof assistant.

Theorem models\_empty\_2 {a} : models (a := a) \[\] empty. Proof.  unfold models.

> ___
> 
> front empty ++ rev (back empty) = \[\]

  cbn \[front empty\].

> ___
> 
> \[\] ++ rev (back empty) = \[\]

  cbn \[app\].  cbn \[back empty\].  cbn \[rev\].  reflexivity. Qed.

Above, the tactic `unfold` replaces occurrences of `models` in the goal with their definition. The tactic `cbn` looks for redexes to simplify; `cbn` can be parameterized by a list of identifiers that it can unfold in square brackets; without that list, `cbn` may unfold any identifier, which would completely simplify the equation in this case: a single `cbn` would be equivalent to the four more fine-grained variants of `cbn [ ... ]` above.

For instance, when we execute `cbn [front empty]`, the `front` function is pattern-matching on its first argument `empty`, which is also known, so `front empty` reduces to the empty list. Then, `app` (the real name of `++`), when its first argument is empty, returns its second argument, so `[] ++ rev (back empty)` reduces to `rev (back empty)`. _etc._

In the end, the goal simplifies to `[] = []`, which is true by reflexivity of equality.

##### Verify `push`

###### Specification

Let `xs` be a list which models a queue `q`. Let `x` be an element. Then after pushing `x` into `q`, the resulting queue `push x q` is modelled by `xs ++ [x]`.

If `xs` models `q`, then `xs ++ [x]` models `push x q`.

Theorem models\_push {a} : forall (x : a) (xs : list a) (q : Queue a),
    models xs q -> models (xs ++ \[x\]) (push x q).

> ___
> 
> forall (x : a) (xs : list a) (q : Queue a),
> models xs q -> models (xs ++ \[x\]) (push x q)

###### Proof

Let’s walk through this proof step by step.

Proof.

> ___
> 
> forall (x : a) (xs : list a) (q : Queue a),
> models xs q -> models (xs ++ \[x\]) (push x q)

  intros x xs q Eq.

> a:Type
> 
> x:a
> 
> xs:list a
> 
> q:Queue a
> 
> Eq:models xs q
> 
> ___
> 
> models (xs ++ \[x\]) (push x q)

  unfold models in \*.

> a:Type
> 
> x:a
> 
> xs:list a
> 
> q:Queue a
> 
> Eq:front q ++ rev (back q) = xs
> 
> ___
> 
> front (push x q) ++ rev (back (push x q)) = xs ++ \[x\]

  destruct q as \[front\_q back\_q\].

> a:Type
> 
> x:a
> 
> xs, front\_q, back\_q:list a
> 
> Eq:front (MkQueue front\_q back\_q) ++
> rev (back (MkQueue front\_q back\_q)) = xs
> 
> ___
> 
> front (push x (MkQueue front\_q back\_q)) ++
> rev (back (push x (MkQueue front\_q back\_q))) =
> xs ++ \[x\]

  cbn in \*.

> a:Type
> 
> x:a
> 
> xs, front\_q, back\_q:list a
> 
> Eq:front\_q ++ rev back\_q = xs
> 
> ___
> 
> front\_q ++ rev back\_q ++ \[x\] = xs ++ \[x\]

  rewrite app\_assoc.

> a:Type
> 
> x:a
> 
> xs, front\_q, back\_q:list a
> 
> Eq:front\_q ++ rev back\_q = xs
> 
> ___
> 
> (front\_q ++ rev back\_q) ++ \[x\] = xs ++ \[x\]

  rewrite Eq.

> a:Type
> 
> x:a
> 
> xs, front\_q, back\_q:list a
> 
> Eq:front\_q ++ rev back\_q = xs
> 
> ___
> 
> xs ++ \[x\] = xs ++ \[x\]

  reflexivity. Qed.

1\. `intros x xs q Eq`

The goal starts with a universal quantifier `forall`. The `intros` tactic moves those parameters, as well as assumptions, from the goal to the context.

-   Before:
    
    ```
    ==============================================
    forall (x : a) (xs : list a) (q : Queue a),
      models xs q -> models (xs ++ [x]) (push x q)
    ```
    
-   After `intros x xs q Eq`:
    
    ```
    x : a
    xs : list a
    q : Queue a
    Eq : models xs q
    =============================
    models (xs ++ [x]) (push x q)
    ```
    

2\. `unfold models in *`

Replace `models` with its definition. The `in *` part of this tactic indicates that the replacement is to take place both above and below the line. In particular, we want to simplify the hypothesis `Eq`. Without `in *`, `unfold models` only unfolds in the goal below the line.

(\* Current goal (below the line) \*)
front (push x q) ++ rev (back (push x q)) \= xs ++ \[x\]

3\. `destruct q as [front_q back_q]`

The function `push` pattern-matches on `q`. We also know that `q`, which has type `Queue a`, must consist of a constructor `MkQueue` applied to two lists. Making that structure explicit allows us to reduce the expression `push x q`. We thus replace `q` with the application of `MkQueue` to two lists named `front_q` and `back_q`.

(\* Current goal \*)
front (push x (MkQueue front\_q back\_q)) ++ rev (back (push x (MkQueue front\_q back\_q))) \= xs ++ \[x\]

4\. `cbn in *`

Now that the second argument of `push` is a constructor, we can simplify that expression, which is what the tactic `cbn` does. The `cbn in *` variant of that tactic also simplifies the hypothesis `Eq` in the context.

(\* Current goal \*)
front\_q ++ (rev back\_q ++ \[x\]) \= xs ++ \[x\]

5\. `rewrite app_assoc`

List concatenation `++` is associative. That is the theorem named `app_assoc`.

app\_assoc : forall xs ys zs, xs ++ (ys ++ zs) \= (xs ++ ys) ++ zs

The `rewrite` tactic uses equality theorems and assumptions to rewrite the goal accordingly.

(\* Current goal \*)
(front\_q ++ rev back\_q) ++ \[x\] \= xs ++ \[x\]

6\. `rewrite Eq`

We have an assumption `(front_q ++ rev back_q) = xs`, named `Eq`. We `rewrite` with it.

(\* Current goal \*)
xs ++ \[x\] \= xs ++ \[x\]

7\. `reflexivity`

Both sides are identical. The goal is solved by reflexivity.

That concludes the proof of `models_push`. `Qed.`

##### Verify `pop`

The specification and proof for `pop` is trickier because its result is an optional value, so we must account for the possibility of it being `None`. The following theorem says that if `q` is modelled by a nonempty list `y :: xs`, then `pop q` must succeed with `Some` result. The first component of the result is `y`. The second component is a queue `q'`, which must be modelled by the rest of the list `xs`. The list `q'` is quantified existentially: it is an output determined by the implementation. In contrast, universally quantified parameters (`a`, `xs`, `y`, `q`) are those chosen by clients of the implementation.

If `q` models `y :: xs`, then there exists a `q'` such that `pop q = Some (y, q')` and `xs` models `q'`.

Theorem models\_pop {a} : forall (xs : list a) (y : a) (q : Queue a),
    models (y :: xs) q ->
    exists q',
      pop q = Some (y, q') /\\
      models xs q'.

> ___
> 
> forall (xs : list a) (y : a) (q : Queue a),
> models (y :: xs) q ->
> exists q' : Queue a,
>   pop q = Some (y, q') /\\ models xs q'

We will not cover the proof in detail. You can try to step trough the proof to see how the goal evolves and guess what the new tactics do. Let us simply mention that the main novelty in this proof, compared to the previous two, is case analysis (`destruct front_q`), used to reason separately about the cases where the front of the queue is empty or not.

Proof.

> ___
> 
> forall (xs : list a) (y : a) (q : Queue a),
> models (y :: xs) q ->
> exists q' : Queue a,
>   pop q = Some (y, q') /\\ models xs q'

  intros xs y q Eq.

> a:Type
> 
> xs:list a
> 
> y:a
> 
> q:Queue a
> 
> Eq:models (y :: xs) q
> 
> ___
> 
> exists q' : Queue a,
>   pop q = Some (y, q') /\\ models xs q'

  unfold models in \*.

> a:Type
> 
> xs:list a
> 
> y:a
> 
> q:Queue a
> 
> Eq:front q ++ rev (back q) = y :: xs
> 
> ___
> 
> exists q' : Queue a,
>   pop q = Some (y, q') /\\
>   front q' ++ rev (back q') = xs

  destruct q as \[front\_q back\_q\].

> a:Type
> 
> xs:list a
> 
> y:a
> 
> front\_q, back\_q:list a
> 
> Eq:front (MkQueue front\_q back\_q) ++
> rev (back (MkQueue front\_q back\_q)) = 
> y :: xs
> 
> ___
> 
> exists q' : Queue a,
>   pop (MkQueue front\_q back\_q) = Some (y, q') /\\
>   front q' ++ rev (back q') = xs

  cbn in \*.

> a:Type
> 
> xs:list a
> 
> y:a
> 
> front\_q, back\_q:list a
> 
> Eq:front\_q ++ rev back\_q = y :: xs
> 
> ___
> 
> exists q' : Queue a,
>   match front\_q with
>   | \[\] =>
>       match GHC.List.reverse back\_q with
>       | \[\] => None
>       | y0 :: front => Some (y0, MkQueue front \[\])
>       end
>   | y0 :: front => Some (y0, MkQueue front back\_q)
>   end = Some (y, q') /\\ 
>   front q' ++ rev (back q') = xs

  destruct front\_q as \[ | x front\_q' \].

> a:Type
> 
> xs:list a
> 
> y:a
> 
> back\_q:list a
> 
> Eq:\[\] ++ rev back\_q = y :: xs
> 
> ___
> 
> exists q' : Queue a,
>   match GHC.List.reverse back\_q with
>   | \[\] => None
>   | y0 :: front => Some (y0, MkQueue front \[\])
>   end = Some (y, q') /\\ 
>   front q' ++ rev (back q') = xs

  \-

> a:Type
> 
> xs:list a
> 
> y:a
> 
> back\_q:list a
> 
> Eq:\[\] ++ rev back\_q = y :: xs
> 
> ___
> 
> exists q' : Queue a,
>   match GHC.List.reverse back\_q with
>   | \[\] => None
>   | y0 :: front => Some (y0, MkQueue front \[\])
>   end = Some (y, q') /\\ 
>   front q' ++ rev (back q') = xs

 (\* front is empty \*) cbn in Eq.

> a:Type
> 
> xs:list a
> 
> y:a
> 
> back\_q:list a
> 
> Eq:rev back\_q = y :: xs
> 
> ___
> 
> exists q' : Queue a,
>   match GHC.List.reverse back\_q with
>   | \[\] => None
>   | y0 :: front => Some (y0, MkQueue front \[\])
>   end = Some (y, q') /\\ 
>   front q' ++ rev (back q') = xs

  rewrite GHC.List.hs\_coq\_reverse.

> a:Type
> 
> xs:list a
> 
> y:a
> 
> back\_q:list a
> 
> Eq:rev back\_q = y :: xs
> 
> ___
> 
> exists q' : Queue a,
>   match rev back\_q with
>   | \[\] => None
>   | y0 :: front => Some (y0, MkQueue front \[\])
>   end = Some (y, q') /\\ 
>   front q' ++ rev (back q') = xs

  rewrite Eq.

> a:Type
> 
> xs:list a
> 
> y:a
> 
> back\_q:list a
> 
> Eq:rev back\_q = y :: xs
> 
> ___
> 
> exists q' : Queue a,
>   Some (y, MkQueue xs \[\]) = Some (y, q') /\\
>   front q' ++ rev (back q') = xs

  exists (MkQueue xs \[\]).

> a:Type
> 
> xs:list a
> 
> y:a
> 
> back\_q:list a
> 
> Eq:rev back\_q = y :: xs
> 
> ___
> 
> Some (y, MkQueue xs \[\]) = Some (y, MkQueue xs \[\]) /\\
> front (MkQueue xs \[\]) ++ rev (back (MkQueue xs \[\])) =
> xs

  split.

> a:Type
> 
> xs:list a
> 
> y:a
> 
> back\_q:list a
> 
> Eq:rev back\_q = y :: xs
> 
> ___
> 
> Some (y, MkQueue xs \[\]) = Some (y, MkQueue xs \[\])

  +

> a:Type
> 
> xs:list a
> 
> y:a
> 
> back\_q:list a
> 
> Eq:rev back\_q = y :: xs
> 
> ___
> 
> Some (y, MkQueue xs \[\]) = Some (y, MkQueue xs \[\]) reflexivity.  +

> a:Type
> 
> xs:list a
> 
> y:a
> 
> back\_q:list a
> 
> Eq:rev back\_q = y :: xs
> 
> ___
> 
> front (MkQueue xs \[\]) ++ rev (back (MkQueue xs \[\])) =
> xs cbn.

> a:Type
> 
> xs:list a
> 
> y:a
> 
> back\_q:list a
> 
> Eq:rev back\_q = y :: xs
> 
> ___
> 
> xs ++ \[\] = xs

  rewrite app\_nil\_r.

> a:Type
> 
> xs:list a
> 
> y:a
> 
> back\_q:list a
> 
> Eq:rev back\_q = y :: xs
> 
> ___
> 
> xs = xs

  reflexivity.  \-

> a:Type
> 
> xs:list a
> 
> y, x:a
> 
> front\_q', back\_q:list a
> 
> Eq:(x :: front\_q') ++ rev back\_q = y :: xs
> 
> ___
> 
> exists q' : Queue a,
>   Some (x, MkQueue front\_q' back\_q) = Some (y, q') /\\
>   front q' ++ rev (back q') = xs

 (\* front is not empty \*) cbn in Eq.

> a:Type
> 
> xs:list a
> 
> y, x:a
> 
> front\_q', back\_q:list a
> 
> Eq:x :: front\_q' ++ rev back\_q = y :: xs
> 
> ___
> 
> exists q' : Queue a,
>   Some (x, MkQueue front\_q' back\_q) = Some (y, q') /\\
>   front q' ++ rev (back q') = xs

  exists (MkQueue front\_q' back\_q).

> a:Type
> 
> xs:list a
> 
> y, x:a
> 
> front\_q', back\_q:list a
> 
> Eq:x :: front\_q' ++ rev back\_q = y :: xs
> 
> ___
> 
> Some (x, MkQueue front\_q' back\_q) =
> Some (y, MkQueue front\_q' back\_q) /\\
> front (MkQueue front\_q' back\_q) ++
> rev (back (MkQueue front\_q' back\_q)) = xs

  injection Eq.

> a:Type
> 
> xs:list a
> 
> y, x:a
> 
> front\_q', back\_q:list a
> 
> Eq:x :: front\_q' ++ rev back\_q = y :: xs
> 
> ___
> 
> front\_q' ++ rev back\_q = xs ->
> x = y ->
> Some (x, MkQueue front\_q' back\_q) =
> Some (y, MkQueue front\_q' back\_q) /\\
> front (MkQueue front\_q' back\_q) ++
> rev (back (MkQueue front\_q' back\_q)) = xs

  intros Eq' Ey.

> a:Type
> 
> xs:list a
> 
> y, x:a
> 
> front\_q', back\_q:list a
> 
> Eq:x :: front\_q' ++ rev back\_q = y :: xs
> 
> Eq':front\_q' ++ rev back\_q = xs
> 
> Ey:x = y
> 
> ___
> 
> Some (x, MkQueue front\_q' back\_q) =
> Some (y, MkQueue front\_q' back\_q) /\\
> front (MkQueue front\_q' back\_q) ++
> rev (back (MkQueue front\_q' back\_q)) = xs

  split.

> a:Type
> 
> xs:list a
> 
> y, x:a
> 
> front\_q', back\_q:list a
> 
> Eq:x :: front\_q' ++ rev back\_q = y :: xs
> 
> Eq':front\_q' ++ rev back\_q = xs
> 
> Ey:x = y
> 
> ___
> 
> Some (x, MkQueue front\_q' back\_q) =
> Some (y, MkQueue front\_q' back\_q)

  +

> a:Type
> 
> xs:list a
> 
> y, x:a
> 
> front\_q', back\_q:list a
> 
> Eq:x :: front\_q' ++ rev back\_q = y :: xs
> 
> Eq':front\_q' ++ rev back\_q = xs
> 
> Ey:x = y
> 
> ___
> 
> Some (x, MkQueue front\_q' back\_q) =
> Some (y, MkQueue front\_q' back\_q) rewrite Ey.

> a:Type
> 
> xs:list a
> 
> y, x:a
> 
> front\_q', back\_q:list a
> 
> Eq:x :: front\_q' ++ rev back\_q = y :: xs
> 
> Eq':front\_q' ++ rev back\_q = xs
> 
> Ey:x = y
> 
> ___
> 
> Some (y, MkQueue front\_q' back\_q) =
> Some (y, MkQueue front\_q' back\_q) reflexivity.  +

> a:Type
> 
> xs:list a
> 
> y, x:a
> 
> front\_q', back\_q:list a
> 
> Eq:x :: front\_q' ++ rev back\_q = y :: xs
> 
> Eq':front\_q' ++ rev back\_q = xs
> 
> Ey:x = y
> 
> ___
> 
> front (MkQueue front\_q' back\_q) ++
> rev (back (MkQueue front\_q' back\_q)) = xs cbn.

> a:Type
> 
> xs:list a
> 
> y, x:a
> 
> front\_q', back\_q:list a
> 
> Eq:x :: front\_q' ++ rev back\_q = y :: xs
> 
> Eq':front\_q' ++ rev back\_q = xs
> 
> Ey:x = y
> 
> ___
> 
> front\_q' ++ rev back\_q = xs apply Eq'. Qed.

The theorem `models_pop` only applies to non-empty queues. Prove a theorem about the behavior of `pop` on the empty queue.

#### Final check

Having completed the proofs, they can be checked non-interactively using the Coq compiler `coqc`. This also compiles an object file `QueueSpec.vo` which makes the definitions and theorems available to build new modules on top of `QueueSpec`.

```
# Compile QueueSpec.v
coqc -Q hs-to-coq/base-thy Proofs -Q src-coq Src -Q theories Proving -R hs-to-coq/base '' QueueSpec.v
```

### Discussion

What good is it to formally verify a library?

The popular marketing slogan for formal verification is that “it guarantees the absence of bugs”. What we actually proved is that the implementation conforms to a model, a specification. We can thus say that this proof guarantees that the implementation is at least as good as the specification. But how good is the specification? This is a hard question at the heart of the area of formal methods.

The obvious way to evaluate the specification is to read it, and convince oneself that, yes, that’s what a queue should do. For that reason, it is desirable for the specification to be significantly simpler than the implementation.

The specification can also be seen as an abstraction of the implementation, describing the features that are relevant to users of the implementation, while hiding other internal _implementation details_. A _rich_ specification is one that allows meaningful properties to be proved about programs using a conforming implementation.

At the very least, we should be able to use the specification to prove simple properties of toy programs, as a form of _unit testing_ of the specification.

For instance, the following unit test is a proof that the list `[1; 2; 3]` models the queue obtained by pushing `1`, `2`, `3` in this order. Note that, although there are simpler proofs, this particular proof relies entirely on the theorems from the specification: the same proof would still work even if we completely changed the implementation and the definition of `models`.

(\* Note: "Example" is a synonym of "Theorem". \*) Example example1 : models \[1; 2; 3\] (push 3 (push 2 (push 1 empty))).

> ___
> 
> models \[1; 2; 3\] (push 3 (push 2 (push 1 empty))) Proof.

> ___
> 
> models \[1; 2; 3\] (push 3 (push 2 (push 1 empty)))

  pose proof (models\_empty (a := nat)) as H.

> ___
> 
> models \[1; 2; 3\] (push 3 (push 2 (push 1 empty)))

  apply (models\_push 1) in H.

> H:models (\[\] ++ \[1\]) (push 1 empty)
> 
> ___
> 
> models \[1; 2; 3\] (push 3 (push 2 (push 1 empty))) cbn \[ app \] in H.

> H:models \[1\] (push 1 empty)
> 
> ___
> 
> models \[1; 2; 3\] (push 3 (push 2 (push 1 empty)))

  apply (models\_push 2) in H.

> H:models (\[1\] ++ \[2\]) (push 2 (push 1 empty))
> 
> ___
> 
> models \[1; 2; 3\] (push 3 (push 2 (push 1 empty))) cbn \[ app \] in H.

> H:models \[1; 2\] (push 2 (push 1 empty))
> 
> ___
> 
> models \[1; 2; 3\] (push 3 (push 2 (push 1 empty)))

  apply (models\_push 3) in H.

> H:models (\[1; 2\] ++ \[3\])
>   (push 3 (push 2 (push 1 empty)))
> 
> ___
> 
> models \[1; 2; 3\] (push 3 (push 2 (push 1 empty))) cbn \[ app \] in H.

> H:models \[1; 2; 3\] (push 3 (push 2 (push 1 empty)))
> 
> ___
> 
> models \[1; 2; 3\] (push 3 (push 2 (push 1 empty)))

  apply H. Qed.

The above proof only uses `push` and `empty`. Here is another example using `pop`, proving that `pop` on the above queue produces the first element `1`.

Infix "<$>" := option\_map (at level 40). (\* Infix notation for option\_map : (a -> b) -> option a -> option b \*) Example example2 :
  fst <$> pop (push 3 (push 2 (push 1 empty))) = Some 1.

> ___
> 
> fst <$> pop (push 3 (push 2 (push 1 empty))) = Some 1 Proof.

> ___
> 
> fst <$> pop (push 3 (push 2 (push 1 empty))) = Some 1

  pose proof example1 as H.

> H:models \[1; 2; 3\] (push 3 (push 2 (push 1 empty)))
> 
> ___
> 
> fst <$> pop (push 3 (push 2 (push 1 empty))) = Some 1

  apply models\_pop in H.

> H:exists q' : Queue nat,
>   pop (push 3 (push 2 (push 1 empty))) =
>   Some (1, q') /\\ models \[2; 3\] q'
> 
> ___
> 
> fst <$> pop (push 3 (push 2 (push 1 empty))) = Some 1

  destruct H as \[q' \[Hpop Hq'\]\].

> q':Queue nat
> 
> Hpop:pop (push 3 (push 2 (push 1 empty))) =
> Some (1, q')
> 
> Hq':models \[2; 3\] q'
> 
> ___
> 
> fst <$> pop (push 3 (push 2 (push 1 empty))) = Some 1

  rewrite Hpop.

> q':Queue nat
> 
> Hpop:pop (push 3 (push 2 (push 1 empty))) =
> Some (1, q')
> 
> Hq':models \[2; 3\] q'
> 
> ___
> 
> fst <$> Some (1, q') = Some 1

  reflexivity. Qed.

That last example can be generalized a little: once we have a non-empty queue (`push 1 empty`), pushing new values will not change the value produced by the next `pop`.

Example example3 {a} : forall (q : Queue a) (x y : a) (xs : list a),
  models (x :: xs) q ->
  fst <$> pop (push y q) = fst <$> pop q.

> ___
> 
> forall (q : Queue a) (x y : a) (xs : list a),
> models (x :: xs) q ->
> fst <$> pop (push y q) = fst <$> pop q Proof.

> ___
> 
> forall (q : Queue a) (x y : a) (xs : list a),
> models (x :: xs) q ->
> fst <$> pop (push y q) = fst <$> pop q

  intros q x y xs Hq.

> a:Type
> 
> q:Queue a
> 
> x, y:a
> 
> xs:list a
> 
> Hq:models (x :: xs) q
> 
> ___
> 
> fst <$> pop (push y q) = fst <$> pop q

  pose proof (models\_push y (x :: xs) q Hq) as Hqy.

> a:Type
> 
> q:Queue a
> 
> x, y:a
> 
> xs:list a
> 
> Hq:models (x :: xs) q
> 
> Hqy:models ((x :: xs) ++ \[y\]) (push y q)
> 
> ___
> 
> fst <$> pop (push y q) = fst <$> pop q

  cbn in Hqy.

> a:Type
> 
> q:Queue a
> 
> x, y:a
> 
> xs:list a
> 
> Hq:models (x :: xs) q
> 
> Hqy:models (x :: xs ++ \[y\]) (push y q)
> 
> ___
> 
> fst <$> pop (push y q) = fst <$> pop q

  apply models\_pop in Hq.

> a:Type
> 
> q:Queue a
> 
> x, y:a
> 
> xs:list a
> 
> Hq:exists q' : Queue a,
>   pop q = Some (x, q') /\\ models xs q'
> 
> Hqy:models (x :: xs ++ \[y\]) (push y q)
> 
> ___
> 
> fst <$> pop (push y q) = fst <$> pop q

  apply models\_pop in Hqy.

> a:Type
> 
> q:Queue a
> 
> x, y:a
> 
> xs:list a
> 
> Hq:exists q' : Queue a,
>   pop q = Some (x, q') /\\ models xs q'
> 
> Hqy:exists q' : Queue a, pop (push y q) = Some (x, q') /\\ models (xs ++ \[y\]) q'
> 
> ___
> 
> fst <$> pop (push y q) = fst <$> pop q

  destruct Hq as \[q' \[Eq' Hq'\]\].

> a:Type
> 
> q:Queue a
> 
> x, y:a
> 
> xs:list a
> 
> q':Queue a
> 
> Eq':pop q = Some (x, q')
> 
> Hq':models xs q'
> 
> Hqy:exists q'0 : Queue a,
> pop (push y q) = Some (x, q'0) /\\ models (xs ++ \[y\]) q'0
> 
> ___
> 
> fst <$> pop (push y q) = fst <$> pop q

  destruct Hqy as \[qy' \[Eqy' Hqy'\]\].

> a:Type
> 
> q:Queue a
> 
> x, y:a
> 
> xs:list a
> 
> q':Queue a
> 
> Eq':pop q = Some (x, q')
> 
> Hq':models xs q'
> 
> qy':Queue a
> 
> Eqy':pop (push y q) = Some (x, qy')
> 
> Hqy':models (xs ++ \[y\]) qy'
> 
> ___
> 
> fst <$> pop (push y q) = fst <$> pop q

  rewrite Eq', Eqy'.

> a:Type
> 
> q:Queue a
> 
> x, y:a
> 
> xs:list a
> 
> q':Queue a
> 
> Eq':pop q = Some (x, q')
> 
> Hq':models xs q'
> 
> qy':Queue a
> 
> Eqy':pop (push y q) = Some (x, qy')
> 
> Hqy':models (xs ++ \[y\]) qy'
> 
> ___
> 
> fst <$> Some (x, qy') = fst <$> Some (x, q')

  cbn.

> a:Type
> 
> q:Queue a
> 
> x, y:a
> 
> xs:list a
> 
> q':Queue a
> 
> Eq':pop q = Some (x, q')
> 
> Hq':models xs q'
> 
> qy':Queue a
> 
> Eqy':pop (push y q) = Some (x, qy')
> 
> Hqy':models (xs ++ \[y\]) qy'
> 
> ___
> 
> Some x = Some x

  reflexivity. Qed.

As another example, it may also be useful to require and prove that the conformance relation (`models`) is “injective”: every queue has at most one model.

Theorem injective\_models {a} : forall (q : Queue a) (xs ys : list a),
  models xs q ->
  models ys q ->
  xs = ys.

> ___
> 
> forall (q : Queue a) (xs ys : list a),
> models xs q -> models ys q -> xs = ys Proof.

> ___
> 
> forall (q : Queue a) (xs ys : list a),
> models xs q -> models ys q -> xs = ys

  (\* Exercise for the reader (easy) \*) Abort.

Both the implementation and the specification may contain mistakes. Formal verification, by connecting the two, weeds out “mismatches” between them. Of course, matching mistakes might happen on both sides in ways that may still allow the proofs to go through. This motivates various forms of _metaspecification_: approaches to evaluate the quality of a specification. Small test cases such as the examples above serve as simple sanity checks. Other approaches include using conventional abstractions (if you have a monoid, you’d better prove the monoid laws), and looking for _completeness theorems_ describing interesting classes of properties that a sufficiently expressive specification can prove.

Moreover, by formalizing the specification and proving the conformance of the implementation, we are often forced to understand both the specification and the implementation to a high level of rigor. By that process, formal verification inherently reduces the likelihood of errors.

For instance, one possibly non-obvious aspect of the above specification is to remember the order of elements in the list modelling the queue: elements are pushed to the end of the list, and popped from the head. The very minor reason to prefer this over the other direction is that the constructor `::` in the theorem about `pop` makes it easier to use in certain proofs, such as in `example2`. Proving the specification correct reduces the potential for confusion about which end of the model elements are inserted.

The `example3` above may suggest a further generalization: “`push` commutes with `pop`”. However, a naive formalization of that property is falsifiable. Why can the following theorem not be proved? Is there a reformulation that you can prove?

(\* Apply a function on the second component of a pair. \*) Definition map\_snd {a b b'} (f : b -> b') (xy : a \* b) : a \* b' :=
  (fst xy, f (snd xy)).  (\* False! \*) Theorem invalid\_push\_pop\_comm {a} : forall (q : Queue a) (x y : a) (xs : list a),
  models (x :: xs) q ->
  pop (push y q) = map\_snd (push y) <$> pop q.

> ___
> 
> forall (q : Queue a) (x y : a) (xs : list a),
> models (x :: xs) q ->
> pop (push y q) = map\_snd (push y) <$> pop q Abort.

## Conclusion

As we can see from these examples, explaining those proofs in English quickly becomes a tedious task: proofs take a lot of work to write; proofs take a lot of work to read, and to check for mistakes; many people are even uncomfortable with the very idea of a proof, requiring years of training to gain confidence that your reasoning is correct. So people currently don’t write proofs.

Proof assistants change that whole game. There are now explicit rules about what a “proof” is: a well-defined language of specifications and proofs.

It’s not all rainbows and roses, of course, but some common criticisms, even by practitioners in the field, can be mitigated, especially when the target audience is people who like programming.

Tactics, or really all proof languages, are well-known to be unreadable. That is mainly because they are extremely context-dependent, and it is not inherently a drawback. Whereas pen-and-paper proofs tend to repeat relevant parts of the context to help keep track of it mentally, proof assistants (and compilers) maintain the context automatically at all points of the proof, and they can display it on a screen, like this post does thanks to the Coq documentation tool [Alectryon](https://github.com/cpitclaudel/alectryon).

Proving is programming, in more ways than one. The language may be unfamiliar; that’s perhaps the least of an issue for programmers. Proofs are programmable: new tactics can be defined and reused in different proofs, proof automation enables reasoning at higher levels of abstraction. Proof management presents new challenges, but practices for managing programs should apply similarly to proofs: documentation, refactoring, using libraries, improving the language.

For those reasons, I have hope that interactive theorem proving will become regular practice in the future of programming.

___

-   [_Software Foundations_](https://softwarefoundations.cis.upenn.edu/), [_Programming Language Foundations in Agda_](https://plfa.github.io/), courses on programming language theory in Coq and Agda.
-   [_The Natural Number Game_](http://wwwf.imperial.ac.uk/~buzzard/xena/natural_number_game/), an introduction to interactive theorem proving in Lean.
-   [_Finding bugs in Haskell by proving it_](https://www.joachim-breitner.de/blog/734-Finding_bugs_in_Haskell_code_by_proving_it), another blog post showing an application of _hs-to-coq_.

___

1.  `Proof.` is technically optional, but it’s good style to have it. It might even become mandatory in the future (see for example [this discussion](https://github.com/coq/ceps/pull/42#issuecomment-638962799)).[↩︎](https://www.cis.upenn.edu/~plclub/blog/2020-10-09-hs-to-coq/#fnref1)
    
2.  To be accurate, sequences of tactics are really called _proof scripts_. If you look at the details, _proof_ is a complicated concept to define. But for all intents and purposes, most Coq users only need to know that “if it compiles, it’s correct”; from that point of view, it’s fine to call a proof script a “proof”.[↩︎](https://www.cis.upenn.edu/~plclub/blog/2020-10-09-hs-to-coq/#fnref2)
