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