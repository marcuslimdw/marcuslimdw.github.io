---
layout: post
title:  "More matrix stuff"
date:   2019-06-01 18:00:00 +0800
categories: [thoughts, tutorials]
tags: [linear-algebra]
asset_path: "/assets/more-matrix-stuff/"
---

I've been reading [Deep Learning](https://www.deeplearningbook.org/) by Ian Goodfellow, the inventor of Generative Adversarial Networks (GANs); currently at the introductory chapters, which briefly go into the mathematics behind deep learning, including linear algebra. That means, yes, more matrices.

Some identities are stated without proof; for those coming from mathematical backgrounds, they must seem quite self-apparent. Again, I wanted to *know* that these identities are valid, so I set out to prove them to myself. There were two in particular...

The first, I encountered while trying to solve [this](https://www.deeplearningbook.org/linear_algebra.pdf) exercise. I ended up with, basically, $\textbf{u} \textbf{u}^T + \textbf{u}^T \textbf{u}$. It seemed obvious that they could be combined, but how?

Of course, it was simple: if you can multiply two vectors together, that is the same as the sum of their elementwise product, which is of course associative. In other words:

<div>
$$
\begin{align}
\begin{bmatrix}
a_1 & a_2 & ... & a_n
\end{bmatrix}

\begin{bmatrix}
b_1 \\
b_2 \\
... \\
b_n
\end{bmatrix}

&= a_1 b_1 + a_2 b_2 + ... + a_n b_n \\

\begin{bmatrix}
b_1 & b_2 & ... & b_n
\end{bmatrix}

\begin{bmatrix}
a_1 \\
a_2 \\
... \\
a_n
\end{bmatrix}

&= b_1 a_1 + b_2 a_2 + ... + b_n a_n \\ 
&= a_1 b_1 + a_2 b_2 + ... + a_n b_n

\end{align}
$$
</div>

The second one was much more involved, for me; it was the statement that an orthogonal matrix multiplied by its own transpose is the identity matrix; in other words, for all orthogonal $\textbf{A}$:

<div>
$$
\textbf{A}^T\textbf{A} = \textbf{A}\textbf{A}^T = \textbf{I}
$$
</div>

My reasoning went something like this; say we replace $\textbf{A}$ above with a 3x3 matrix :

<div>
$$
\begin{bmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{bmatrix}

\begin{bmatrix}
a & d & g \\
b & e & h \\
c & f & i
\end{bmatrix}

=

\begin{bmatrix}
a^2 + b^2 + c^2 & ad + be + cf & ag + bh + ci \\
ad + be + cf & d^2 + e^2 + f^2 & dg + eh + fi \\
ag + bh + ci & dg + eh + fi & g^2 + h^2 + i^2
\end{bmatrix}
$$
</div>

The product is equal to the identity matrix:

<div>
$$
\begin{bmatrix}
a^2 + b^2 + c^2 & ad + be + cf & ag + bh + ci \\
ad + be + cf & d^2 + e^2 + f^2 & dg + eh + fi \\
ag + bh + ci & dg + eh + fi & g^2 + h^2 + i^2
\end{bmatrix}

=

\begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1 
\end{bmatrix}
$$
</div>

This gives us the following set of linear equations:

<div>
$$
\begin{align}
a^2 + b^2 + c^2 &= 1 \\
d^2 + e^2 + f^2 &= 1 \\
g^2 + h^2 + i^2 &= 1 \\
\end{align}
$$
</div>

And:

<div>
$$
\begin{align}
ad + be + cf    &= 0 \\
ag + bh + ci    &= 0 \\
dg + eh + fi    &= 0
\end{align}
$$
</div>

These equations can be connected back to the norms and mutual orthogonality of the individual vectors comprising $\textbf{A}$. 

In particular, the first set implies that each row of $\textbf{A}$ is of unit norm. This is because $\textbf{a}_1 = \begin{bmatrix} a & b & c \end{bmatrix}$ and ${\lVert \textbf{a} \rVert}^2_2 = a^2 + b^2 + c^2 = 1$; the same logic applies to the other two rows, taking care of the "normal" part of "orthonormal rows".

The next three equations, then, impose constraints upon orthogonality. They are equal to $\textbf{a}_1 \textbf{a}^T_2$, $\textbf{a}_1 \textbf{a}^T_3$ and $\textbf{a}_2 \textbf{a}^T_3$ respectively, which is equivalent to taking their dot product. 

When the dot product of two vectors is 0, then they *must* be orthogonal, since $\textbf{x} \cdot \textbf{y} = {\lVert \textbf{x} \rVert}_2 {\lVert \textbf{y} \rVert}_2 cos \theta$, where $\theta$ is the angle between them, and $cos \theta = 0$ only for $\theta = \frac{\pi}{2}, \frac{3 \pi}{2}$ (I am not sure if this reasoning is sound or generalisable to higher dimensions, but it seems to make sense for this case.).

The conclusion, therefore, is that if a square matrix multiplied by its transpose is equal to the identity matrix, then its rows area of unit norm and mutually orthogonal.

I spent some time thinking about the *columns*. The above statements say nothing about them, and I tried (fruitlessly) proving that they were also of unit norm and mutually orthogonal from this position.

And then I realised the answer was actually very simple, and came from the fact that $\textbf{A}^T\textbf{A} = \textbf{A}\textbf{A}^T$. By taking the multiplication in the other direction, I got this:

<div>
$$
\begin{bmatrix}
a & d & g \\
b & e & h \\
c & f & i
\end{bmatrix}

\begin{bmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{bmatrix}

=

\begin{bmatrix}
a^2 + d^2 + g^2 & ab + de + gh & ac + df + gi \\
ab + ed + hg & b^2 + e^2 + h^2 & bc + ef + hi \\
ac + df + gi & bc + ef + hi & c^2 + f^2 + i^2
\end{bmatrix}
$$
</div>

The same reasoning as above can be applied to show that if this is to be equal to the identity matrix, then the columns of $\textbf{A}$ must be of unit norm and mutually orthogonal.

As an aside, while I was proofreading this, I was thinking about why functional languages tend to use singly linked lists, since then you can only lengthen/remove elements on one side, then I realised that in an imperative language, you can just modify the pointers/references/etc. of one node. 

However, in a functional language, since data is immutable, you would have to replace that node, which would mean that the previous node's pointer/reference/etc. needs to be changed, which means replacing *that* node, and so on...so you would effectively have to create an entirely new linked list just to change one element.

The functional paradigm really is a different way of thinking.
