---
title: Exercises 1.16-1.28
book: sicp
chapter: Building Abstractions with Procedures 
chapter-num: 1
section: 1.2.2
---

### Exercise 1.16

~~~scheme
(define (fast-expt b n)
  (fast-exp-iter b n 1))

(define (square x) (* x x))

(define (even? n) (= (remainder n 2) 0))

(define (fast-exp-iter b n a)
  (cond ((= n 0) a)
        ((even? n) (fast-exp-iter (* b b) (/ n 2) a))
        (else (fast-exp-iter b (- n 1) (* a b)))))
~~~
[Try in Repl.it](https://repl.it/@TanmayGujar/SICP-EX-116){: .external target="_blank"}

In the **fast-exp-iter** procedure, the value $$(ab)^n$$ remains constant. In general, the technique of defining an invariant quantity that remains unchanged from state to state is a powerful way to think about design of iterative algorithms.

### Exercise 1.17

~~~scheme
(define (double x) (* x 2))

(define (halve x) (/ x 2))

(define (even? x) (= (remainder x 2) 0))

(define (fast-mult a b)
  (cond ((= b 0) 0)
        ((even? b) (double (fast-mult a (halve b))))
        (else (+ a (fast-mult a (- b 1))))))
~~~
[Try in Repl.it](https://repl.it/@TanmayGujar/SICP-EX-117){: .external target="_blank"}

### Exercise 1.18

~~~scheme
(define (double x) (* x 2))

(define (halve x) (/ x 2))

(define (even? x) (= (remainder x 2) 0))

(define (fast-mult a b)
  (fast-mult-iter a b 0))

(define (fast-mult-iter a b n)
  (cond ((= b 0) n)
        ((even? b) (fast-mult-iter (double a) (/ b 2) n))
        (else (fast-mult-iter a (- b 1) (+ a n)))))
~~~
[Try in Repl.it](https://repl.it/@TanmayGujar/SICP-EX-118){: .external target="_blank"}

### Exercise 1.19

<p>
$$
\begin{align*}
a_1 &\leftarrow b_0q + a_0q + a_0p \\
b_1 &\leftarrow b_0p + a_0q \\
\\
a_2 &\leftarrow b_1q + a_1q + a_1p \\
b_2 &\leftarrow b_1p + a_1q \\
\\
a_2 &\leftarrow (b_0p + a_0q)q + (b_0q + a_0q + a_0p)*(q + p) \\ 
a_2 &\leftarrow b_0(2pq + q^2) + a_0(2pq + q^2) + a_0(q^2 + p^2)\\
\\
b_2 &\leftarrow (b_0p + a_0q)p + (b_0q + a_0q + a_0p)q \\
b_2 &\leftarrow b_0(p^2 + q^2) + a_0(2pq + q^2)
\end{align*}
$$
</p>

From the above equations we can identify that

<p>
$$
p' = q^2 + p^2 \\
q' = 2pq + q^2
$$
</p>

~~~scheme
(define (even? x) (= (remainder x 2) 0))

(define (fib n)
  (fib-iter 1 0 0 1 n))

(define (fib-iter a b p q count)
  (cond ((= count 0) b)
        ((even? count)
         (fib-iter a
                   b
                   (+ (* p p) (* q q))
                   (+ (* q q) (* 2 p q))
                   (/ count 2)))
        (else (fib-iter (+ (* b q) (* a q) (* a p))
                        (+ (* b p) (* a q))
                        p
                        q
                        (- count 1)))))
~~~
[Try in Repl.it](https://repl.it/@TanmayGujar/SICP-EX-120){: .external target="_blank"}

The above program will compute fibonacci numbers in $$logn$$ steps.

### Exercise 1.20

**Normal order evaluation**
Let **noe** denote number of times remainder is calculated. The positions where **noe** is incremented are used to locate the places where remainder operation is actually performed.
~~~scheme
(gcd 206 40)

(if (= 40 0)
    206
    (gcd 40 (remainder 206 40)))

(if (= (remainder 206 40) 0) ; noe = 1 
    40
    (gcd (remainder 206 40) (remainder 40 (remainder 206 40))))

(if (= (remainder 40 (remainder 206 40)) 0) ; noe = 3 
    (remainder 206 40)
    (gcd 
        (remainder 40 (remainder 206 40)) 
        (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))))

(if (= (remainder (remainder 206 40) (remainder 40 (remainder 206 40))) 0) ; noe = 7 
    (remainder 40 (remainder 206 40)) 
    (gcd 
        (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))
        (remainder 
            (remainder 40 (remainder 206 40))
            (remainder (remainder 206 40) (remainder 40 (remainder 206 40))))))

(if (= 
    (remainder 
            (remainder 40 (remainder 206 40))
            (remainder (remainder 206 40) (remainder 40 (remainder 206 40))))
    0) ; noe = 14

    (remainder (remainder 206 40) (remainder 40 (remainder 206 40))) ; noe = 18
    (gcd 
        (remainder 
            (remainder 40 (remainder 206 40))
            (remainder (remainder 206 40) (remainder 40 (remainder 206 40))))
        (remainder 
            (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))
            (remainder 
            (remainder 40 (remainder 206 40))
            (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))))))
