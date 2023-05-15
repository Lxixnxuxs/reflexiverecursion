---
author: Linus Meierhöfer
title: Haskell, A categorical analysis
date: 2023-03-21
description: A description
math: true
---

# 


# What is a category?
Let us first tackle the question what a category is and why its study even can be useful? In general, category theory offers an alternative view on the foundation of mathematics, i.e. all the different mathematical theories can be modeled using categorical terms. While set theory, the most widely accepted and used foundational theory, relies on the concept of membership and (at least in some sense) construction, category theory tries to solely focus on structure by just looking at the relationship between abstractly defined objects. Category Theory is all und just about relationship! 
When using a computer-science analogy one might think of set-theory as the "imperative" view on maths arguing over actual constructions (not really but in most parts) and elements whereas category theory is equivalent to a "functional" view reasoning solely over the relationship of objects. Of course this analogy goes both ways as such it is no surprise that categorical analysis is in the heart of functional programming. Every viewpoint has its advantages and drawbacks, it just comes down on what is more handy for you to work with given a specific question. 
When reasoning about the relationships of a theory in categories we first have to introduce some base case concept in which we can work. This is done by defining a category $C$ with a defined set of properties:
- First, in order to speak about relationships, we need something as a ground set to argue over. This is formalized by $C$ having a collection $C_0$ of objects. These are in most parts purely abstract and may not need to have any other property than solely to exist
- Secondly to introduce relations we give each pair of objects $x, y \in C_0$ a set of morphisms $C_1(x, y)$ (we won't call them functions to make a clear distinction to set theory)
- In order to get some interesting non-trivial set of tools, we also enforce that for objects $x, y, z \in C_0$ and $f \in C_1(x, y)$ and $g \in C_1(y, z)$ there also exists a morphism $g \circ f \in C_1(x, z)$. This captures the idea of function composition as an universal phenonenon in mathematics.
- For the same reasons we require an identity morphism for each object, i.e. for $x \in C_0$ there exists $id_x \in C_1(x, x)$ with the property that for any $y \in C_0$ we have if $f \in C_1(x, y)$ then $1_y \circ f = f = f \circ 1_x$
- Also we want the function composition to fulfill the fudamental property of associativity, so for $w, x, y, z \in C_0$ and $f\in C_1(w, x), g \in C_1(x, y), h \in C_1(y, z)$ we have that $h \circ (g \circ f) = (h \circ g) \circ f$

Since we are only interested in structure by relationships, most categorical laws can naturally be expressed in diagrams.  

## Haskell as a Category
Having now taken a first glance at the formalism we proceed to transform Haskell into this and explain more advanced concepts on the way. When reasoning about how to formalize Haskell into an category one might fastly develop the idea to not look at actual Haskell values but Haskell types. Actually the notion of a Haskell category has already been given great attention such that it also received a name: Hask (The category of Haskell types) from now on abbreviated by $\mathcal{H}$. So remember: We only talk about types here.
We now can use the formalism from the previous paragraph and build a type category by ourselfs:
- The objects in $\mathcal{H}_0$ are all Haskell types
- The morphisms $\mathcal{H}_1(A, B)$ between two types $A, B \in \mathcal{H}_0$ are the functions from type $A$ to type $B$
- As function composition in Haskell is defined by the `.` operator $\mathcal{H}$ also fulfills the third requirement
- Similarly this applies for the `id` function defined for every type for the fourth requirement
- As we talk about well-defined functions (without side-effects) here and since function composition is associative, we also fulfill the last constraint

## Datatypes from the Categorial Viewpoint
Given this definition it is hardly surprising that all Haskell datatypes can be explained in categorial terms. But there is a subtle problem here. Obviously some types in Haskell have more structure than others and many types rely on other types per definition. Take the tuple type $(A, B)$ with a type $A$ in the first position and a type $B$ in the second position. 

### The best way of choosing a structure


### Product types


### Coproduct types

### Not the whole story 


## Adding more structure
After we now have understod the basic categorial formalism of Haskells type theory we will proceed to explore the concept of Functors and Monads. I first try to give some intuition why these concepts are neccessary. We do this by observing a general phenomena in Haskells type system: Generalizing the concepts of the last chapter we observe many datastructures that are solely dependent on other types: Some examples include `List`, `Maybe`, `Tree`, etc. We could summarize them as Container types, wrapping other types by adding some additional structure. So whats so special about these constructs?
Observe that all of the mentioned container types preserve the functions between their base types, they rely on.

Having now layed out the basic catrgorial formalism to analyze Haskell, we will proceed to describe Functors, Applicatives and Monads. Before we do that, we try to give some intution why these extremly rich concept are actually neccessary here. As we described, until yet the objects in $\mathcal{H}$ were not really relevant and our category is somewhat unstructured. But in Haskell (and any other category), there is definitly some structure which should be explainable by our analyzing theory. Take lists as an easy example: Every list has a type and by use of the `map` function can be transformed into a list of another type without changing the overlaying structure (as it will have the same number of elements). Moreover, lists can be flattened and there are also lists of list, etc. Similairly for the `Maybe` type. We can transform a `Maybe a` to a `Maybe b` by a custom function without changing the structure of the `Maybe` itself and most importantly we can "flatten" two nested `Maybe`s into a new one. This should give rise to a generalization of these concepts in our theory.

### Haskell Functors
After having defined what a category in the previous chapters, when thinking like a mathematician, we would soon stumble about the question on how different categories relate to each other. So we need a notion of mapping a cateogry $C$ into another category $D$ which is in some sense interesingly enough to reason about. For this we require this mapping to preserve the categorical structure. To fomalize this we introduce the concept of a Functor $F: C\rightarrow D$ (note that we are working with categories now) such that:
- F maps any object $C_0$ into $D_0$
- F maps morphisms from $C_1$ into $D_1$
- F preserves identities $F_(id_A) = id_{F(A)}$
- F preserves morphism composition $F(f \circ g) = F(f) \circ F(g)$

Functors play an incredibly important role in Haskell. Therefore it has its own formalization: The `Functor` typeclass with the following signature
``class Functor (f :: * -> *) where
  fmap :: (A -> B) -> f A -> f B``

Lets look at this by an exmaple: We want to define a Functor from the Category $\mathcal{H}$ into a subcategory of $\mathcal{H}$ which we will define. For this we define a custom list structure: 
`data L a = Empty | Cons a (L a) deriving (Show, Read, Eq, Ord)`
together with the type instantiation
`instance Functor L where
  fmap f (Cons x l) = Cons (f x) (fmap f l)
  fmap _ Empty = Empty`
This definition actually does two things. First it implicitly defines the subcategory $\mathcal{G} \subset \mathcal{H}$ of all $L a$ types. Secondly, by providing a data constructor `Cons` we define a Functor mapping for $\mathcal{H}_0$ into $\mathcal{G}_0$, i.e. we can map arbitrary Haskell types into their corresponding $L$ types as required by the first constraint of Functors. For this mapping to be a proper functor, by property (2) we also need a mapping from morphisms to morphisms. This is done by adding `L` to the `Functor` typeclass and specifying the corresponding mapping. I.e. for a `L` of type `A` and a function from `A` to `B` the function `fmap` returns an `L` of type `B` without altering the structure of `L`. We could easily check the last two properties of Functors to be true in this case as applying the identity function to `fmap` will result in the same list and by definition of 'fmap' we do not make assumption that change the behaviour of function composition.

The key take away one should remember is that by defining a `data`-type in the `Functor` typeclass one implicitly defines its subcategory and also the corresponding morphism. One could also restrict the domain-category accordingly by introducing type-constraints in the `data`-definition.

The above explanantion implies that all Haskell containers can actually be seen as subcategories of $\mathcal{H}$ with corresponding Functors and that their respective `map` functions are actually the morphism-part of this Functor. But this implication is only one-sided, so not every Functor-type must represent a container-like object since its a much broader concept as we have seen.

Similarly typeclasses are a neat way to express subcategories of $\mathcal{H}$ as make restrictions to certain specified subsets of $\mathcal{H}_0$.

### Monad

### Monoid in the category of endofunctors