# Exotic Programming Ideas: Part 3 (Effect Systems)

markdownload-timestamp:: 2023-04-14T09:46:44 (UTC -05:00)
markdownload-source:: https://www.stephendiehl.com/posts/exotic03.html
markdownload-hostname:: www.stephendiehl.com
author:: Stephen Diehl
tags:: 



## Excerpt
> Personal Blog

---
### Exotic Programming Ideas: Part 3 (Effect Systems)

Continuing on in our series on exotic programming ideas, we’re going to explore the topic of _effects_. Weak forms of effect tagging are found in many mainstream programming languages, however the use of programming with whole _effect systems_ that define syntax for defining and marking regions of effects in the surface syntax is still an open area in language design.

-   Week 1 - [Module Systems](https://www.stephendiehl.com/posts/exotic01.html)
-   Week 2 - [Term Rewriting](https://www.stephendiehl.com/posts/exotic02.html)
-   Week 3 - [Effect Systems](https://www.stephendiehl.com/posts/exotic03.html)
-   Week 4 - [Datalog](https://www.stephendiehl.com/posts/exotic04.html)

First, we should define what we mean by an effect. We’ll adopt the definitions commonly used in functional programming discipline, simply because it has a precise definition wheres colloquial usage often does not. First a _pure function_ is a unit of code whose return value is entirely determined by its inputs, and has no observable _effect_ other than simply returning a value. A pure function is a function in programming which behaves a like a function in mathematics. For example in Python code we denote a “function” called `f`:

In the pseudocode traditionally known as mathematics we denote the function $f$:

$$
f(x) = x^2
$$

There are a few subtle points to mention in this definition. The first is the matter of perspective in our analysis of effects. This Python function compiles into a sequence of bytecodes which manipulate stack variables, mallocs megabytes of data on the heap, invokes the garbage collector, and swaps hundreds of thousands of values in and out of registers on the CPU all corresponding to doing the exponentiation of an arbitrary size integer inside a PyObject struct. All this is very effectful from the perspective of the underlying implementation, however we can’t _observe_ the functioning of these internals from within the normal language.

The big idea in pure functional programming is that programming will inevitably consist of both pure and effectful (sometimes called _impure_) logic. Additionally we suppose it is a useful property of the surface language to be able to distinguish between units of logic which have effects, and to be able to classify these type of effects in order to greater reason about program composition.

The alternative to this is the model found in most languages where all logic is mushed upto in a big soup of effects and relies on the programmer’s intuition and internal mental model to distinguish between which code can do perform effects (memory allocations, side channels, etc) and logic which cannot. The research into effect systems is fundamentally about canonising our intuition about correct program effect synthesis into a formal model that compilers can reason about on our behalf and interact with developer tools in an ergonomic way.

Functional Languages like Idris, Haskell, F\* and a few other research languages have been exploring the design space for the better part of the last decade. Concepts such as monads saw initial exploration for demarcating pure and impure logic but have fallen off in recent years as that model has hit a wall in terms of usefulness. The most common area of active exploration is one known as _algebraic effect handlers_ which admits a tractable inference algorithm for checking effectful logic while not introducing any runtime overhead.

There are no mainstream languages which use this model, however there is a academic language out of Microsoft Research lab called [Koka](https://www.google.com/search?q=koka+microsoft+research&oq=koka+microsoft+research&aqs=chrome..69i57j69i60l2.2695j0j4&sourceid=chrome&ie=UTF-8) which presents the most developed implementations of these ideas. As far as I can tell no one uses this language for anything, however it is downloadable and quite usable to explore these ideas. We will write all of our example code in Koka.

In Koka the absence of any effect is denoted by the effect `total`. The only result of computing the function `f` is simply returning the square of its input.

```
fun f( x : int ) : total int
{
  return pow(x,2)
}
```

However we can write a effectful function, such as one that reads from the screen, by tagging it with a `console` effect. The body of this function can then invoke functions such `println` and the result of the invocation of thees functions is captured in the signature of the function that invokes them. The return type `()` denotes the unit type which is called `void` in C-like languages.

```
fun main() : console ()
{
  println("I can write to the screen!");
}
```

It is worth noting that the `println` function provided by the standard library has the following type signature which itself includes the effect.

```
fun println( s : string ) : console ()
```

And as such the compiler is aware of the effect it carries and the following function can be written without an annotation and _effect inference_ will deduce the appropriate signature without the user having to specify it. The return type can also be inferred using the usual type inference techniques.

```
fun main()
{
  println("I can write to the screen!");
}
```

#### Exceptions

Besides input/output, the most common type of effect found in most programming is the ability to fail. Usually langauge runtimes will implement this functionality using some exceptions which perform a non-local jump to logic which handles the exception or unwinds the call stack and aborts. This is clearly an effect that we can model and we can create an interface similar to _checked exceptions_ found in other languages.

The `throw` function will take an error sum type and result in a effect marked by `exn`. While the `try` function will consume a function which results in `exn` type and return an error. The `error` type is either an `Error` or a `Ok` on successful execution.

```
type error<a> {
  Error( exception : exception )
  Ok( result : a )
}
```

The error handling functions can be written as higher order functions that consume and handle functions taged with the `exn` effect.

```
fun throw( err : error<a> ) : exn a
fun try( action : () -> <exn|e> a ) : e error<a>
fun maybe( t : error<a> ) : maybe<a>
```

For example we can do classic case of handling division of mero and wrapping up the arithmetic error in a `maybe` sum type which handles the zero case with a `nothing`.

```
fun divide(a : int, b : int) : exn int {
  match(b) {
    0 -> throw("Division by zero");
    _ -> return (a / b);
  }
}

fun safeDiv(a : int, b : int) : maybe<int>
{
  maybe( try { divide(a,b) } );
}
```

Elaboration of pattern matching inside the compiler can deduce incomplete patterns and infer that an exception should be added to the type of the pattern match that can fail at runtime.

```
fun unjust( m : maybe<a> ) : exn a {
  match(m) {
    Just(x) -> x
  }
}
```

Whereas a complete pattern match is deduced as total.

```
fun unmaybe( m : maybe<a>, default : a ) : total a {
  match(m) {
    Just(x) -> x
    Nothing -> default
  }
}
```

#### Non-termination is an Effect

By our above definition about effects, the only observable result of invoking a function is return a resulting value. Therefore functions which do not compute a value, and run infintely are not functions and have a side-effect called _divergence_. Deducing whether a given function is total is non-trivial in the general case, however a function which is the composition of units of logic which are all independently total must itself be total.

There are many simple cases where we can immedietely deduce non-totality from simply analysising call-sites. For example the following function is automatically tagged with the `div` effect since it recurses on itself.

```
fun forever(f) {
  f();
  forever(f);
}
```

The `forever` combinator has the inferred type:

```
forever: forall<a,b,e> (() -> <div|e> a) -> <div|e> b
```

The effect checker can deduce totality across mutually recusive definitions, so functions that invoke each other must themselves either be entirely total or possibly diverge on composition.

```
fun f1() : div int
{
  return 1 + f2();
}

fun f2() : div int
{
  return 2 + f1();
}
```

#### Rows of effects

While tagging individual effects independently is useful in its own right, programing in the large requires us to compose logic together and thus we need a way to synthesize the combination of effects. In Koka this is represented as a `row` of effects. This is denoted with the bracket syntax:

In the language of mathematics, effect rows are commutative monoids with an operation _extension_ denoted by the pipe and a neutral element (`total` or `<>`) representing the absence of effects. The commutative and associativity of the extension operation allows for a canonical ordering of effects in signatures.

```
<e1> | e2     = <e1,e2>
<e1,e2> | e3  = <e1,e2,e3>
<e1> | <>     = <e1>
<e1> | e1     = <e1>  
<e2, e1>      = <e1,e2>  
```

For example we can write down a function which invokes a random number generator with the non-determinism effect `ndet` as well as raising an exception with the effect `exn`. The synthesis of the two is now the row `<ndet, exn>`.

```
fun multiEffect() : <ndet, exn> ()
{
  val n = srandom-int()
  throw("I raise an exception");
}
```

The effect system denotes functions which may diverge or throw exceptions as `pure` with the following alias.

In the Haskell approach to effects there is a single opaque IO monad which inhabits any action which can perform any type of console input, output or system operation. However languages which richer effect systems can model the IO hierarchy in much more granularity. For example Koka defines the following three tiers of IO effects in increasing expressivity.

```
// Functions that perform arbitrary IO operations
// but are terminating without raising exceptions
alias io-total = <ndet,console,net,file,ui,st<global>>

// Functions that perform arbitrary IO operations
// but raise no exceptions
alias io-noexn = <div,io-total>

// Functions that perform arbitrary IO operations.
alias io = <exn,io-noexn>  
```

#### Reference Lifetimes & Boundaries

State is an essential part of programing, and it is one which is inherently effectful. Importantly we’d like to be talk about which regions of memory or logic we are able to write to within a given scope. For this we need to be able to refer to effects over a given region of memory as a paramter to the effect. The language allows us to parameterise effects with a _heap parameter_ using bracket notation. There are three core stateful effects provided by the standard library:

1.  `alloc<h>` - The alloc effect for allocating references over heap parameter `h`.
2.  `read<h>` - The read effect from a reference from a heap parameter `h`.
3.  `write<h>` - The write effect for writing to a reference on heap parameter `h`.

To create and manipulate references there are three core functions:

```
fun ref( value : a ) : (alloc<h>) ref<h,a>
fun set( ref : ref<h,a>, assigned : a ) : (write<h>) ()
fun (!)( ref : ref<h,a> ) : <read<h>|e> a
```

Functions which no observable leakage of internal references can be marked as total if the types of references are not referenced in either the argument types or the return type. Thus _local state_ can be embedded inside pure functions. For instance the following function is total even though internally it uses mutable references:

```
fun localState() : total int
{
  val z = ref(0);
  set(z, 10);
  set(z, 20);
  return (!z);
}
```

The compiler itself also has a level of syntactic sugar for working with references. The `val` introduces an immutable named variable, however the `var` syntax can be used to define a mutable reference concisely.

```
val z = ref(0)     
var z : int = 0    // Identical to above
```

Variables can be updated using `:=` operator.

```
set(z, 10) 
z := 10            // Identical to above
```

Thus we can write pseudo-imperative logic like the following counter function:

```
fun bottlesOfBeer() {
  var i := 0; 
  while { i >= 99 } {
    println(i)
    i := i + 1
  }
}
```

References can be passed as arguments to functions and heap parameters can be quantified as type variables, allowing us to write generic functions which operate over references of any type.

```
fun add-refs( a : ref<h,int>, b : ref<h,int> ) : st<h> int {
    a := 10
    b := 20
    (!a + !b)
}
```

The combination of the ability to read, write, and allocate is given the name `st` in the standard library to denote stateful computations.

```
alias st<h> = <read<h>,write<h>,alloc<h>>
```

Using this kind of effect system for tracking references gives us a powerful abstraction for denoting regions of logic that have _read barriers_ and _write barriers_ and separating mutation from pure logic in a way that is machine checkable. Future languages that had this kind of information at compile-time could use it to inform compilation of regions of local mutation into more efficient efficient code with still maintaining guarantees about the boundaries of pure logic.

#### Effect polymorphism

Finally we’d also like to be able to write higher order functions which can take arguments which are either effectful or pure and incorporate their effects as part of the types of output. The common `map` function is a higher-order function which takes a list and applies a given function argument over each element of the list. To write this in a language with an effect system we need to be able to refer to the effect of the function argument as a type variable (`e` in this example) and use it the output type for `map`.

```
fun map( xs : list<a>, f : (a) -> e b ) : e list<b>
```

In the case where we apply a total arithmetic function over the list, we simply get back a list of ints. While in the IO case we can apply a function like `println` which will individually print each integer out to the console, and resulting type is a list of units. Both use cases are subsumed by the same function using this _parametric polymorphism_ over the abstract effect type variables.

```
val e1 = map([1,2,3,4], println);   // console list<()>
val e2 = map([1,2,3,4], dbl);       // list<int>

fun dbl(x : int) : total int
{
  return (x+x)
}
```

It is still early days for effect system research. The key takeaway that I would like to push for future work is the observation that languages which aim to improve the ergonomics and performance of effect modeling cannot simply push the entire system into a library. There needs to be language-level support for both integrated effect types and annotations in the surface language for labeling subexpressions and giving hints to type inference. A lot of approaches to do this in Haskell, Scala, etc are inevitably doomed to poor ergonomics by this simple fact.

There is a whole new level of static analysis tools that can be built by not just inferring types, but providing a whole new level of static information on top of our code that is otherwise tossed out by the compiler. This is a very exciting technique and I hope it bears more fruit in the coming decade.

#### External References

1.  [F\* Reference](https://www.fstar-lang.org/tutorial/)
2.  [Eff Programming Langauge](https://www.eff-lang.org/)
3.  [Koka Language Specification](https://koka-lang.github.io/koka/doc/kokaspec.html#sec-koka-language-specification)
4.  [Algebraic Effects for Functional Programming - Daan Leijen](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/08/algeff-tr-2016-v3.pdf)
