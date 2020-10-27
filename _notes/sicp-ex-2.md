---
title: Exercises 1.9-1.15
book: sicp
chapter: Building Abstractions with Procedures 
chapter-num: 1
section: 1.2.1
---

### Exercise 1.9

For process 1 the evaluation takes place as follows:
~~~scheme
(+ 4 5)
(inc (+ 3 5))
(inc (inc (+ 2 5)))
(inc (inc (inc (+ 1 5))))
(inc (inc (inc (inc (+ 0 5)))))
(inc (inc (inc (inc 5))))
(inc (inc (inc 6)))
(inc (inc 7))
(inc 8)
9
~~~
Since, the process is charaterized by a chain of deferred operations, it is a recursive process.

For process 2 the evaluation takes place as follows:

~~~scheme
(+ 4 5)
(+ 3 6)
(+ 2 7)
(+ 1 8)
(+ 0 9)
9
~~~
The process is an iterative process.

### Exercise 1.10

Part 1:
For (A 1 10)
~~~scheme
(A 1 10)
(A 0 (A 1 9))
(A 0 (A 0 (A 1 8)))
...
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 1))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 2)))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 4))))))))
...
1024 ;2^10
~~~
From this we can also deduce that **(g n)** in the latter part of the exercise calculates $$2^n$$

For (A 2 4)
~~~scheme
(A 2 4)
(A 1 (A 2 3))
(A 1 (A 1 (A 2 2)))
(A 1 (A 1 (A 1 (A 2 1))))
(A 1 (A 1 (A 1 2)))
...

; Since, from the first procedure, we could decude that (A 1 n) = 2^n
;the above application will translate to (((2^2)^2)^2)
65536
~~~
From this we can decude that **(h n)** in the latter part of the exercise calculates $$h(n) = 2^{h(n-1)}$$, where $$h(1) = 2$$ and $$h(0) = 0$$.

For (A 3 3)
~~~scheme
(A 3 3)
(A 2 (A 3 2))
(A 2 (A 2 (A 3 1)))
(A 2 (A 2 2))
...

; Since, we know the value of A(2 n), we can calulate the final value
(A 2 4)
...
65536
~~~

Part 2:

From the definition of the procedure, we can see that (A 0 n) will be evaluated to (* 2 n), i.e $$f(n) = 2n$$
We have already defined (h n) and (g n) in terms of mathematical equations in Part 1.

### Exercise 1.11

Recursive process:
~~~scheme
(define (f n)
  (if (< n 3)
      n
      (+ (f (- n 1)) (* (f (- n 2)) 2) (* (f (- n 3)) 3))))
~~~

Iterative process:
~~~scheme
(define (g n)
  (if (< n 3)
      n
      (g-iter 2 1 0 (- n 2))))

(define (g-iter a b c count)
  (if (= count 0)
      a
      (g-iter (+ a (* 2 b) (* 3 c)) a b (- count 1))))
