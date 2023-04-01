---
author: Linus Meierhöfer
title: Forcing measurability
date: 2023-03-21
description: A description
math: true
---

When first learning about the measure-theoretic foundations of probability theory, one of the first questions coming to my mind was why we actually need sigma algebras in the formalism instead of just using always the entire powerset. The standard answer to this is that for large enough ground sets, there arise non-measurable sets which would produce conflicts. A simple example of this is the Vitali Set over the real line constructed by the axiom of choice. However, diving deeper into this question there is a forcing notion able to express a model of $ZF$ in which every subset of the reals is measurable. This model is called Solovay model, named after the US mathematician Robert M. Solovay.

## Map
Generally speaking, the Solovay Model contains many non-trivial properties. Some of them are 
Therefore to construct this model, we
- Start with a model $M$ of $ZF+I$
- Construct a model $M[G]$ from $M$ in which every set of real numbers that are in some sense "easy to describe" are Lesbegue measurable
- Construct a submodel $N$ of $M[G]$ which only contains set that are "easy to describe" in $M$ but still fulfills axioms of set theory

## Step 1
Before starting the forcing, we first have to introduce some concepts which are fundamental to the working of the outlined proof.

### Inaccessible cardinals
An inaccesible cardinal $\kappa$ is, as the name suggests, a cardinal that cannot be obtained by applying the usual set operations on smaller cardinals. More abstractly speaking $\kappa$ is defining a size that is such big that you have to assume it in stead of actually build it from the base case sizes and the operations defined on them. Otherwise you could describe it by having such high information content sucht that it is not possible to "compress" it using smaller cardinals and cardinal operations. If we describe it in formalism $\kappa$ has to satisfy that:
-  $\kappa$ is not a sum of fewer than $\kappa$ smaller cardinals
- $\alpha < \kappa$ implies $2^{\alpha} < \kappa$
Since we covered both cardinal arithmetic opertions, there is no way to construct $\kappa$ from smaller cardinalities. One of the most basic consequences of the axiom of choice is the possiblity of describing every cardinality by cardinal arithmetic. Therefore the existence of an inaccesible cardinal and the axiom of choice is exclusive.

### What means "easy to describe"?
To make it more precise what we mean by a set to be "easy" to describe we introduce the notion of definable by a countable sequence of ordinals. Therefore a set $A$ is cso-definable if there is an $e \in \Omega^{\omega}$ and a formula $\phi(a, b)$ such that $$y \in A \Implies \phi(y, e)$$

## Step 2

### Levy collapse
The construction of the Solovay model relies on a forcing notion called Levy collapse, which plays an important role in many other independence results. The core idea is to take an inaccessible cardinal $\kappa$ and a regular cardinal $\alpha$ and collapse all cardinals smaller than $\kappa$ onto $\alpha$ while specifying $\kappa$ as the cardinal successor of $\alpha$.

### Measurability in the collapsed model
After we have now obtained our forced model $M[G]$, we have to understand why every cso-definable set in $M[G]$ is Lesbegue-measurable in $M[G]$.
For this, we first observe that every measurable set $A$ can be decomposed into the union of a borel set and a null set, i.e. there exists $B \in  \mathbb{B}, N \in \mathbb{N}$ with $A = B \cup N$. Intuitively, we can view this as 
