---
layout: post
title:  "Matrix multiplication and matplotib"
date:   2019-05-22 18:00:00 +0800
categories: [thoughts, tutorials]
tags: [linear-algebra, matplotlib]
asset_path: "/assets/matrix-multiplication-and-matplotlib/"
---

A note: this post isn't really meant to be instructional; it's more a way for me to note down something that I just learnt, so explanations will be sparser and less common. Feel free to contact me if you don't get anything, though.

For a long while I've wondered "why is matrix multiplication so weird?" The intuitive ways of multiplying two groups of numbers together, to me, is to take the elementwise product (which I now know is called the *Hadamard product*).

When I learnt about matrices, over 10 years ago now, in school, I didn't really have anywhere to get an answer to that question, since "why" questions weren't really prized in the education system, and Wikipedia and Google weren't the behemoths that they are now. It was never really relevant to any part of my life then, anyway, so I forgot about it.

In the past few months, though, matrices have come up at least tangentially in my research on machine learning, but I again dismissed the "why" as "just one of those things".

More recently, I was playing with the `matplotlib.transforms` module, and noticed that the transformations were represented as 3x3 matrices. I also read, in a few places, that "a matrix can represent a transformation". To me, that made absolutely no sense, so I started digging.

I came across [this question](https://math.stackexchange.com/questions/31725/intuition-behind-matrix-multiplication) on Math Stack Exchange, which cast matrix multiplication as *linear function composition*, and it started to make sense.

If a matrix can represent a linear function, then that is why matrix multiplication, with suitable manipulation, can be used to find the optimal coefficients for a linear regression. That means that if a transformation can be performed by applying a linear operation to a point's coordinates, *it can also be expressed in matrix form*.

I started with the following:

$$
\Bigg(
\begin{matrix}
a & b \\
c & d
\end{matrix}
\Bigg)

\Bigg(
\begin{matrix}
x \\
y
\end{matrix}
\Bigg)

=

\Bigg(
\begin{matrix}
ax + by \\
cx + dy
\end{matrix}
\Bigg)
$$

Once I started experimenting with possible values of $a$, $b$, $c$ and $d$, it became much clearer. For example, the identity matrix would have $a$ and $d$ equal to 1 and the others to 0, which would mean that multiplying the identity matrix by a point would leave it unchanged - exactly as expected.

$a$ and $d$, then, would control scaling of each axis. Changing $b$ and $c$ would effectively perform a shear transformation, where one axis is scaled based on the *other* axis.

And then I wondered: how can I represent a *translation* with this? Since each element of the matrix product depends on either $x$ or $y$, there would be no way to represent the addition of a constant value.

That, of course, is why a 2D transformation is represented by a 3x3 matrix in `matplotlib`: the transformations instead take on this form:

$$
\begin{pmatrix}
a & b & c \\
d & e & f \\
0 & 0 & 1
\end{pmatrix}

\begin{pmatrix}
x \\
y \\
1
\end{pmatrix}

=

\begin{pmatrix}
ax + by + c \\
dx + ey + f \\
1
\end{pmatrix}
$$

Through the inclusion of an additional row in both the transformation and the point matrices, we can achieve the effect of adding a constant to the result, based on the values of $c$ and $f$.

Another cool thing I realised about using matrix multiplication is that you can *apply it to an arbitrary number of points*, for example (using the 2-row form to save space):

$$
\begin{matrix}
a & b \\
c & d \\
\end{matrix}

\begin{matrix}
x_0 & x_1 & ... & x_n \\
y_0 & y_1 & ... & y_n \\
\end{matrix}

=

\begin{matrix}
ax_0 + by_0 & ax_1 + by_1 & ... & ax_n + by_n \\
cx_0 + dy_0 & cx_1 + dy_1 & ... & cx_n + dy_n \\
\end{matrix}
$$

It really does make sense; everything is connected. If we had been taught this way back then, more people would probably have been interested in what seemed like just a useless mathematical curiosity.

<img src="{{ page.asset_path }}/rotation.png" width="55%" align="right"/>

Incidentally, I didn't talk about rotation. After a fair bit of drawing, which I transferred to `matplotlib` for precision, I got something like what you can see on the right.

Say we have an initial point $(x_0, y_0)$, and we rotate it counter-clockwise by some angle $\alpha$, giving the point $(x_1, y_1)$. How can this rotation be represented as a matrix?

To answer that, I first had to think how to represent those coordinates in terms of $\theta$ and $\alpha$. By following the dotted lines, we can see that $x_0 = cos \theta$ and $y_0 = sin \theta$, assuming that the length of the hypotenuse is 1. If it's not, then we just multiply both by its length. 

Applying the same logic to the post-rotation point, $x_1 = cos (\theta + \alpha)$ and $y_1 = sin (\theta + \alpha)$. If we can find some way to express the equations for $x_1$ and $y_1$ in terms of those for $x_0$ and $y_0$, then we can create a matrix equation looking like the ones above and find a general matrix form for rotational transformations.

For this part, I just looked up [trigonometric identities](https://en.wikipedia.org/wiki/List_of_trigonometric_identities#Angle_sum_and_difference_identities), since I remembered there were formulae for this kind of thing, just not what exactly they said. Applying them here, we get the following:

<div align="center">
$$
\begin{align}
x_1 &= cos(\theta + \alpha) \\
    &= cos \theta\ cos \alpha - sin \theta\ sin \alpha \\
y_1 &= sin(\theta + \alpha) \\
    &= sin \theta\ cos \alpha + cos \theta\ sin \alpha
\end{align}
$$
</div>

This looks much better. Now, putting it back into the matrix multiplication form above (again, the 3rd row isn't necessary here):

$$
\begin{align}
\begin{matrix}
a & b \\
c & d \\
\end{matrix}

\begin{matrix}
cos \theta \\
sin \theta
\end{matrix}

&=

\begin{matrix}
cos \theta\ cos \alpha - sin \theta\ sin \alpha \\
sin \theta\ cos \alpha + cos \theta\ sin \alpha
\end{matrix} \\

\begin{matrix}
a cos \theta + b sin \theta \\
c cos \theta + d sin \theta
\end{matrix}

&=

\begin{matrix}
cos \theta\ cos \alpha - sin \theta\ sin \alpha \\
cos \theta\ sin \alpha + sin \theta\ cos \alpha
\end{matrix}
\end{align}
$$

We can now perform simple factorisation to get the values of $a$, $b$, $c$ and $d$ in terms of $\alpha$. By solving for them and substituting the values in the original matrix, we can derive the general matrix form for a rotational transformation of $\alpha$ radians:

$$
\begin{matrix}
cos \alpha & -sin \alpha \\
sin \alpha &  cos \alpha
\end{matrix}
$$

This is more mathematics than I've done in a while, but I actually find myself enjoying it quite a bit, since it's actually applicable to the code I write on a daily basis (and hope to write in the future). I think that's the really important part; linking all these disparate bits of knowledge together and understanding where they fit in the grand scheme of things.