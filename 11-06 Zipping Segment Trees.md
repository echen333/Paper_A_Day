---
title: Zipping Segment Trees
link: https://arxiv.org/pdf/2004.03206.pdf
author: Edward Chen
date: 11/06/22
tags: CS, Data Structures
---

I was really surprised to learn about zip trees today. Normally, self-balancing binary trees use rotations to maintain bounds on the height of the tree, but this paper introduces "zipping" trees to do this instead.

Formally, the problem is given a set of intervals [a,b], return all the intervals for which x is in, commonly known as a stabbing query. From figure 1, if we can maintain a segment tree where each node represents a set of intervals, we can perform these queries easily.

![](img/Screen%20Shot%202022-11-08%20at%2012.54.44%20AM.png)

It is also given that we can perform these set operations between nodes easily with the Union-Copy Data Structure. Union-Copy was introduced by van Kreveld and Overmars, and the logic went be reviewed here, but feel free to check it out in the paper. It supports the following operations:

- createSet(): Creates a new empty set in $O(1)$.
- deleteSet(): Deletes a set in $O(1+k\cdot F_N(n))$, where k is the number of elements in the set, and F(n) is the time the find operation takes in one of the chosen union-find structures.
- copySet(A): Creates a new set that is a copy of A in $O(1)$.
- unionSets(A,B): Creates a new set that contains all items that are in A or B, in $O(1)$.
- createItem(X): Creates a new set containing only the (new) item X in $O(1)$.
- deleteItem(X): Deletes X from all sets in $O(1 + k)$, where k is the number of sets X is in.

Let us start by denoting each zip tree having two *spines*: the *left spine* of a subtree is the path from the tree's root to the previous and conversely, the *right spine* is the path from the root node to the next node compared to the root node.

Then, for our segment tree we maintain that for every node, its rank must be greater than their child's rank (similar to in the Optimized Union-Find Data Structure by Tarjan). That way, every path down from the root node is a monotonically decreasing sequence. These ranks can be assigned randomly with probability proportional to frequency of a complete binary tree's depth. As in, rank $k$ should be assigned with probability $(1/2)^{k+1}$.

Thus, the crux is that instead of inserting nodes at the bottom of the tree and then rotating the tree as necessary to achieve balance, these trees determine the height of the node to be inserted before inserting it in a randomized fashion by drawing a rank.

![](img/Screen%20Shot%202022-11-08%20at%2012.48.11%20AM.png)

As illustrated above, for each zip and unzip operation, we have:

**Unzipping routine**. This inserts new into the tree at the position currently occupied by v by first disassembling the search path below v, and then reassembling the different parts as left and right spines below new.

**Zipping routine**. This removes v from the tree, zipping the left and right spines of v. 

![](img/Screen%20Shot%202022-11-08%20at%201.02.30%20AM.png)