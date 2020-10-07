---
title: The Elements of Programming
book: sicp
chapter: Building Abstractions with Procedures 
chapter-num: 1
section: 1.1.0
---

Every powerful language has three mechanisms to form complex ideas from simple ones

- Primitive expressions, which are the simplest enitites. E.g ints, strings, booleans, operators like +, - etc.
- Means of combination, by which compound elements are built. 
- Means of abstraction, by which compound elements can be named and manipulated as units. E.g complex functions or complex procedures.

## Expressions

Expressions can be as simple as just typing in a number. The scheme interpreter responds with printing the result of evaluating the expression. 

The interpreter always operates in the same basic cycle:
1. Read an expression from the terminal.
2. Evaluate the expression.
3. Print the result

**Note: It is not necessary to explicitly instruct the interpreter to print the value of the expression.**

~~~ scheme
486
=> 486

(+ 2.7 10)
=> 12.7
~~~

The last expression above, which is formed by delimiting a list of expressions within parentheses is called a combination. The leftmost expression is the operator, and the other elements are operands. In this way, Scheme denotes procedure application. This covention of placing the operand first is called the **Prefix notation**. It has been opted since it avoids any ambiguity in deciding which part is the operator and which is the operand.

Combinations can be nested, like so:
~~~ scheme
(+ (* 3 5) (- 10 6))
=> 19
~~~
and there is no limit on the depth of nesting.

## Naming the environment

Names identify a variable whose value is the object.

In Scheme, names can be assigned as:
~~~ scheme
(define pi 3.14)

pi
=> 3.14
~~~
This causes the interpreter to associate 3.14 with the name pi.

**define** is the simplest means of abstraction in Scheme.

This possibility of associating names with values and later retrieving them means that the interpreter must maintain some sort of memory to keep track of them. This memory is called the **environment**.

## Evaluating combinations

The interpreter follows a procedure while evalating expressions. This can be written as:

1. Evaluate the subexpressions of the combination.
2. Apply the operator i.e the value of the leftmost expression to the operands i.e other subexpressions in the combination.

Since, combinations can have a nested structure, which means that operands in an combination can themselves be combinations, by looking at the above procedure we can say that it is recursive in nature. 

Recursion is a powerful technique for dealing with hierarchical, treelike objects. The evaluation procedure described above is an example of a general kind of process called **tree accumulation**.

**Note: (define x 3) does not follow the above evaluation rule. Exceptions to evaluation rule are called special forms.**

## Compound procedures

Compound procedures can be created using the following syntax:

~~~ scheme
(define (<name> <formal parameters>)
    <body>
)
~~~

Here, **name** defines the name of the procedure. **formal parameters** are the names which are used to describe arguments in the body of the procedure. And the **body** is an expression whose value is returned after replacing the formal arguments with actual arguments.

~~~ scheme
(define (square x) (* x x))
; calculates square of a number
~~~

## Substitution model in short

To apply a compound procedure to arguments, evaluate the body of the procedure with each formal parameter replaced by the corresponding argument.

## Applicative order vs Normal order evaluation

In applicative order evaluation, the interpreter first evaluates the operands and the operator and applies the value of the operands to the value of the operator.

~~~ 
-> (sum-of-squares (+ 5 1) (+ 3 4))
-> (+ (square 6) (square 7))
-> (+ (* 6 6) (* 7 7))
-> (+ 36 49)
-> 85
~~~

In case of normal order evaluation, the interpreter will substitute operand expressions for parameters until it has obtained an expression purely in terms of primitive operations. It will then perform the evaluation.

~~~ 
-> (sum-of-squares (+ 5 1) (+ 3 4))
-> (+ (square (+ 5 1)) (square (+ 3 4)))
-> (+ (* (+ 5 1) (+ 5 1)) (* (+ 3 4) (+ 3 4)))
-> (+ (* 6 6 ) (* 7 7))
-> (+ 36 49)
-> 85
~~~

Notice that normal order evaluation calculates the same thing more than once, and hence, the Scheme interpreter uses applicative order evaluation.

## Conditionals and Predicates

When there are multiple clauses you can use:

~~~
(cond (<p1> <e1>)
      (<p2> <e2>)
      (<p3> <e3>)
      (else <e4>))
~~~

The **else clause** is not compulsary. If none of the predictes \<p1>, \<p2> ... are true, then the value of the conditional is the value of the expresssion in the else clause, or it is undefined if the else clause is not present.

In case there are only two possible clauses, you can use the **if** special form.

~~~
(if (<predicate>) (<consequent>) (<alternative>))
~~~

The statement is evaluated is follows:
If \<predicate> evaluates to true, then the interpreter evaluates the \<consequent> expression, else, it evaluates the \<alternative> expression.

Both **if** and **cond** are special forms, in the sense that expressions are evaluated (or not) based on the value of the predicate. i.e. all operands are not evaluated.

## Logical operators

Complex predicates can be formed by using **and**, **or** and **not** operators.


~~~
(and <e1> <e2> ... <en>)
~~~
**and** returns false if any of the \<en> evaluates to false. If all are true then the value of the expression is the value of the last \<en> (This is so because, any value that is not #f or false is automatically considered as true by the interpreter).


~~~
(or <e1> <e2> ... <en>)
~~~
**or** returns false if any of the \<en> evaluates to false. If all are true then the value of the expression is the value of the last \<en> (This is so because, any value that is not #f or false is automatically considered as true by the interpreter).

~~~
(not <e1>)
~~~
**not** negates the value of the expression. 

**and** and **or** are sprecial forms, as all operands are not evaluated. **not** is an ordinary procedure.


## Procedures as black box abstractions

Any process can be defined by breaking down the problem of computing it into different procedures. However, it is important to understand that each procedure created must accomplish a single task that can be used as a module in defining other procedures. A procedure definition should be able to suppress detail of its implementation. That is, the user of the procedure need not know how the procedure is implemented in order to use it.

### Local names

It is also important that the choice of the name used for defining the formal parameters do not concern the user of the procedure.

To do this, the interpreter binds the names of the formal parameters to the values supplied by the actual parameters for all set of expressions in the procedure(i.e body of the procedure), called scope of the name. 

A procedure definition binds the formal parameters and produce bound variables. The variables which are not bound are called free. A procedure may depend on both bound and free variables. 

The result of procedure application does not depend on the names of the bound variables, however, it does depend on the names of the free variables and subsquently depend on the values of the free variables.

In short, free variables are those which may be defined outside the procedure scope, and bound variables are those whose value is bound at the time of procedure application.

### Internal definitions ans block structure

We can even define procedures inside of other procedures. This is used so that the auxiliary procedured that we define, do not clutter up the global scope

Such nesting of definitions is called block structure.

~~~ scheme
(define (average x y)
  (/ (+ x y) 2))

(define (square x) (* x x))

(define (sqrt x)
  (define (sqrt-iter guess)
    (if (good-enough? (improve guess) guess)
        guess
        (sqrt-iter (improve guess))))

  (define (improve guess)
    (average guess (/ x guess)))

  (define (good-enough? guess last-guess)
    (< (abs (- guess last-guess)) 0.001))

  (sqrt-iter 1.0 x))
~~~

Notice that **x** can be used in the internal definitions of the auxiliary procedure without passing it as a parameter to the procedure. **x** behaves like a free variable for the auxiliary functions, and it gets it value from the argument with which **sqrt** is called. This is called as **lexical scoping**.


