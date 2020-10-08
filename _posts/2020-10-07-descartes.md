---
title: A proof for Descartes' Rule of Signs
---

The Descartes' rule of signs is used to determine the number of possible positive or negative zeros for a polynomial.

It states the following:

> Suppose $$f(x)$$ is the formula for a polynomial function written with descending powers of $$x$$.
> - If $$P$$ denotes the number of variations of sign in the formula for $$f(x)$$, then the number of positive real zeroes is one of the numbers $$\{P, P-2, P-4, ...\}$$
> - If $$N$$ denotes the number of variations of sign in the formula for $$f(-x)$$, then the number of negative real zeroes is one of the numbers $$\{N, N-2, N-4, ...\}$$

## Proof

$$
Let,
f(x) = b_nx^n + b_{n-1}x^{n-1} + b_{n-2}x^{n-2} + ... + b_1x + b_0 \\
h(x) = f(x)/b_n = x^n + a_nx^{n-1} + a_{n-1}x^{n-2} + ... + a_1x + a_0
$$

We note that $$h(x)$$ has the same roots as $$f(x)$$, and has the same sign pattern changes. Thus, for further statements we can use $$h(x)$$ instead.

### I.
For $$h(x)$$, we see that if $$a_0 < 0$$, then $$V[h(x)]$$ is odd, else if $$a_0 > 0$$, then $$V[h(x)]$$ is even. Where $$V[h(x)]$$ is the number of sign changes.

This is so because, between any two coefficients with the same sign, the number of sign variations has to be even. If the coefficients have different signs then the number of sign variations has to be odd. 

We can think of a positive coefficient as a **light on**💡 and a negative coefficient as a **light off**. Then, if we start with a **light on** state, no matter how many transitions we make, to get back to the **light on** state, the number of transitions are even. A similar argument can be made for **light on**💡 to **light off** state.

### II.
Let $$P[h(x)]$$ be the number of positive real roots. Then a similar statement can be made about $$P[h(x)]$$

For h(x) as described above,<br>
If,<br>
$$a_0 < 0$$, then $$P[h(x)]$$ is odd;<br>
else if,<br>
$$a_0 > 0$$, them $$P[h(x)]$$ is even.

This is be proved using the following line of thought

1. All polynomials with real coefficients can be broken down into a product of its real factors (of the form $$(x-c)$$) and quadratic equations(which give its complex roots).

2. Complex roots, in case of a polynomial with real coefficients, always occur in conjugate pairs. i.e $$(a+bi)$$ and $$(a-bi)$$.

3. The quadratic equation obtained from multiplying complex conjugates always has a constant term which is grater than 0.

    $$
    \begin{align*}
    &(x-a-bi)(x-a+bi)\\
    &=(x-a)^2 - (bi)^2\\
    &=(x-a)^2 + b^2\\
    &=x^2 - 2ax + a^2 + b^2
    \end{align*}
    $$

    Since $$a^2 + b^2$$ cannot be negative, we conclude that the costant is always positive.

4. $$a_0$$ in $$h(x)$$ is the product of negation of all the positive and negative roots and the constant terms in quadratic eqautions obtained by breaking $$f(x)$$ into its factors.<br>
    For example:<br>
    $$
    \begin{align*}
    &f(x) = (x+c_1)(x-c_2)(x^2-2ax+a^2+b^2) \\
    &When\;we\;expand\;f(x) \\
    &a_0 = (c1)(-c_2)(a^2+b^2)
    \end{align*}
    $$

5. We can see that $$a_0 < 0$$ only when there are an odd number of positive roots.
    Negative and complex roots cannot change the sign of $$a_0$$ because negative roots are of the form $$(x+c)$$ in the factorization, and the constant term due to complex roots is positive as seen in **3**.

6. We can now conclude that the number of positive roots $$P[f(x)]$$ is odd, if $$a_0 < 0$$, and it is even, if $$a_0 > 0$$.

### III.
$$V[f(x)(x-n)] > V[f(x)]$$

Let's suppose that the variation in sign changes for $$h(x)$$ is as follows: $$++--+-+$$. For $$(x-n)$$ its $$+-$$

Multiplying, we have

$$
\begin{align*}
++--+-+& \\
+-& \\
\hline
--++-+-& \\
++--+-+\hspace{1em}&\\
\hline
+\#-\#+-+-&
\end{align*}
$$

The $$#$$ could end up positive or negative depending on the absolute value of the coefficients.

Notice however, that the $$#$$ symbol only appears before and after a transition in sign in $$h(x)$$. Therefore, they must always appear between two opposing signs. Also, the original number of sign changes is preserved, and the unknown $$#$$ signs may only increase the number of sign variations.

From $$I.$$, we can say that between any two opposite signs, the number of sign changes has to be odd. So, it does not matter what sign $$#$$ takes between any given $$+$$ and $$-$$ sign. The number of sign changes remains an odd number. 

We can also see from the result in the bottom row of the above computaion that that $$(x-n)$$ term contributes an additional sign change at the end(rightmost sign).

Thus, we conclude that the $$V[h(x)(x-n)] > V[h(x)]$$. And, that if $$V[h(x)]\;is\;odd$$ then $$V[h(x)(x-n)]\;is\;even$$ and vise versa. 

$$\therefore$$ multiplying a polynomial with a **positive factor**$$(x-n)$$ increases the number of sign changes, and changes the polarity from odd to even or vise versa.

### IV.
Let a function $$f(x)$$ be denoted by
$$
f(x) = N(x)(x-p_1)(x-p_2)(x-p_3)
$$
Where $$N(x)$$ is only composed of negative and complex roots.

From $$II.$$ we note that the number of sign changes in $$N(x)$$ has to be even.

Multiplying by $$(x-p_1)$$ will lead to an odd number of sign changes, greater then the number of sign changes in $$N(x)$$. Next multiplying by $$(x-p_2)$$ will lead to an even number of sign changes, greater than the number of sign changes in $$N(x)(x-p_1)$$.

We can now conclude that for an even number of sign changes there are even positive roots, and the number of roots can at maximum be equal to the number of sign changes. A similar argument can be made for odd number of sign changes.

Now, if there are $$P$$ sign changes, we can say that there can be $$\{P, P-2, P-4...\}$$ positive roots in the function.

The second result of the theroem can easily be derived from the fact that $$f(-x)$$ is just $$f(x)$$ flipped about the y-axis. i.e the negative roots of the function now occur on the positive x-axis. 

Q.E.D