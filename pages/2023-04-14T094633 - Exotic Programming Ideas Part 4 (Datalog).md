# Exotic Programming Ideas: Part 4 (Datalog)

markdownload-timestamp:: 2023-04-14T09:46:32 (UTC -05:00)
markdownload-source:: https://www.stephendiehl.com/posts/exotic04.html
markdownload-hostname:: www.stephendiehl.com
author:: Stephen Diehl
tags:: 



## Excerpt
> Personal Blog

---
### Exotic Programming Ideas: Part 4 (Datalog)

Continuing on in our series on exotic programming ideas, we’re going to explore the topic of logic programming and a particular form known as _datalog_.

-   Week 1 - [Module Systems](https://www.stephendiehl.com/posts/exotic01.html)
-   Week 2 - [Term Rewriting](https://www.stephendiehl.com/posts/exotic02.html)
-   Week 3 - [Effect Systems](https://www.stephendiehl.com/posts/exotic03.html)
-   Week 4 - [Datalog](https://www.stephendiehl.com/posts/exotic04.html)

Datalog is a very simple formalism, it consists of only two things:

1.  A database of facts
2.  A set of rules for deriving new facts from existing facts

Datalog is executed by a query processor that given these two inputs, finds all instance of facts implied by both the databased and rules. For our examples we’re going to be coding our examples in the Souffle language. The namesake of the language is an acronym for the _Systematic, Ontological, Undiscovered Fact Finding Logic Engine_. It can be installed simply on many Linux systems with the command:

```
$ apt-get install souffle
```

Souffle is a minimalist datalog system designed for complex queries over large data sets, such as those encountered in the context of doing static program analysis over large codebases. Souffle is implemented in C++ and can compile datalog programs into standalone executables via compilation to C. It is one of the simpler and most efficient implementations of these systems to date, and notably it has a simple type system on top of the logic engine.

Datalog encodes a decidable fragment of logic for which satisfaction is directly computable over a finite universe of terms. These terms consist of three classes of expressions. A _fact_ represented syntactically by a expression suffixed by a period.

A _rule_ represented by a head and body term with a _turnstile_ ($\vdash$) symbol between.

And _query_ terms which query the truth value of fact within the database.

An example of these terms would be “classical” categorical syllogism about the Greek philosopher Socrates. Socrates is a man, all men are mortal, therefore we can deduce that Socrates is mortal.

```
// All men are mortal / valar morghulis
mortal(X) :- human(X).

// Socrates is a man.
human("Socrates").

// Is Socrates mortal?
?- mortal("Socrates").
```

Execution of a Souffle program is in effect a set of transformations from input facts to deduced output facts by a prescription of rules.

![][fig1]

Souffle can read input facts from a variety of sources including stdin, CSV files, and SQLite databases. he default input source is a tab-separated file for a relation where each row in the file represents a fact in the relation. This is specified with a IO _input declaration_ of the form.

```
.decl A(n: symbol)
.input A
```

This would read from a file `A.facts` of tab separated values for a symbolic value `n`. However we can modify our input by passing explicit IO parameters to it instead.

```
.input A(IO=file, filename="fact_database.csv",  delimiter=",")
.input A(IO=stdin, delimiter=",")
.input A(IO=sqlite, dbname="fact_database.db")
```

#### Simple Example

A simple end to end example would be the program that associates a set of symbolic inputs with a set of numerical ones by a simple one-to-one mapping. We could write the simple `A` to `B` transformation as the following rules.

```
/* Input from A.facts */
.decl A(n: symbol)
.input A

/* Output to B.csv */
.decl B(n: number)
B(0) :- A("Hello").
B(1) :- A("World").
.output B
```

This reads from the input facts in `A.facts` and outputs `B.csv` which not surprisingly looks like the following when we invoke Souffle with the following command.

The output matches `A` to `B` according to the rules in `hello.dl`.

| A | B |
| --- | --- |
| Hello | 0 |
| World | 1 |

#### Relations

Logic inside of datalog is specified by relations, which are ordered tuples of typed variables that relate quantities. For example if we had a set of named entities (represented as symbols) we can assign a relation that maps a property to each of them as facts.

```
.decl Human(x : symbol)
.decl Klingon(x : symbol)
.decl Android(x : symbol)

Human("Picard").
Human("Riker").
Klingon("Worf").
Android("Data").
```

Each variable is given a specific type indicated by `var : type` where type is one of the following builtins.

1.  `symbol` - Symbolic quantities represented as strings.
2.  `number` - Signed integers
3.  `unsigned` - Unsigned integers
4.  `float` - Floating point values

Relations can take multiple arguments that assign properties to a collection of symbols according to rules over that relation.

```
.decl Rank(x:symbol, y:symbol)
.decl Peers(x:symbol, y:symbol)

Rank("Captain", "Picard").
Rank("Officer", "Riker").
Rank("Officer", "Worf").
Rank("Officer", "Data").
```

Rules over these relations are defined by _clauses_. The following can be read as a pair (x,y) is in A relation, if it is in B relation.

Clauses in Souffle are known in logic as _Horn clauses_. They are comprised of subterms joined together with a conjunction ($\land$) (logical AND in programming) that implies a statement $u$. Can read the following statement in math as

> if a and b and … and z all hold, then also u holds

$$
u \leftarrow a \land b \land \ldots \land z
$$

The above is known as the _implicative form_. Logically this is equivalent to the statement written in terms of disjunction ($\lor$) (logical OR in programming) and negation ($\lnot$) (logical NOT in programming).

$$
\lnot a \lor \lnot b \lor \ldots \lor z
$$

In Datalog notation we write this Horn clause in the following syntax.

In clauses, free variables that occur inside of the conjoined expressions are implicitly universally quantified, i.e. they are required to hold forall (denoted $\forall$) substitutions of the variable in a domain. The domain in this case is the set of inhabitants of a given type (i.e. symbol, unsigned, etc).

So the datalog expression above

Is equivalent to the Horn clause.

$$
\forall x. \forall y. \lnot B(x,y) \lor A(x,y)
$$

So for example we know that Klingons and Humans both eat food, but obviously androids do not. We write the following rules over a variable term `X` of type symbol.

```
.decl Eats(x:symbol)

Eats(X) :- Human(X).
Eats(X) :- Klingon(X).
.output Eats
```

Which produces the following output from the given set of input facts about the symbols in scope given the constraint on their species.

| Eats |
| --- |
| Riker |
| Picard |
| Worf |

Terms can also be negated, for instance if we wanted to enumerate all officers that eat could write the following rule which excludes androids.

```
InvitedToDinner(X) :- Rank("Officer", X), !Android(X).
```

| InvitedToDinner |
| --- |
| Riker |
| Worf |

A more complex rule can consist of multiple terms separated by command, all of which have to evaluate true. For example two officers are peers if they have same rank and are not the same person. We use an intermediate variable `Z` to test the equality of rank in this relation.

```
.decl Peers(x:symbol, y:symbol)
Peers(X, Y) :- Rank(Z, X), Rank(Z, Y),  X != Y.
```

| Peers |  |
| --- | --- |
| Riker | Worf |
| Riker | Data |
| Worf | Riker |
| Worf | Data |
| Data | Riker |
| Data | Worf |

Rules can also be recursive. For example we we wanted to write a query which found all first degree friend relations of a given set of facts we could write the following recursive definition.

```
.decl Friend(x:symbol, y:symbol)
.decl FriendOfFriend(x:symbol, y:symbol)

FriendOfFriend(X, Y) :- Friend(X, Z), FriendOfFriend(Z, Y).
```

An important class of relations are that of _equivalence relations_. These are relations between two terms that satisfy an additional set of constraints: _reflexivity_, _symmetry_, and _transitivity_. Relations of this form behave similar to how the equality symbol is treated in algebra.

$$
\begin{align*}
a &= a \\
a &= b \Rightarrow b = a \\
a &= c, b = c \Rightarrow a = c
\end{align*}
$$

```
// every element of R1 is equivalent to every element of R2
.decl equivalence(x:number, y:number)
equivalence(a, b) :- R(a), R(b).

// reflexivity
equivalence(a, a) :- equivalence(a, _).

// symmetry
equivalence(a, b) :- equivalence(b, a).

 // transitivity
equivalence(a, c) :- equivalence(a, b), equivalence(b, c).
```

In the implementation of the relation, the data and intermediary deductions are stored in a common B-tree data structure. However this can be modified by passing an explicit argument to force the use of a disjoint-set or trie data structure for specific uses cases.

```
.decl A(x : number, y : number) eqrel // Disjoint-set
.decl B(x : number, y : symbol) btree // B-tree
.decl C(x : number, y : symbol) brie  // Dense trie
```

When working with integral types (`number`, `unsigned` and `float`) arithmetic expressions behave as expected and can be pattern matched on. For example we can write the Fibonacci function as a relation in terms of itself.

```
fib(0, 0).
fib(1, 1).
fib(n+1, x+y) :- fib(n, x), fib(n-1, y), n < 15.
```

This generates the following output facts.

| fib |  |
| --- | --- |
| 0 | 0 |
| 1 | 1 |
| 2 | 1 |
| 3 | 2 |
| 4 | 3 |
| 5 | 5 |
| 6 | 8 |
| 7 | 13 |
| 8 | 21 |
| 9 | 34 |
| 10 | 55 |
| 11 | 89 |
| 12 | 14 |
| 13 | 233 |
| 14 | 377 |
| 15 | 610 |

#### Type System

Souffle has a limited type system that can be used to track the types of variables quantified in clause expressions. For example one cannot have a variable which simultaneously holds a symbol and unsigned integer. We can however define custom _type synonyms_ on top of the ground types of the languages that will let us semantically distinguish different user-defined types of symbols and numeric quantities. For example:

```
.type Person <: symbol
.type Food <: symbol

.decl Eats(x : Person, y : Food)
.decl Dinner(x : Person, y : Food)

Dinner(x, y) :- Eats(x, y). // Well-typed
Dinner(x, y) :- Eats(y, x). // Not well-typed, Person != Food
```

Unions of type synonyms are also permitted so long as the underlying types of the synonyms are equivalent.

```
.type A <: number
.type B <: number
.type C = A | B    // Either in A or B
```

The language also has the ability to define both _sum_ and _product_ types which let us both options and records as terms in our clauses.

```
.type Sum = A { x: number } | B { x: symbol }
.type Prod = C { x: number, y : symbol }
```

For example we could build a linked list (or cons list) out of a product type which has both a head element and a tail element.

```
// Linked list
.type List = [ head : number, tail : List ]
.decl MyList(x : List)

MyList( [1, [2, nil]] ).
```

Logic programming is a very natural way of expressing abstract program semantics and with tools like Souffle we have excellent logic engine that can be embedded in other static analysis applications to collect facts about code and deduce implications about code composition and usage. This a powerful technique and future static analysis tools are going to yield whole new ways of reasoning about and synthesising code in the future.

#### External References

1.  [Souffle Language Reference](https://souffle-lang.github.io/docs.html)
2.  [SWI-Prolog Online](https://swish.swi-prolog.org/)
3.  [μZ in Z3](https://rise4fun.com/Z3/tutorialcontent/fixedpoints#h21)
4.  [Tool Talk: Souffle](https://www.youtube.com/watch?v=Qp3zfM-JSx8)
5.  [Domain Modeling with Datalog](https://www.youtube.com/watch?v=oo-7mN9WXTw)

[fig1]: data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAisAAABqCAYAAAB9NRHIAAAEcHRFWHRteGZpbGUAJTNDbXhmaWxlJTIwaG9zdCUzRCUyMmFwcC5kaWFncmFtcy5uZXQlMjIlMjBtb2RpZmllZCUzRCUyMjIwMjAtMTItMDVUMTElM0EyNyUzQTE4LjUyNFolMjIlMjBhZ2VudCUzRCUyMjUuMCUyMChYMTElM0IlMjBMaW51eCUyMHg4Nl82NCklMjBBcHBsZVdlYktpdCUyRjUzNy4zNiUyMChLSFRNTCUyQyUyMGxpa2UlMjBHZWNrbyklMjBDaHJvbWUlMkY4Ni4wLjQyNDAuNzUlMjBTYWZhcmklMkY1MzcuMzYlMjIlMjBldGFnJTNEJTIydmpsQjVWQmpkTkRCLWtpOTNNTEglMjIlMjB2ZXJzaW9uJTNEJTIyMTMuMTAuOSUyMiUyMHR5cGUlM0QlMjJkZXZpY2UlMjIlM0UlM0NkaWFncmFtJTIwaWQlM0QlMjJMQWd3cGlBMmJFYVZlc216aVc4YSUyMiUyMG5hbWUlM0QlMjJQYWdlLTElMjIlM0V6VlpSYjVzd0VQNDFQRTRLR0VMNnVHWE4xa2xUcWtiVHRrY1hYOEdTc1pFeEpkbXZuNmtQaUtGUnUwanQ4b1R2OCUyRm51JTJGTjNIUVVEVzVmNkxwbFh4WFRFUVFiUmclMkI0QjhEcUlvVENOaUh4MXlRQ1JNWW9ma21qUEVSbURIJTJGd0NDQzBRYnpxRDJISTFTd3ZES0J6TWxKV1RHdzZqV3F2WGRIcFR3czFZMGh4bXd5NmlZb3o4NU00VkRWMUU2NGwlMkJCNTBXZk9WeGV1WjJTOXM1NGs3cWdUTFZIRUxrT3lGb3JaZHlxM0s5QmRPejF2TGh6bXhPN1EyRWFwSG5OZ1lmMkclMkY5OXY3M1pVdGdzNDlXUFglMkZHZCUyRklCUkhxbG84TUkzc21xTWhUWTBNelZXYmc0OUhWbzFra0VYY1JHUVQyM0JEZXdxbW5XN3JWV0F4UXBUQ211RmRqbmN1RE13RVdnRCUyQjVNM0NBZGVyS0pBbFdEMHdicmdnUlV5aVZvYU5OS09qUWw3dG91anBpd1JvNmlGZklnODBtVVh5TmclMkZzRWRtN04wMUFpNk10U2hOTG95MmVFYmJ0akVYcTdvNHZU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Laylqer7UAs2tfD777/LuHHj5N5775WffvpJVl55ZTnooIPklFNOkVQq1StneqvzvmTrqX4jdjqd/keY1VdfXd5//33f8HOMu+mmm+Tggw+er9dqvsiSFga8WXnzzTddsX788cfnaVaOPPJI2X///VXWf6eddpKxY8fK+uuvrxJPM4glUmrOq7dYDQ0Nks/n5cQTT5QxY8ZJc3OjMyulX8VZfy6PEbJZOeKII+YwHO+8845sv/32ctVVV8nOO+/cI1Q0K1lpbc1Kc2O9pNN1EkUJSSaTC4umC+V9rGuhq6tLNthgA1lqqaXkggsukOWXX17efvttZ7bXXXddmThxYq849VbnfQHuqX7HZuXrr79WeRDFg9OSSy4pP/74o29q/TbO0n0hOLMyfvx4QZHt6OiQzz//XEDi++67T5ZbbjlpaWlxzhvG47vvvhMYjRNOOEFeeukl58A/+ugjt6jxz6+99prA/f75558ybNgwefrpp+dYdHRWejIrIBRucg8++KDLYYsttpDrr79eoihybvqwww6Tb775RlZYYQW5+eab5a677pJzzz1XFl98cbn44oudwA488ED59ttv3XtifF9PuP3GyJkdhfFXTZJkKpJh7UOlra22CzTWD1wC5jAgEOWoUWNk7NgTpbG5UVKSkmQSN6VStyXuuNSSWQGfyruW0M/HH3/sCiWu+Gd0Ess7K+DxLbfc4sZsueWWcvnll0tdXZ1ccsklcs0117i/x00FY1ZcccX+pG2/xLZUoPtlgnMFDUELkyZNclpFdw9ci68vvvhC1l57bXnllVfkjz/+8KrzF110kau9W2+9tbz66quCjs2ECRNk0003dUboyy+/nMXj+GeYo/L6vdtuu83KoS+zgvfAfQT51dfXu4eDzTbbzL0eXSI8wE6fPt29P+4he+21lzzwwAPu3oR7GTr/1dKVJS0EZ1aw0Oecc45z1SiqI0eOlEUWWcQRCcX1uOOOc//+/fffu62j9957zxmXeZkVmJd77rnHEaTSzspDDz0kp556qrz++uvuRjZ8+HAnJhBt6NChcuaZZ8quu+4qEMYzzzzj4q+xxhpyww03uM7KqFGjXEHH1hbM0qGHHur+Da35alw+pIxv2Mgv3japRq5a71koiAwatJgrVrjQysUcR44cLePGnSjZlhaJEklJpZKzOi2YN1rOoc0fLey5OyvQ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
