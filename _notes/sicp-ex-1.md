---
title: Exercises 1.3-1.8
book: sicp
chapter: Building Abstractions with Procedures 
chapter-num: 1
section: 1.1.1
---

### Exercise 1.3
~~~scheme
(define (square x) (* x x))
(define (sumOfSquares x y) (+ (square x) (square y)))
(define (>= x y) (not (< x y)))
(define (f x y z)
  (cond ((and (>= x y) (>= y z)) (sumOfSquares x y))
        ((and (>= x y) (>= z y)) (sumOfSquares z x))
        (else (sumOfSquares z y))
  )
)
~~~
[Try in Repl.it](https://repl.it/@TanmayGujar/SICP-EX-13){: .external target="_blank"}

### Exercise 1.4
If b is greater than 0, then return (a + b), otherwise, return (a - b).

### Exercise 1.5
The program will crash in case of applicative order evaluation.

In normal order evaluation the expression will get evaluated as follows:

~~~
-> (test 0 (p))
-> (if (= 0 0) 0 (p))
-> 0
~~~
The **(p)** part will never run because if is a special form and the predicate evaluates to true.

In case of applicative order evaluation, the interpreter will try to evaluate all the operands before applying the operator. i.e it will try to evaluate (p) beforehand. This will lead to an infintite series of calls and the program will crash.

### Exercise 1.6

It will lead to infinte loop. **sqrt-iter** gets evaluated each time because it is an operand and **new-if** is a normal procedure unlike **if** which is a special form. In case of a normal procedure, all the expressions are evaluated before applying the operator to the operands.

### Exercise 1.7

With the precision set to $$ 10^{-3} $$ , i.e the difference between the square of the guessed value and square of the actual root is allowed to differ at most by $$ 10^{-3} $$ , we find that the difference between the guessed root and the actual root is large. This is because, for small values << 1, if we look at the graph of $$ x^2 $$, we find that the for small change in y, the corresponding change in x is larger.


<iframe src="https://www.desmos.com/calculator/goqcwtfbjs?embed" width="500px" height="500px" style="border: 1px solid #ccc" frameborder=0></iframe>

For larger numbers, the interpreter drops the numbers with lower base which causes erronous subtraction and results in a wrong root.
~~~scheme
(define (sqrt-iter guess x)
  (if (good-enough? (improve guess x) guess)
      guess
      (sqrt-iter (improve guess x) x)))

(define (improve guess x)
  (average guess (/ x guess)))

(define (average x y)
  (/ (+ x y) 2))

(define (square x) (* x x))

(define (good-enough? guess last-guess)
  (< (abs (- guess last-guess)) 0.001))

(define (sqrt x)
  (sqrt-iter 1.0 x))
~~~
[Try in Repl.it](https://repl.it/@TanmayGujar/SICP-EX-17){: .external target="_blank"}

### Exercise 1.8

~~~scheme
(define (cube-iter guess x)
  (if (good-enough? (improve guess x) guess)
      guess
      (cube-iter (improve guess x) x)))

(define (improve guess x)
  (/ (+ (/ x (square guess)) (* 2 guess)) 3))


(define (square x) (* x x))

(define (good-enough? guess last-guess)
  (< (abs (- guess last-guess)) 0.001))

(define (cubert x)
  (cube-iter 1.0 x))
~~~
[Try in Repl.it](https://repl.it/@TanmayGujar/SICP-EX-18){: .external target="_blank"}