~~~
$$\therefore$$ there are 18 remainder operations performed for normal order evaluation.

**Applicative order evaluation**

~~~scheme
(gcd 206 40)
(gcd 40 6)
(gcd 6 4)
(gcd 4 2)
(gcd 2 0)
2
~~~
In case of applicative order evaluation, the operands are always evaluated in the combination in which they are called. This means that the number of times **remainder** operation is performed, is same as the number of times **gcd** is called, which is 5.

### Exercise 1.21

199 and 1999 are prime. 7 is the smallest divisor for 19999.

### Exercise 1.22

~~~scheme
(define (square x) (* x x))

(define (smallest-divisor n) (find-divisor n 2))
(define (find-divisor n test-divisor)
  (cond ((> (square test-divisor) n) n)
        ((divides? test-divisor n) test-divisor)
        (else (find-divisor n (+ test-divisor 1)))))
(define (divides? a b) (= (remainder b a) 0))
(define (prime? n)
  (= n (smallest-divisor n)))

(define (timed-prime-test n)
  (newline)
  (display n)
  (start-prime-test n (runtime)))

(define (start-prime-test n start-time)
  (if (prime? n)
      (report-prime (- (runtime) start-time))))

(define (report-prime elapsed-time)
  (display " *** ")
  (display elapsed-time))

(define (next-odd n)
    (if (= (remainder n 2) 0)
        (+ n 1)
        (+ n 2)))

(define (search-for-primes start end)
  (define next-num (next-odd start))
  (cond ((< next-num end)
       (timed-prime-test next-num)
       (search-for-primes next-num end))))
~~~

The timing data does bear out. The numbers 100,000 and 1,000,000 also support the $$\Theta(\sqrt n)$$ prediction very well. This result is complatible with the notion that programs on the machine run in time proportional to the number of steps required for computation.

### Exercise 1.23

~~~scheme
(define (square x) (* x x))

(define (smallest-divisor n) (find-divisor n 2))
(define (find-divisor n test-divisor)
  (define (next test-divisor)
    (if (= test-divisor 2)
        3
        (+ test-divisor 2)))
  (cond ((> (square test-divisor) n) n)
        ((divides? test-divisor n) test-divisor)
        (else (find-divisor n (next test-divisor)))))
(define (divides? a b) (= (remainder b a) 0))
(define (prime? n)
  (= n (smallest-divisor n)))

(define (timed-prime-test n)
  (newline)
  (display n)
  (start-prime-test n (runtime)))

(define (start-prime-test n start-time)
  (if (prime? n)
      (report-prime (- (runtime) start-time))))

(define (report-prime elapsed-time)
  (display " *** ")
  (display elapsed-time))

(define (next-odd n)
    (if (= (remainder n 2) 0)
        (+ n 1)
        (+ n 2)))

(define (search-for-primes start end)
  (define next-num (next-odd start))
  (cond ((< next-num end)
       (timed-prime-test next-num)
       (search-for-primes next-num end))))
~~~

The expectation isn't confirmed. The ratio of speeds is around 1.4.

### Exercise 1.24

We would expect the time required to be around 2 more than the time required for primes near 1000. The data does not bear this out, and is likely because primitive operations such as multiplication and division take a longer time as the size of the inputs increase.

### Exercise 1.25

Yes, Alyssa is correct! But this procedure would not serve well since multiplication using large numbers execute in a non trivial time. The expmod procedure we defined restricted the value of each iteration to *m*, and hence the time required for multiplication in each iterarion is fairly constant.

### Exercise 1.26

In Louis's procedure, expmod is computed twice. We know that *expmod* executes in $$logn$$ (i.e the input is halved at each step), but since we compute the same value twice at each iteration, the total number number of times expmod is called is equal to $$2^{logn}$$, effectively giving us a $$\Theta(n)$$ process.

### Exercise 1.28

~~~scheme
(define (square x) (* x x))
(define (non-trivial-sqrt base exp m)
  (define num (expmod base (/ exp 2) m))
  (if (and (not (or (= num 1) (= num (- m 1)))) (= (remainder (square num) m) 1))
       0
       (remainder (square num) m)))

(define (expmod base exp m)
  (cond ((= exp 0) 1)
        ((even? exp) (non-trivial-sqrt base exp m))
        (else
         (remainder
          (* base (expmod base (- exp 1) m))
          m))))
  
(define (miller-rabin-test n)
  (define (try-it a)
    (not (= (expmod a (- n 1) n) 0)))
  (try-it (+ 1 (random (- n 1)))))

(define (fast-prime? n times)
  (cond ((= times 0) true)
        ((miller-rabin-test n) (fast-prime? n (- times 1)))
        (else false)))
~~~