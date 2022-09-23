---
title: zkSNARKS in a Nutshell
link: https://chriseth.github.io/notes/articles/zksnarks/zksnarks.pdf
author: Edward Chen
date: 9/21/22, 9/22/22
tags: CS, crypto
---

## Day 13 and 14 of Summarizing a Paper a Day: zkSNARKS in a Nutshell

Zero Knowledge (ZK) is the new shiny thing. It has blown up recently, so let's dive in to how zkSNARKs actually work. 

ZK-SNARKs, short for succinct non-interactive arguments of knowledge are a level above ZK. ZK refers to proving that you hold a piece of information without actually conveying that information outright, verified using a "witness." With ZK-SNARKs, there is the added requirement for them to be non-interactive, ruling out probabilitistic proofs, and succinct, using only limited forms of homomorphic encodings. 

#### P and NP

P refers to polynomial runtime programs $(O^k),$ where $k$ is small and finite, whereas we formally denote NP to be exponential runtime computation problems. Read more about [computation classes](https://en.wikipedia.org/wiki/Complexity_class) here. Formally, we say a problem is NP here if $L(x)=1$ iff a witness $w$ makes $V(x,w)=1$. 

#### SAT

An example of an NP problem is boolean satisfiability (SAT). The problem is finding boolean variables $x_1, x_2, ..., x_n$ that satisfy a boolean formula, e.g. $((x_1\land x_2) \land \neg x_2).$ Formulas include the operands $\lor, \land, \neg$ for or, and, and negation.

We note that SAT is actually NP-complete. This means that any problem in NP can be turned into a SAT problem. For any NP-problem $L$, there is a reduction function s.t.: $$L(x) = SAT(f(x))$$.

#### Reduction Function

We use PolyZero as our reduction function, trying to find $x$ for which $L(x) = 0$. This is equivalent to SAT (where we go for $L(x)=1$) under the reduction;

 - $r(x_1) \vcentcolon = 1-x_1$
 - $r(\neg f) \vcentcolon = 1-r(f)$
 - $r((f\land g)) \vcentcolon = 1-(1-r(f))(1-r(g))$
 - $r((f\lor g)) \vcentcolon = r(f)r(g)$

Plugging in to each equivalence proves that $r(f) =0$ iff $L(f)=1$.  By reframing our SAT problem, we make it easier to use polynomials, similar to finding roots. We do this efficiently with QSP's.

#### Quadratic Span Programs (QSPs)

In the use case here, a QSP is finding a linear combination of a set of polynomials that is a multiple of another given polynomial. Additionally, a given binanry input string $u$ (from the SAT problem) restricts the polynomial dimensions you can use. 

For the ZK problem, the input $u$ is *accepted* by the QSP iff there are tuples $a = *a_1, ..., a_m$ and $b = (b_1, ... b_m)$ from the field $F$ s.t.:

 - $a_k, b_k = 1$ if $k=f(i,u[i])$ where $u[i]$ is the ith bit.
 - $a_k, b_k = 0$ if $k=f(i,1-u[i])$ 
 - the target polynomial $t$ divides $v_aw_b$

The generic translation from our circuits(SAT/reduction) to QSPs is out of the scope of the paper, but we at least know they are in the same complexity class of NP-complete. 

Now, we are left with finding whether $v_aw_b$ is divisibly by $t$. Normally, this is a polynomial time algorithm, but since the number of gates and thus, the degree can be quite high, we need a better way. We utilize the Fundamental Theorem of Algebra here: a polynomial of degree $n$ can be determined with $n+1$ points.Instead of finding the polynomial values, we plug in points to ensure correctness. All you would need now is to caclulate $t(s), v_k(s),$ and $w_k(s)$ to see if $t(s)h(s)=v_a(s)w_b(s)$.

#### The ZK Part

The crux of the ZK relies in using homomorphic encryptions. Here, the verifier randomly generates a point $s$ that is plugged into the polynomials referred to earlier. Using the encryption $E$, they can compute $E(v(s))$ without actually knowing $v_k(s)$. 

How does this encryption $E$ come about? The classic way is using elliptic curves, where we fix a group and a generator. Note that a group element is a *generator* if there is a number $n$ s.t. all the elements in $g^0, g^1,... g^{n-1}$ are distinct. With the $s$ chosen by the verifier, we compute:

$$E(s^0), E(s^1), ..., E(s^d)$$

As stated earlier, the neat thing here is we can compute $E(f(s))$ using only these computations. For example, if $f(x)=4x^2+2x+4$, then $E(f(s)) = E(4s^2+2s+4) = g^{4s^2+2s+4} = E(s^2)E(s^1)^2E(s^0)^4$.

## Final Notes

There is still much groundwork to be laid in ZK, but the idea behind it seemed so cool that when I heard about zkSNARKs, I couldn't figure at all how they could've possibly worked under the hood. They allow for easy verifiability for both very computationally easy and computationally hard problems .

Several very interesting applications are possible from this, most of them stemming from privacy. Websites could offer licenses without needing the specific id of your license or implementing them in [EVM](https://github.com/echen333/Paper_A_Day/blob/master/09-15%20Ethereum_White_Paper.md) could reduce gas fees.