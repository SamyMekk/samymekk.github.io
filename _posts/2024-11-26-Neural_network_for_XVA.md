---
title: 'Neural network for an efficient XVA computation'
date: 2024-11-26
permalink: /posts/2024/11/neural-net_XVA/
tags:
  - XVA
  - neural network
  - Conditional Expectation Learning
---


We will show how neural networks can help to handle the main issues of the computations of XVAs.


This post will be related to the work I did during my internship as a Quantitative Research at Forvis Mazars during the period May-November 2024. 
To access to a PDF version of the presentation, click [here](https://samymekk.github.io/files/MemoireActuariatSlides.pdf).


# Table of contents
1. [A quick definition of XVAs](#whatIs)
2. [Neural networks as a computational tool ! ](#example)


# What are XVAs  ? <a name="whatIs"></a>



We will focus on computational aspects of $\text{XVAs}$. For a deep understanding of theses quantities, see [this book](https://onlinelibrary.wiley.com/doi/book/10.1002/9781119508991).
<br>
XVAs can be summarized as the following : 


- $\text{CVA}$ and $\text{DVA}$ represent 
- $\text{FVA}$ and $\text{MVA}$ represent
- $\text{KVA}$ represents

In the following, we will focus on the main quantity of interest for banks which is the $\text{CVA}$.
<br>
<br>
The $\text{CVA}$ equation is given by : 


<center>

$CVA_t = \mathbb{E}^{\mathbb{Q}}[(1-R^C) \mathbb{1}_{t \leq \tau^C \leq T} \frac{(V_{\tau^C})^+}{B_{\tau^C}}|\mathcal{G}_t]$

</center>

$\text{with the following notations :}$

- $\mathbb{1}_{t \leq \tau^C \leq T}$ represents the potential default time between t and $T$.
- $R^C$ refers to the recovery rate
- 

# Neural Networks for XVAs <a name="example"></a>
