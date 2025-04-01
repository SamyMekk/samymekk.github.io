---
title: 'An overview of the space of measures and related differential calculus'
date: 2025-07-01
author: 'Samy Mekkaoui'
permalink: /posts/2025/02/Measures_Spaces/
tags:
  - Probability Space
  - Wasserstein Distance
  - Itô's Formula 
---


In this blog post, we will focus on the some analysis on the space of probability measures as it is the main technical component arising in Mc-Kean Vlasov SDEs.

This blog post is mainly inspired from the excellent book of Carmona-Delarue : **Probabilistic Theory of Mean Field Games with Applications I** available [here](https://link.springer.com/book/10.1007/978-3-319-58920-6)




# Table of contents
<!-- no toc -->
1. [Metric Spaces of Probability Measures](#whatIs)
    1. [Distance Between Measures](#Distance-MeasureSpaces)
    2. [Some topological properties of the Wasserstein space $\mathcal{P}_2(\mathbb{R}^d)$](#Topological-Properties)
2. [Differentiability of Functions of Probability Measures](#Differentiability)
    1. [Some notions of derivatives on Probability Measures](#Derivatives-Probability-Measures)
    2. [Itô Formula Along Flow of Measures](#Ito)

   
## Some Metrics on Probability Measures <a name="whatIs"></a>

We will denote by $(E,d)$ a complete separable metric space and the corresponding $\sigma$-algebra $\mathcal{E}$ making $(E,\mathcal{E})$ a mesurable space. In the following, it will be assumed to be the Borel $\sigma$-field $\mathcal{B}(E)$ determined by the topology of $(E,d)$. We will also denote by $\mathcal{P}(E)$ the space of probability measures on $(E,\mathcal{E})$.
### The notion of distance between measures  <a name="Distance-MeasureSpaces"></a>



#### The Lévy-Prokhorov distance

The Lévy-Prokhorov distance between 2 measures $\nu$ and $\mu$ on $(E,\mathcal{E})$ is defined as the following :

$$
\begin{align}
d_{LP}(\mu,\nu) = \text{ inf } &\lbrace \epsilon >  0 :  \forall A \in \mathcal{E}, \mu(A) \leq \nu(A^{\epsilon}) + \epsilon   \\ 
 &\text{ and } \nu(A) \leq \mu(A^{\epsilon}) + \epsilon \rbrace  \notag 
 \end{align}
$$
where $A^{\epsilon} = \lbrace x  \in E : \exists y \in A : d(x,y) < \epsilon \rbrace$. Note that $A \subset A^{\epsilon}$ but we want the distance $d$ to control in the same way the distance between $\mu$ and $\nu$.
There exists a simplest caracterization of ths result defined as the following :

$$
\begin{align}
d_{LP}(\mu,\nu) = \text{ inf }\lbrace \epsilon > 0 : \underset{\pi \in \Pi(\mu,\nu)}{\text{ inf}} \int_{E \times E} \mathbb{1}_{d(x,y) > \epsilon} \pi(dx,dy) < \epsilon \rbrace 
\end{align}
$$
where $\Pi(\mu,\nu)$ denotes the set of probability measures on $E \times E$ with $\mu$ and $\nu$ beeing his first and second marginals.

This representation is more practicable as it leads to easiers computations. For example, when $\mu = \delta_{x}$ and $\nu = \delta_{y}$ for $x,y \in E$. We can see that $d_{LP}(\mu,\nu) = 1 \wedge d(x,y)$.


$\textit{Proof : }$
Consider the case when $d(x,y) \geq 1$. Therefore, in this case, we have for $\epsilon < d(x,y)$ $$\int_{E \times E} \mathbb{1}_{d(x,y) > \epsilon} \pi(dx,dy) = 1$$
Therefore, it is clear that $d_{LP}(\mu,\nu) = 1$.

Now if we consider the cas when $d(x,y) < 1$. Therefore, in this case, we still have, for $\epsilon < d(x,y) < 1$ but for $\epsilon \geq d(x,y)$ the integral becomes null meaning that $d_{LP}(\mu,\nu) = d(x,y)$. Finally, we have $d_{LP}(\mu,\nu) = 1 \wedge d(x,y)$.


#### The total variation distance 

The total variation distance between 2 measures $\mu$ and $\nu$ on $(E,\mathcal{E})$ is defined as the following : 

$$
\begin{align}
d_{TV}(\mu,\nu) = 2 \underset{A \in \mathcal{B}(E)}{\text{ sup}} | \mu(A) - \nu(A) |
\end{align}
$$

When choosing $$\epsilon = \underset{A \in B(E)}{\text{ sup }} | \mu(A) - \nu(A) |$$ 
for $\mu,\nu \in \mathcal{P}(E)$ in the definition of $d_{LP}(\mu,\nu)$ , we have : 

$$
\begin{align}
d_{LP}(\mu,\nu) \leq \frac{1}{2} d_{TV}(\mu,\nu) \notag 
\end{align}
$$
Moreover, we have another representation of $d_{TV}(\mu,\nu)$ as follows :

$$
\begin{align}
d_{TV}(\mu,\nu) = 2 \underset{ \pi \in \Pi(\mu,\nu)}{\text{ inf }} \int_{E \times E} \mathbb{1}_{x \neq y} d\pi(x,y)
\end{align}
$$

##### Representation in terms of random variables.

If we assume that the probability space ($\Omega,\mathcal{F},\mathbb{P})$ is atomless in the sense that $\forall A \in \mathcal{F}$, if $\mathbb{P}(A) > 0$ there exists $B \subset A$ and $B \in \mathcal{F}$  such that $\mathbb{P}(B) > 0$. Then, for any distribution $\mu \in \mathcal{P}(E)$, we can construct a random variable $X : \Omega \to E$ such that $\mathcal{L}(X) = \mu$. (This result is admitted). If we apply this result to the space $E \times E$ equipped with a product distance, we see  that whenever $\mu$ and $\nu$ are probability distrubtions and $\pi \in \Pi(\mu,\nu)$, we can find a pair of random variables $(X,Y) : \Omega \to E \times  E$ such that $\pi=\mathcal{L}(X,Y)$. Therefore, we have  2 probabilistic representations for $d_{LP}(\mu,\nu)$ and $d_{TV}(\mu,\nu)$ given by :


$$
\begin{align}
&d_{LP}(\mu,\nu) = \text{ inf } \lbrace \epsilon > 0 : \underset{\mathcal{L}(X)=\mu, \mathcal{L}(Y) = \nu}{\text{ inf }} \mathbb{P}\left[d(X,Y)> \epsilon\right] < \epsilon \rbrace \notag \\
&d_{tv}(\mu,\nu) =  2  \underset{\mathcal{L}(X)=\mu, \mathcal{L}(Y) = \nu}{\text{ inf }}{\mathbb{P}[X \neq Y]} \notag 
\end{align}
$$


### The Wasserstein distance 

Now, we introduce the most famous notion of distance in the space of measures / probabilities. First, we now introduce the notation $\mathcal{P}_{p}(E)$ for the subspace of probability measures with a moment of order p meaning that $\mu \in \mathcal{P}_{p}(E)$ if we have $\int_{E} d(x,0)^{p} \mu(dx) < + \infty$ or for any other choic as it does not play any role in the definition.

Now, for any $p \geq 1$ and $\mu,\nu \in \mathcal{P}_p(E)$, the $p-th$ Wasserstein distance is defined by :

$$
\begin{align}
W_{p}(\mu,\nu) = \underset{\pi \in \Pi(\mu,\nu)}{\text{ inf }}\left[\int_{E \times E} d(x,y)^{p} \pi(dx,dy)\right]^{\frac{1}{p}}
\end{align}
$$
Assuming that there exists atleast one  $\pi \in \Pi(\mu,\nu)$ realizing the infimum in (6). If we denote the notation  $\Pi_p^{opt}(\mu,\nu)$ for all the couplings realizing ths infimum, we can write

$$
\begin{align}
\Pi^{opt}_{\mu,\nu} = \lbrace \pi \in \Pi(\mu,\nu) : W_{p}(\mu,\nu)^p = \int_{E \times E} d(x,y)^{p} \pi(dx,dy) \rbrace
\end{align}
$$
This set is therefore nonempty.


The following proposition is the Duality Kantorovich duality theorem as is particularly important in optimal transport theory.



$\textit{Proposition : }$

Suppose $(E,d)$ is a metric complete space and $\mu,\nu \in \mathcal{P}_{p}(E)$, therefore we have :

$$
\begin{align}
W_{p}(\mu,\nu)^{p} = \underset{(\phi,\psi), \phi(x) + \psi(y) \leq d(x,y)^{p}}{\text{ sup }} \left[\int_{E} \phi(x) \mu(dx) + \int_{E} \psi(y)\nu(dy) \right].
\end{align}
$$
where the supremum is taken over all the bounded continuous functions $(\phi,\psi)$ defined on $E$. Moreover, if $\pi \in \Pi^{opt}(\mu,\nu)$ there exists $\phi \in L^1(E,\mu)$ and $\psi \in L^1(E,\nu)$ such that we have for $\pi$ almost $(x,y) \in E \times E$,
$$
\begin{align}
\phi(x) + \psi(y) = d(x,y)^p
\end{align}
$$

##### Representation in terms of random variables

Consider now an atomless probability space $(\Omega,\mathcal{F},\mathbb{P})$. A random variable $X : \Omega \to E$ is said to be of order p for $p \geq 1$ if $\mathbb{E}[d(x_0,X)^{p}] < + \infty$ for any $x_0 \in E$. If $X$ and $Y$ are 2 random variables of order p, then we have :

$$
\begin{align}
W_{p}(\mathcal{L}(X),\mathcal{L}(Y))^{p} \leq \mathbb{E}[d(X,Y)^{p}]
\end{align}
$$
Moreover, for $\mu$ and $\nu$ of order $p$, we have :

$$
\begin{align}
W_{p}(\mu,\nu)^{p} = \text{ inf } \lbrace \mathbb{E}[d(X,Y)^{p}] : \mathcal{L}(X) = \mu, \mathcal{L}(Y) = \nu \rbrace
\end{align}
$$







### Some topological properties of the Wasserstein space $\mathcal{P}_2(\mathbb{R}^d)$ <a name="#Topological-Properties"></a>

To be done asap 

## Differentiability of derivatives on Probability Measures <a name="Differentiability"></a>

### Some notions of derivatives on Probability Measure <a name="Derivatives-Probability-Measures"></a>

To be done asap.

### An Itô Formula Along the flow of measures <a name="Ito"></a>

To be done asap.

