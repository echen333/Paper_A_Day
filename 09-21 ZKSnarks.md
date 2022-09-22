---
title: zkSNARKS in a Nutshell
link: https://chriseth.github.io/notes/articles/zksnarks/zksnarks.pdf
author: Edward Chen
date: 9/21/22
tags: CS, crypto
---

## Day 13 of Summarizing a Paper a Day: zkSNARKS in a Nutshell

Zero Knowledge (ZK) is the new shiny thing. It has blown up recently, so let's dive in to how zkSNARKs actually work. 

ZK-SNARKs, short for succinct non-interactive arguments of knowledge are a level above ZK. ZK refers to proving that you hold a piece of information without actually conveying that information outright, verified using a "witness." With ZK-SNARKs, there is the added requirement for them to be non-interactive, ruling out probabilitistic proofs, and succinct/ using limited forms of homomorphic encodings. 

#### P and NP

P refers to polynomial runtime programs $(O^k),$ where $k$ is countably finite, whereas we formally denote NP to be exponential runtime computation problems. Read more about computation classes here. Formally, we say a problem is NP here if $L(x)=1$ iff a witness $w$ makes $V(x,w)=1$. 

#### SAT

An example of an NP problem is boolean satisfiability (SAT). The problem is finding boolean variables $x_1, x_2, ..., x_n$ that satisfy a boolean formula, e.g. $((x_1\land x_2) \land \neg x_2).$ Formulas include the operands $\lor, \land, \neg$ for or, and, and negation.

We note that SAT is actually NP-complete. This means that any problem in NP can be turned into a SAT problem. For any NP-problem $L$, there is a reduction function s.t.: $$L(x) = SAT(f(x))$$.

#### Reduction Function

we use PolyZero as our reduction function, trying to find $x$ for which $L(x) = 0$. This is equivalent to SAT (where we go for $L(x)=1$) under the reduction;

 - $r(x_1) \vcentcolon = 1-x_1$
 - $r(\neg f) \vcentcolon = 1-r(f)$
 - $r((f\land g)) \vcentcolon = 1-(1-r(f))(1-r(g))$
 - $r((f\lor g)) \vcentcolon = r(f)r(g)$

Plugging in to each equivalence proves that $r(f) =0$ iff $L(f)=1$.  By reducing our SAT problemt, we make it easier to use polynomials, similar to finding roots. We do this efficiently with QSP's.

#### Quadratic Span Programs (QSPs)

In the use case here, a QSP is finding a linear combination of a set of polynomials that is a multiple of another given polynomial. Additionally, a given binanry input string $u$ (from the SAT problem) restricts the polynomial dimensions you can use. 

For the ZK problem, the input $u$ is *accepted* by teh QSP iff there are tuples $a = *a_1, ..., a_m$ and $b = (b_1, ... b_m)$ from the field $F$ s.t.:

 - $a_k, b_k = 1$ if $k=f(i,u[i])$ where $u[i]$ is the ith bit.
 - $a_k, b_k = 0$ if $k=f(i,1-u[i])$ 
 - the target polynomial $t$ divides $v_aw_b$

Once you know the factors, you just need to check that the polynomial $t$ divides $v_aw_b$, which is computationally easy.

The general pipeline is something like: problem -> reduction function -> PolyZero -> QSP -> testing points w/ Fundamental Theorem/ elliptic curves -> ZK multiplier.