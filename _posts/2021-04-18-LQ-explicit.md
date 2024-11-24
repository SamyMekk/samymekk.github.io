---
title: 'A general method for explicit solution of (non markovian) control problem (In progress)'
date: 2021-04-18
permalink: /posts/2020/05/LQ-explicit/
tags:
  - optimal control
  - explicit
  - LQ
---


In this post, we are going to talk about a very easy to use general method to solve (non markovian) control problem explicitly, provided some algebraic assumptions on the structure of the dynamic and cost functional are satisfied.
As explained below, the method allows us to find explicit solutions which might be usefull in the to benchmark RL algorithm on various non markovian problems. The goal here is to make an easy to read presentation, for more details I have linked the technical papers when they exist !



# Table of contents
1. [Reminder on the classical (markovian) Linear-Quadratic case](#lq)
1. [Dynamic with delays][Link to paper](https://hal.archives-ouvertes.fr/hal-03145949v3/document)
2. [Volterra case (rough noise, fractional Brownian motion, etc)](#volterra)
    1. [Convolution type] (See [this paper](https://imstat.org/wp-content/uploads/2020/11/AAP1645.pdf))
    2. [Non convolution type] (unpublished yet)
3. [On the use of the signature : a general linearization on the path space](#sig)(unpublished yet)