~~~
[Try in Repl.it](https://repl.it/@TanmayGujar/SICP-EX-111){: .external target="_blank"}

### Exercise 1.12

~~~scheme
(define (pascal elem row)
  (if (or (= elem 0) (= elem row))
      1
      (+ (pascal (- elem 1) (- row 1)) (pascal elem (- row 1)))))
~~~
[Try in Repl.it](https://repl.it/@TanmayGujar/SICP-EX-112){: .external target="_blank"}

We can assign indexes and rows to the numbers in the Pascal's triangle as follows:
~~~
[row 0]        1[0]
[row 1]      1[0] 1[1] 
[row 2]    1[0] 2[1] 1[2]
[row 3]  1[0] 3[1] 3[2] 1[3]
[row 4]1[0] 4[1] 6[2] 4[3] 1[4]
...    ...
~~~
From this we can observe the pattern that given a $$(index, row)$$ pair, the value can be calculated as the sum of value at $$(index-1, row-1)$$ and the one at $$(index, row -1)$$. Also, for any $$row$$, $$(index=row, row)$$ and $$(0, row)$$ are always 1. These observations allow us to define a recursive process as described above.

### Exercise 1.13
Part 1:
<p>
$$
\begin{align*}
Given, \\
\phi &= \frac{1 + \sqrt{5}}{2} \\
\psi &= \frac{1 - \sqrt{5}}{2} \\
To\;prove, \\
fib(n) &= \frac{\phi^n - \psi^n}{\sqrt{5}}\\
\end{align*}
$$
</p>
<p>
$$
\begin{align*}
fib(1) &= \frac{1 + \sqrt{5} - 1 + \sqrt{5}}{2\sqrt{5}} \\
       &= 1 \\
\\
fib(k) &= \frac{(1 + \sqrt{5})^k - (1 - \sqrt{5})^k}{2^k\sqrt{5}} \\
\\
fib(k+1) &= fib(k) + fib(k-1) \\
         &= \frac{(1 + \sqrt{5})^k - (1 - \sqrt{5})^k}{2^k\sqrt{5}} + \frac{(1 + \sqrt{5})^{k-1} - (1 - \sqrt{5})^{k-1}}{2^{k-1}\sqrt{5}} \\
         &= \frac{1}{\sqrt{5}}\left(\left(\frac{1+\sqrt{5}}{2}\right)^{k-1}\left(\frac{1+\sqrt{5}}{2} + 1\right) - \left(\frac{1-\sqrt{5}}{2}\right)^{k-1}\left(\frac{1-\sqrt{5}}{2} + 1\right)\right) \\
         &= \frac{1}{2^k\sqrt{5}}\left(\left(1+\sqrt{5}\right)^{k-1}\left(3+\sqrt{5} \right) - \left(1-\sqrt{5}\right)^{k-1}\left(3-\sqrt{5}\right)\right) \\
         &= \frac{1}{2^k\sqrt{5}}\left(\left(1+\sqrt{5}\right)^{k-1}\left(\frac{1+2\sqrt{5}+5}{2} \right) - \left(1-\sqrt{5}\right)^{k-1}\left(\frac{1-2\sqrt{5}+5}{2}\right)\right) \\
         &= \frac{1}{2^{k+1}\sqrt{5}}\left(\left(1+\sqrt{5}\right)^{k-1}(\left(1+\sqrt{5}\right)^2 - \left(1-\sqrt{5}\right)^{k-1}\left(1-\sqrt{5}\right)^2\right) \\
         &= \frac{(1 + \sqrt{5})^{k+1} - (1 - \sqrt{5})^{k+1}}{2^{k+1}\sqrt{5}} \\
         Q.E.D
\end{align*}
$$
</p>

Part 2:
**To prove**: $$fib(n)$$ is the closest integer to $$\frac{\phi^n}{\sqrt{5}}$$.

<p>
$$
fib(n) = \frac{\phi^n - \psi^n}{\sqrt{5}} \\
Now, |\psi| < 1 \\
\therefore |\psi|^n < 1 \\
The\;maximum\;value\;of\;\frac{\psi^n}{\sqrt 5}\;occurs\;at\;n=1 \\
i.e\;\frac{\psi}{\sqrt{5}} \approx -0.2763
$$
</p>

Since, $$\frac{\phi^n}{\sqrt{5}} = fib(n) + \frac{\psi^n}{\sqrt{5}}$$ , and $$ fib(n) $$ is an integer, also $$ \lvert \frac{\psi^n}{\sqrt{5}} \rvert < 0.5 $$ , we conclude that $$fib(n)$$ is the closest integer to $$\frac{\phi^n}{\sqrt{5}}$$ .

### Exercise 1.14

![Process graph](/assets/images/sicp-ex-1-14-a.png)
{: .img-flex}

Figure above shows the tree generated by the process when making a change of 11 cents. The pairs denote (amount, kinds-of-coins). Below figure illustrates the tree generated when *kinds-of-coins* is 1.

![Process graph contd](/assets/images/sicp-ex-1-14-b.png)
{: .img-flex}

#### Order of growth of space:
We can see that the number of deferred operations depend on the maximum depth of the tree.
This depth depends on the **kinds-of-coins**, their **denominations**, as well as the **input** amount. It is fairly obvious that the longest chain of deferred operations must contain the partial chain which originates from the **highest amount** paired with the **lowest denomination**, which is $$(11, 1)$$ in this case.

Since, other variables i.e **kinds-of-coins** and their **denominations** are constant. We can say that they increment the depth of a tree by a constant for any input amount. 

As the lowest denomination is **1**, the longest partial chain of variable length is of length $$11$$. Or, the depth depends directly on the value of the input, and the order of growth of space is $$\Theta(n)$$.

#### Order of growth of number of steps:
Consider for a moment that there exits only one possible denomination "50"(i.e kinds-of-coins can only be 1 or 0), and the change amount is 100. The process described by the given procedure would make a total of 5 recursive calls as shown:

~~~scheme
(cc 100 1)
(+ (cc 100 0) (cc 50 1))
(+ 0 (+ (cc 50 0) (cc 0 1)))
(+ 0 (+ 0 1))
1
~~~
i.e the number of recursive calls in given by 
<p>
$$
R(n) = 2n+1
$$
</p>
Where **n** is given by the depth of the tree, or the value of $$amount/denomination$$ rounded down. Ignoring the multiplier "2" and the constant "1", we can say that the order of growth is $$\Theta(n)$$ or $$\Theta(amount)$$. 

If we look at the process closely, we see that the amount, as it trickles down follows the pattern:
100 -> 50 -> 0 or expressed in the form of a list (100, 50, 0). 

For any "amount" **a**, and a single given "denomination" **d**, the terms in the list can be written as:
<p>
$$
\left(a, a-d, a-2d, ... a-nd \right),where\;n=\left\lfloor \frac{a}{d} \right \rfloor
$$ 
</p>

In case that there were two denominations, say "25 and 50". Each of the values in the list is used as the starter amount to be divided by denomination "25". Each element in the list above, produces a similar list for denomination "25". The process is shown below:
~~~
denomination: 50

amount: (100)
1ist0: (100, 50, 0)

denomination: 25

amount: list0[0] = 100
list1_a: (100, 75, 25, 0)

amount: list0[1] = 50
list1_b: (50, 25, 0)

amount: list0[2] = 0
list_c: ()

list1:  list1_a + list1_b + list1_c
~~~
i.e the total number of recursive calls are given by:
<p>
$$
\begin{align*}
R(n) &= (2*n_0 + 1) + (2*n_1 + 1) + (2*n_2 + 1) \\
where,n_i &= \left\lfloor \frac{a - id_1}{d_2} \right \rfloor \\
d1 &= 50 \\
d2 &= 25 \\
i &= 0, 1, ...\left\lfloor \frac{a}{d_1} \right \rfloor
\end{align*}
$$
</p>

From the above treatment, we can say that if there are only two denominations $$d_1\;and\;d_2$$ then
<p>
$$
\begin{align*}
R(n) &= \sum_{i=0}^{\left\lfloor \frac{a}{d_1} \right \rfloor}(2*\left\lfloor \frac{a - id_1}{d_2} \right \rfloor + 1) \\
Order\;of\;growth\; &= \Theta(a^2) \\
\end{align*}
$$
</p>

For 5 such levels of denomination, $$ Order\;of\;growth\; = \Theta(a^5) $$, and for **k** such levels:
<p>
$$
Order\;of\;growth\; = \Theta(a^k)
$$
</p>

### Exercise 1.15

**Part a:**

~~~scheme
(sine 12.15)
(p (sine 4.05))
(p (p (sine 1.35)))
(p (p (p (sine 0.45))))
(p (p (p (p (sine 0.15)))))
(p (p (p (p (p (sine 0.05))))))
...
~~~
$$\therefore$$ p is applied 5 times.

**Part b:**

Order of growth of number of steps depends on the number of time **p** is called. From the definition of **p**, we can see that the angle is repeatedly divided by 3, till its value is less than some precision **c**.
<p>
$$
\frac{a}{3^k} \lt c \\
Here,\;k\;is\;the\;number\;of\;times\;p\;is\;called \\
a \lt c3^k \\
log(\frac{a}{c}) \lt klog(3) \\
k \gt \frac{loga - logc}{log3}
$$
</p>

$$\therefore$$ order of growth of number of steps is $$\Theta(loga)$$

Since, the compiler needs to retain how many times **p** is to be called, the order of growth of space is also $$\Theta(loga)$$.
