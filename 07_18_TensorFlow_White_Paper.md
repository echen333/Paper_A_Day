---
title: TensorFlow White Paper
link: https://twitter.com/chen_eddi/status/1549358620260806656, https://arxiv.org/pdf/1603.04467v2.pdf
author: Edward Chen
date: 7/18/22
tags: AI, ML
---

# Day 1 of Summarizing AI papers: TensorFlow: Large-Scale Machine Learning on Heterogeneous Distributed Systems (7/18/22)

TensorFlow(TF) is Google’s second-generation AI tool meant for building flexible and large scale ML models. 

Released in 2015, its computations can utilize a variety of hardware, from iOS platforms to large-scale GPU's and everything in between.

Each TF computation is represented by a directed graph, representing a dataflow connection. At the heart of this novelty is their node placement algorithm. The algo attempts to map kernels, which are specific implementations of operations only performable on certain devices, to these respective devices. By starting with a device source, we can DFS, with edges being 2 devices that can connect.

At each node, we either come across a device for which there is no task, one task, (which we just assign), or multiple possible tasks. With multiple tasks, a heuristic estimates the time/cost of computation time. And we greedily assign after. This is somehow good?

When we need to connect the information between devices, though, we might have trouble with synchronization and fault tolerance. One helpful solution is consolidating send and receive nodes by replacing device-traversing edges and directing them to send and receive nodes.

This forces us to send information only once between devices and reduces the complexity of synchronization. 

Meanwhile, in section 4 and 5, the paper goes over specific optimizations for common computations. For example, when calculating the gradient of a matrix from some tensor I on which C depends, it backtracks backwards and combines each operation with the chain rule. Or how TF uses lossy compression to optimize casting between float types. Ngl, some of the other optimizations went straight over my head. 

Section 6 goes over common strategies for development, which mainly includes starting small and building and testing piece by piece. Spending time debugging takes much more than twice the time plus a large amount of frustration. Six nice tips included here.

Finally, in section 7, we utilize the graph model of TF to speed up model training. Assume we are training a model with stochastic gradient descent for 1000 samples. With parallelism, the paper employs ten replicas of the state, each training a mini-batch of 100 samples, but combining them all synchronously at the end, mimicking a sequential SGD. Asynchronously speaking, each device would need their own client model to be able to update, as shown in below.
