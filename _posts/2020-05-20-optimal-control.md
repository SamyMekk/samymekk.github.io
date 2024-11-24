---
title: 'A short introduction to optimal control'
date: 2020-05-20
permalink: /posts/2020/05/optimal-control/
tags:
  - optimal
  - control
---
This is a very non-mathematical and straightforward presentation of what is optimal control. The goal is to see :
1. What it is mainly about,
2. Some ideas to solve the problems we are about to see. 

To convey the main ideas, as briefly and simply as possible, I will present the minimum necessary package. Have some mercy for my informal tone and my lack of precision, I like it this way because it allows our imagination to fill the gaps and go further ;)

# Table of contents
1. [What is optimal control ?](#whatIs)
2. [Can you give some examples ?](#example)
    1. [A toy example : An eagle an a bird](#subparagraph1)
    2. [A more realistic one : Battery management](#battery)
3. [What do we need in optimal control ?](#whatDoWeWant)
4. [How could I get it ?](#solve)
    1. [Bellman principle](#bellman)
    2. [The pipeline](#procedure)
5. [The (explicit) linear quadratic case](#explicit)
    

# Tell me what is optimal control ? <a name="whatIs"></a>

  
Okay let's give a short answer : *You have a thing/system which is moving through time and over which you have a control. Your goal is then to choose the best control in order to minimize a cost*. In general you have **3 features** that you monitor : 
- **A state of a system**, denoted $X$,
- **A control** $\alpha$ over $X$,
- **A Loss criterion** $J$ that you want to minimize.

Your goal is simple : you want to find the best control $\alpha$ over $X$ so that you minimize your cost $J$. So classically we first define a model other the system :  
\\[ X_t = x + \int_0^t b(s, X_s, \alpha_s)ds, \qquad t \in [0, T], \label{eq:dynamic} \\]
where $b$ is a given model. And a loss criterion that we want to minimize
\\[J(t,x,\alpha) = \int_t^T f(X_s, \alpha_s)ds + g(X_T).\\]
The number $J(t,x,\alpha)$ is to be interpreted as *the total ammount you will pay if, on $[t,T]$, you implement the strategy $\alpha$ when your state start at $X_t=x$*. To sum up, what you have to do is simple 
1. Define a **model** of your system, ie : $(t, x,\alpha) \mapsto b(t, x,\alpha)$,
2. Define a **loss criterion** $(t,x,\alpha) \mapsto J(t,x,\alpha)$,
3. **Solve the minimization** problem $ V(t,x)= \inf_{\alpha} J(t,x,\alpha)$.

Note here the introduction of a new function : the **value function** $V$. Basically, $V(t,x)$ tells you how much you are going to pay if, starting from $x$ at time $t$, you apply until the end an optimal strategy $\alpha^\star$ (we like to put stars on things that are optimal : $\alpha^\star$ denotes an optimal strategy, so that $V(t,x)=J(t,x,\alpha^\star)$). 


# Humm...can you give me an example ? <a name="example"></a>

## A toy example : An eagle an a bird <a name="eagle"></a>

Assume you are an eagle. You are hungry. You want to eat. So you are seeking a prey to get close to. Here your **state** is your position in space $X \in \mathbb{R}^3$. Your **control** $\alpha$ is your speed, so **the model of your system** is $\frac{d X_t}{dt} = \alpha_t$ with $x$ being your initial position. Or as we prefer to write 
\\[X_t = x + \int_0^t \alpha_s ds. \\]
Sundenly a moving prey appears, its position is $P_t$. Now here is the thing. You want to get close to this beautiful prey, but it is costly for you to go fast, so you decide that your **loss criterion** to minimize will be something like 
\\[\int_0^T \left(\lambda(X_t - P_t)^2 + N\alpha_t^2 \right) dt + X_T^2,\\] 
where $\lambda$ and $N$ are given constants. As you can see, the bigger $\lambda$ is, the more you want to get close to your prey (you *penalize* the fact of being far from it); the bigger $N$ is, the more it costs you to go fast. That's it ! We have defined a **model**, **loss criterion** and the only things that remains to do is to **solve the minimization** problem ! (See below)

## Battery management  <a name="battery"></a>
If you're intersted to see an example closer to the real world with a real implementation with neural nets you can go to this  [posts](https://enzomiller.github.io/posts/2020/06/stochastic-control-storage-deep-learning/) ! (it talks about electricity storage and how to optimally control a battery when you want to minimize the bill. It assumes that you have a solar panel, a random consumption, random market prices, etc. This is a project that I did with the electricity company [EDF](https://fr.wikipedia.org/wiki/%C3%89lectricit%C3%A9_de_France).) 



# Ok so... What do we want ? <a name="whatDoWeWant"></a>

Basically 2 things : you want the **optimal control** $\alpha^*$ and the price you're going to pay, i.e. the **value fonction** $(t,x) \mapsto V(t,x)$. 


# Nice ! How can I get them in general ? <a name="solve"></a>
There are many different ways to approach this. The differents methods ([Bellman principle](https://en.wikipedia.org/wiki/Bellman_equation), [Pontryaguin principle](https://en.wikipedia.org/wiki/Pontryagin%27s_maximum_principle), etc) are of course related and have strong links together. I tend to think that a human being would prefer to see the Bellman principle first, so in this post I will quickly explain what it means and how we can use it to obtain the [Hamilton-Jacobi-Bellman equation](https://en.wikipedia.org/wiki/Hamilton%E2%80%93Jacobi%E2%80%93Bellman_equation) ! 

## What is the Bellman principle ? <a name="bellman"></a>

I stumbled upon this nice sentence on Wikipedia (paraphrased from Bellman's book : *Dynamic programming*) :
> An optimal policy has the property that whatever the initial decision is, the remaining decisions must constitute an optimal policy (with regard to the state resulting from the first decision)
>
> <cite><a href="https://en.wikipedia.org/wiki/Bellman_equation#Bellman's_principle_of_optimality">Wikipedia</a> (I slightly modified the sentence)</cite>

So now imagine yourself wanting to act optimally. You are in state $x$ at time $t$ and you want to know the minimal price you will pay, i.e. $V(t,x)$ if from now on you act optimally. Recall the definition of the value function :
\\[
  V(t,x) = \inf_\alpha \left( \int_t^T f(X_s, \alpha_s) ds + g(X_T) \right).
\\]
**Now pay attention we will use the Bellman principle !** let's use the citation and cut our trajectory into 2 pieces : one from $t$ to $t+h$, and another one from $t+h$ to $T$ : 
\\[
  V(t,x) =\inf_\alpha \left(  \int_t^{t+h} f(X_s, \alpha_s) ds + \left( \int_{t+h}^T f(X_s, \alpha_s) ds +g(X_T)\right) \right).
\\]
Then the thing is to notice that **the remaining decisions must constitute an optimal policy** yields the following identity :
\\[
  V(t,x) =\inf_\alpha \left(  \int_t^{t+h} f(X_s, \alpha_s) ds + V(t+h,X_{t+h}) \right).
\\]
Now let's see what happends when $h \to 0$ and do a little [Taylor expansion](https://en.wikipedia.org/wiki/Taylor_series) :
\\[
   V(t,x) = \inf_a \Bigg( h  f(x, a)   + V(t,x) + h \bigg(\partial_t V (t,x) + \langle \partial_x V (t,x), b(x,a) \rangle \bigg) + o(h) \Bigg).
\\]
Here the $o(h)$ notation denotes a function such that $\frac{o(h)}{h} \to 0$ (see [this page](https://www.tutorialspoint.com/little-oh-notation-o) to know more).  Then, by simplifying by $V(t,x)$ and dividing by $h$ we get 
\\[
   0 = \inf_a \left( \partial_t V (t,x) + f(x, a)+   \langle \partial_x V (t,x), b(x,a) \rangle \right).
\\]
Note that $\partial_t V (t,x)$ does not depend on the control so the **HJB equation** reads 
\\[
   0 =  \partial_t V (t,x) + \inf_a \left(f(x, a)+   \langle \partial_x V (t,x), b(x,a) \rangle \right), \qquad t \in [0,T], x \in \mathbb{X}, \;\;\boldsymbol[1]
\\]
where $\mathbb{X}$ is the space where you system evolves. $\mathbb{X}$ could be $\mathbb{R}$, $\mathbb{R}^d$, a Hilbert space, ... actually whatever space where you can define a calculus ! Note also that from the definition of the value function $V(t,x) = \inf_\alpha \left( \int_t^T f(X_s, \alpha_s) ds + g(X_T) \right)$, we necessarily have the additional constrain on $V$ : 
\\[
  V(T,x) = g(x), \qquad \boldsymbol[2]
\\]
and that's it ! The combination of **[1]** and **[2]** constitutes the **HJB equation**. It is a **partial differential equation with a terminal condition**. Now you know that the **value function** solves **[1]** and **[2]**.

## Ok so...what should I do now ?  <a name="procedure"></a>

From now on the procedure is very simple :
1. Solve the **HJB equation** (i.e. **[1]**-**[2]**) to obtain the value fonction $(t,x) \mapsto V(t,x)$.
2. At every (t,x), find an optimal control $\alpha^\star(t,x)$ such that 
\\[
\inf_a \left(f(x, a)+  b(x,a)\partial_x V (t,x) \right) = f(x, \alpha^\star(t,x))+  b(x,\alpha^\star(t,x))\partial_x V (t,x)
\\]
Great ! At the end of this procedure you will have the value function $(t,x) \mapsto V(t,x)$ and the **optimal control** in a [feedback form](https://en.wikipedia.org/wiki/Feedback#Control_theory) $(t,x) \mapsto \alpha^\star(t,x)$. Now you could ask : "How do I solve the HJB equation ?". I'm glad you ask ! Recently neural nets have been used to solve partial differential equations (see [this article](https://arxiv.org/abs/1708.07469) for instance, or this [blog post](https://enzomiller.github.io/posts/2020/07/nn-pdes/)). Note also that in this [blog post](https://enzomiller.github.io/posts/2020/05/neural-net_optimal-control/) I prensent a quite different way to solve the optimal control problem with neural nets.

# Can you solve the problem explicitly ? (Theory is nice but explicit things also) <a name="explicit"></a>

Usually no, in general we don't have any explicit formulas for the optimal control $\alpha^*$ or the value $(t,x) \mapsto V(t,x)$ of the problem. But in some cases we do have explicit formulas ! The most classical one is the linear quadratic case where the **model** is of the form : 
\\[  X_t = X_0 + \int_0^t (A X_s + B X_s) dt, \\]
and the **cost criterion**  : 
\\[J(t,x,\alpha) = \frac{1}{2} \int_t^T Q X_s^2 + N \alpha_s^2 ds + P X_T^2, \\]
were $A,B,Q,N$ could be real numbers, matrices, [linear operator](https://mathworld.wolfram.com/LinearOperator.html), etc.
As you may have noticed the **eagle & bird** problem has this kind of structure, so let's solve it ! 

First let's find the **value function**, i.e. solve the HJB equation. Here $b(x,a) = A x + B a$ and $f(x,a) = Qx^2 + N a^2$, so the HJB equation reads 
\\[
   0 =  \partial_t V (t,x) + \inf_a \bigg(Q x^2 + N a^2+  \langle \partial_x V (t,x), Ax+Ba \rangle  \bigg), \qquad t \in [0,T], x \in \mathbb{X}, V(T,x) = P x^2.
\\]
Then note that $\inf_a \left(Q x^2 + N a^2+  (Ax + B a)\partial_x V (t,x) \right)$ is minimized for 
\\[
a = a^\star(t,x) = - \frac{1}{2}N^{-1}B^\star \partial_x V(t,x).
\\]
Here $B^\star$ denotes the [adjoint](https://www.encyclopediaofmath.org/index.php/Adjoint_operator) of $B$ (You can ignore this detail for now, I just wanted to write the right formula:)). As you may have noticed we now have the optimal control in a feedback form $(t,x) \mapsto a^\star(t,x)$ provided we have the value function. To compute it let's see what the HJB equation looks like now that we have minimized the infimum : 
\\[
   0 =  \partial_t V (t,x) + Q x^2 + A x \partial_x V (t,x) - \frac{N^{-1}}{4} (B \partial_x V(t,x))^2, \qquad V(T,x) = P x^2.
\\]
At this point you could say : "I'm not solving this ! Too boring !". Yes but do not worry, here we are lucky. Try this [ansatz](https://en.wikipedia.org/wiki/Ansatz) for the value function :
\\[
  V(t,x) = K_t x^2, 
\\]
where $t \in [0,T] \mapsto K_t$ is a function that we have to determine. To find $K$, first note that necessarily it must satisfy this final condition $K_T = P$. Now to determine $K$, plug this ansatz into the HJB equation above : you should get
\\[
  0 =  x^2 \bigg( \dot{K}_t Q + A^\star K + K A  - K_t B N^{-1} B^\star K_t \bigg).
\\]
Then note that it is necessary and sufficient for $K$ to satisfy
\\[
  \dot{K}_t + Q + A^\star K_t + K_t A  - K_t B N^{-1} B^\star K_t = 0, \qquad K_T = P
\\]
which is called a [Riccati equation](https://en.wikipedia.org/wiki/Riccati_equation). That's it, we now have what we want ! 
Indeed we have 
- The **value function** : $V(t,x) = K_t x^2$,
- The **optimal control** : $\alpha^\star(t,x) = - N^{-1} B^\star K_t x $.
The only difficulty that remains is to compute $K$. I won't talk about it in this post but be reassured, this not a big issue anymore ! There are plenty of ways to compute solutions of differential equations that I won't go through here but if you're interested into a quite new way of dealing with any differential equations you can go read this [post](https://enzomiller.github.io/posts/2020/07/nn-pdes/) where I give a minimal starter code to solve a differential equation with neural nets ! 



