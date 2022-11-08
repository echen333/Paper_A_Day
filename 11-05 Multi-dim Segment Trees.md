---
title: Multidimensional segment trees can do range updates in poly-logarithmic time
link: https://arxiv.org/pdf/1811.01226.pdf
author: Edward Chen
date: 11/05/22
tags: CS, Data Structures
---

While researching previous research on segment trees, I came across this paper. It appears to be a very well put together paper on segment trees, but I realized that put harshly, there is nothing novel here.

The paper describes 2-d or x-d range sum and updates in poly-logarithmic time using lazy propagation and does a good at that. But published in 2020, the ideas of lazy propagation and multi-dimensional segment trees have been around too long to not have been combined. In fact, there have been lots of algorithmic programming problems that I have done on codeforces.com which readily uses this idea.

I also think that their explanations are overall quite complicated. I'm not sure if this is just a fact as I was already familiar in the space, but there seems to be a lot of complicated sections that weren't needed. For instance, in their Lemma 4.3, I'm not sure why there scaling is:

$$scaling = \frac{x_{end}-x_{start}+1}{x_2-x_1+1} =\frac{||r|||}{||r'||}$$

I think ignoring the size of segment factor in lazy updates is much more effective and removes this part.

In my mind, the quality of papers on arxiv were all supposed to be high quality. But diving deeper reveals complicated and somewhat incomplete sections on an idea that isn't entirely new. It seems really hastily put together.

This also reinforced some of my ideas on algorithms research. I think research in algorithms among newer research often relies on the low-hanging fruit that is either 1) well-known but no one wants to write a paper on it or 2) just way too small in scope.

On a more positive note, I do like the nice, colorful graphics and pictures.

![](img/Screen%20Shot%202022-11-07%20at%2010.25.19%20PM.png)
