---
title: Procedures and the Processes they  generate
book: sicp
chapter: Building Abstractions with Procedures 
chapter-num: 1
section: 1.2.0
---

The ability to visualize the consequences of the actions under consideration is crucial to becoming an
expert programmer. Only then, one can plan the course of action to be taken by a process.

A procedure is a pattern for local evolution of a process. i.e a procedure describes a part of what the process evolution will look like. 

In this section we will study some common patterns of process evolution.

## Linear recursion and iteration

Consider the two snippets for calculating the factorial of a number.

A.
~~~ scheme
(define (fact n) 
(   (if (= n 1) 
        1
        (* n (fact (- n 1)))))

~~~

B.
~~~ scheme
(define (fact n)
    (define (fact-iter prod count)
        (if (> count n)
            (prod)
            (fact-iter (* prod count) n (+ count 1))
    )) 
(fact-iter 1 1))
~~~

The pattern of process evolution for the snippets looks like the following:

A.
~~~
(fact 6)
(* 6 fact(5))
(* 6 (* 5 fact(4)))
(* 6 (* 5 (* 4 fact(3))))
(* 6 (* 5 (* 4 (* 3 fact(2)))))
(* 6 (* 5 (* 4 (* 3 (* 2 fact(1))))))
(* 6 (* 5 (* 4 (* 3 (* 2 1)))))
(* 6 (* 5 (* 4 (* 3 2))))
(* 6 (* 5 (* 4 6)))
(* 6 (* 5 24))
(* 6 120)
720
~~~

B. 
~~~
(fact 6)
(fact-iter 1 1)
(fact-iter 1 2)
(fact-iter 2 3)
(fact-iter 6 4)
(fact-iter 24 5)
(fact-iter 120 6)
(fact-iter 720 7)
720
~~~

**A.** is an example of a recursive process and it is characterized by a series of deferred operations. The characteristic feature of a recursive process is that the compiler has to maintain some sort of memory to keep track of where the process is currently in the set of deferred operations. In this case, it is the set of values required for the deferred multiplication operations.

Another important feature is the requirement of an implementation of a stack. Stack is an artificial data structure which operates in a LIFO manner.

If the amount if information required to track and number of steps increases in a linear fashion w.r.t. the input, then the process is called a **Linear recursive process**.

**B.** is an example of an iterative process. For such a process, each state can be exacly described by a fixed number of state varibles, a rule to move from one state to the next, and possibly a termination case, which limits the number of iteration. If the number of iteration increase linearly with size of input, then the process is called a linear iterative process.

In case of an iterative process, we can stop the process at an iteration and restart the process with the values of the state variables equal to the their values at the point of stop, and the process would compute the correct value. We won't be able to do this in case of a recursive process since the computation does not solely depend on the values of the state variables. 

**Note that a recursive procedure is just the syntatic description of how a procedure is written. Namely that the procedure calls itself. This is entirely different form a recursive process, which actually describes the pattern of process evolution.**

In fact, both recursive and iterative processes are described by recursive procedures in the above examples. Scheme actually does not have iteration structures such as **for, while or do- while** loops. It used recursive procedures to descibe iteration. This is acceptable since the interpreter employs a compiler optimization trick called **Tail recursion** which allows it to compute iterative processes in a constant space. Other language like CPP must use syntactic structures like **loops**, because the they treat all recursive procedures as recursive processes, i.e. they do not use tail recursion.

## Tree recursion

When a recursive procedure makes two or more recursive calls to itself, it describes a tree recursive process.

The following example of computing Fibonacci numbers, evolves as a tree process.

~~~ scheme
(define (fib n)
(cond ((= n 0) 0)
      ((= n 1) 1)
      (else (+ (fib (- n 1)
               (fib (- n 2)))))))
~~~

This process computes same values several times, and is very inefficient way to compute fibonacci numbers. We can show that the fib(0) and fib(1) and computed a total of fib(n+1) times.

~~~
fib(2) = fib(1) + fib(0)
fib(3) = fib(2) + fib(1)
       = 2*fib(1) + fib(0)
fib(4) = fib(3) + fib(2)
       = 3*fib(1) + 2*fib(0)
       .
       .
       .
fib(n) = fib(n)*fib(1) + fib(n-1)*fib(0)

Coeff of fib(1) is fib(n), and that of fib(0) is fib(n-1)
=> fib(n) + fib(n-1) = fib(n+1)
~~~
This value is important as fib(0) and fib(1) are termination cases, therefore the number of procedure calls is much more than this number. The fibonacci number can also be described as the closest interger to $$\phi^n$$.
Where,
<p>
$$
\phi = \frac{1 + \sqrt{5}}{2}
$$
</p>
Thus, the number of steps grows exponentially with the input.

## Orders of Growth

Order of growth of a process is used to obtain a gross measure of the resources required by the process as the inputs become larger.

If $$n$$ is the parameter that measures the size of the problem, and $$R(n)$$ describes the amount of resources required by the process, then $$\Theta(f(n))$$ describes the order of growth. Here, $$f(n)$$ is such that $$k_1f(n) < R(n) < k_2f(n)$$ for any sufficiently large value of $$n$$.

The amount of resources can be measured as the number of steps in the process, or the amount of memory consumed etc.
For e.g if in computing the fibonacci series using an iterative process, we choose $$R(n)$$ as the number of steps required by the process, then the order of growth is $$\Theta(\phi^n)$$.

The order of growth only provides a crude measure of the behaviour of the process. For e.g a process requiring $$n^2$$, and one requiring $$100n^2$$ have the same order of growth. But, it does provide a useful indication of how the behaviour of the process changes as we increase the size of the problem. e.g in case of a linear process the amount of resources required increases by the same amount as the increase in the input, whereas in case of an exponential process, each increment will multiply the resources required by a constant factor.

## Exponentiation

The simplest procedure to calculate $$a^n$$, can be implemented as a recursive call which multiplies $$a$$ by itself $$n$$ times. Such an algorithm would have an $$O(n)$$ order of growth.

~~~ scheme
(define (expt a n)
    (if (= n 0)
        1
        (* a (expt a (- n 1)))))
~~~

However, we can notice that 
<p>
$$
b^n = (n^\frac{n}{2})^2; if\;n\;is\;even \\
b^n = b * b^{n-1}; if\;n\;is\;odd
$$
</p>
an this can be used to define an algorithm which calculates exponents in $$O(logn)$$.

~~~scheme
(define (square a) (* a a))

(define (fast-expt a n)
    (if (= (remainder n 2) 0)
        (square (fast-expt a (/ n 2)))
        (* a (fast-expt a (- n 1)))))
~~~

The above algorithm executes in $$O(logn)$$ steps since the input size (n) is essesntially halved at each step.

## Greatest common divisors

One way to find GCD of two numbers would be to factor the two numbers and then find the product of common factors. However, there is another algorithm, called Euclid's algorithm which is much more efficient.

Euclid's algorithm is based on the fact that $$ GCD(a, b) = GCD (b, r) $$, where $$r$$ is the remainder of $$a / b$$. We can see this fact easily
<p>
a = q*b + r; where\;q\;is\;the\;quotient
</p>
The largest factor of $$a$$ and $$b$$,is also a factor of $$r$$. Since $$r$$ is the largest factor of itself, and any factor of both $$b$$ and $$r$$ is also a factor of $$a$$, finding the GCD of $$b$$ and $$r$$ should give us the correct result.

~~~scheme
(define (gcd a b)
    (if (= b 0)
        a
        (gcd b (remainder a b))))
~~~

#### Lame's theorem
> If Euclid's Algorithm requires $$k$$ steps to compute the GCD of some pair, then the smaller number in the pair must be greater than or equal to the $$k^{th}$$ fibonacci number.

Proof:
Consider $$(a_k, b_k)$$ is some pair that is obtained while calulating the GCD using Euclid's algorithm, where $$a_k > b_k$$. The successive reduction steps are of the form $$(a_{k+1}, b_{k+1})\rightarrow(a_k, b_k)\rightarrow(a_{k-1}, b_{k-1})$$. 

We know that,
<p>
$$
\begin{align*}
a_k &= q*b_k + b_{k-1} \\
b_{k+1} &= q*b_k + b_{k-1} \\
b_{k+1} &\geq b_k + b_{k-1}
\end{align*}
$$ 
</p>

We can now prove, Lame's theorem by induction. For $$k = 1, b_k \geq 1\;and\;Fib(1) = 1$$
The general case is $$b_k > Fib(k)$$
For k+1,
<p>
$$
\begin{align*}
b_{k+1} &\geq b_{k} + b_{k-1} \\
b_{k+1} &\geq Fib(k) + Fib(k-1) \\
b_{k+1} &\geq Fib(k+1) \\
Q.E.D    &
\end{align*}
$$
</p>

## Testing for primality

The simplest solution to test a numbers primality would be to check all the numbers $$>1\;and\;<\sqrt n$$. This method is of the order $$O(\sqrt n)$$. A solution using the described method can be written as follows:

~~~scheme
(define (smallest-divisor n) (find-divisor n 2))

(define (find-divisor n test)
    (cond ((> (square test) n) n)
          ((divides? test n) test)
          (else (find-divisor n (+ test 1)))))

(define (divides? test num) (= (remainder num test) 0))
(define (prime? n) (= (smallest-divisor) n))
~~~

#### The Fermat Test

**Fermat's Little Theorem**: If *n* is a prime number and *a* is any positive number less than *n*, then *a* raised to the *nth* power is congruent to *a modulo n*.

Here, two numbers are said to be *congruent modulo n* if they both have the same remainder when divided by *n*. In simple terms, the statements says that, if we divide $$a^n$$ by $$n$$, then the result will be $$a$$, if $$n$$ is prime.

If n is not prime then, for most numbers, $$a<\n$$, the above relation will not apply. i.e if we select $$k$$ numbers less than n, and check if the above relation applies, then if the relation doesn't apply to any one of them, the number is not prime, else it's prime.

This test can be implemented as follows:

~~~scheme
(define (even? n) (= (remainder n 2) 0)
(define (square x) (* x x))
(define (expmod base exp m)
    (cond ((= exp 0) 1)
          ((even? exp)
            (remainder      ; remainder is applied to keep the numbers below n
                (square (expmod base (/exp 2) m))
                m)) 
          (else (remainder (* base (expmod base (-exp 1) m))
                m))))
(define (fermat-test n)
    (define (try-it a)
        (= (expmod a n n) a))
    (try-it (+ 1 (random (- n 1)))))
; (random n) returns a number between 0 and n,
; thus the above comination returns one between 1 and n-1
(define (fast-prime? n times)
    (cond ((= times 0) true)
          ((fermat-test n) (fast-prime? n (- times 1)))
          (else false)))
~~~
In the above code, repeated application of *remainder* does not alter the final result since squaring doesn't affect remainders.
E.g
Let $a$, be the value at any given step in the recursion, n be the divisor, q and r are quotient and remainder in the division
<p>
$$
\begin{align*}
a = qn + r \\
(remainder\;a^2\;n) &= (remainder\;q^2n^2 + r^2 + 2qnr\;n) \\
&= (remainder\;r^2\;n)
\end{align*}
$$
</p> 
If we take the remainder before applying the square, and take the reminder again we get still $$(remainder\;r^2\;n)$$.

The number of steps grow as $$O(logn)$$, since the exponentiation procedure grows as $$O(logn)$$.

### Probabilistic methods

When a number passes the Fermat's test, it is a strong indication that the number is prime. However, it is not certain that that it is indeed a prime. If the test passes for more cases then we simply improve the probability that the number is prime. Algorithms where the error probability can be made arbitrarily small are called a probablilistic algorithms.

There are some numbers e.g 561, 1105, 1729; which pass fool Fermat's test, these are called Carmichael numbers. Variations of the Fermat's test such as the *Miller-Rabin* test cannot be fooled. 