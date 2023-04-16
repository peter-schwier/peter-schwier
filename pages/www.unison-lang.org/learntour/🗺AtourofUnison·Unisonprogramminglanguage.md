---
title: Learn Unison | 🗺 A tour of Unison · Unison programming language
pageTitle: 🗺 A tour of Unison · Unison programming language
length: 23848
author: 
timestamp: 2023-04-16T15:03:54-05:00
markdownload-tags: []
markdownload-source: https://www.unison-lang.org/learn/tour/
markdownload-hostname: www.unison-lang.org
---

# 🗺 A tour of Unison · Unison programming language

## Excerpt
> A friendly programming language from the future.

---
This document walks through the basics of using the Unison codebase manager and writing Unison code. We will introduce bits and pieces of the core Unison language and its syntax as we go. The Unison language documentation is a more in-depth resource if you have questions or want to learn more.

If you want to follow along with this document (highly recommended), this guide assumes you've already gone through the steps in[the quickstart guide](https://www.unison-lang.org/learn/quickstart)and skimmed through[the big idea](https://www.unison-lang.org/learn/the-big-idea).

## 👋 to the Unison codebase manager

The Unison Codebase Manager, or UCM for short, is the command line tool that runs the Unison programming language and allows you to interact with the Unison code you've written and saved. Put differently, the UCM is the interface to your Unison codebase.

💡 Remember: Unison code is not saved as text-based file content. Because of this, we need a tool that lets us change and run Unison programs.

Its many responsibilities include:

-   Typechecking and compiling new code
-   Organizing, navigating, and finding Unison definitions
-   Storing the state of your codebase
-   Running Unison programs and Unison binaries
-   Publishing and pulling Unison libraries

## 🎉 Running the UCM

By default, running`ucm`in a directory will interact with any`.u`suffixed file in the directory where the command was issued while opening the default codebase in your home directory. You'll get a message in the UCM like:

```
Now starting the Unison Codebase Manager (UCM)...

[…]

🐣 Since this is a fresh codebase, let me download the base library for you.
```

What's happening here? This is the Unison Codebase Manager starting up and initializing a fresh codebase with the standard library. We're used to thinking about our codebase as a bag of text files that's mutated as we make changes to our code, but in Unison the codebase is represented as a collection of serialized syntax trees, identified by a hash of their content and stored in a collection of files inside of a`.unison`directory in the path you supplied to the ucm.

The Unison codebase format has a few key properties:

-   It isappend-only:once a file in the`.unison`directory is created, it is never modified or deleted, and files are always named uniquely and deterministically based on their content.
-   As a result, a Unison codebase can be versioned and synchronized with Git or any similar tool and will never generate a conflict in those tools.

Let's explore the`base`library that was just downloaded and get used to navigating a Unison codebase.

You can view the terms and types in a namespace with the[`ls`](https://www.unison-lang.org/learn/ucm-commands/ls)ucm command.

The output should be a numbered list of definitions and their associated signatures.

```
.> ls base.data.List

1.   ++                    ([a] -> [a] -> [a])
2.   +:                    (a -> [a] -> [a])
3.   :+                    ([a] -> a -> [a])
4.   Nonempty              (type)
[…]
```

📁

Here the command is performed in the top-level namespace, represented by`.`It's common for the top-level of your codebase to house multiple Unison namespaces representing the various projects you're working on. The prompt shows us which namespace we are currently in. If we were in a different namespace, we would need to change the[`ls`](https://www.unison-lang.org/learn/ucm-commands/ls)command from using therelative path`base`to theabsolute path`.base`.

Because of the append-only nature of the codebase format, we can cache all sorts of interesting information about definitions in the codebase andnever have to worry about cache invalidation.For instance, Unison is a statically-typed language and we know the type of all definitions in the codebase—the codebase is always in a well-typed state. So one thing that's useful and easy to maintain is an index that lets us search for definitions in the codebase by their type. Try out the following commands (new syntax is explained below):

```
.> find : [a] -> [a]

  1. base.data.List.distinct : [a] -> [a]
  2. base.data.Heap.sort : [a] -> [a]
  3. base.data.List.dropLast : [a] -> [a]
  4. base.data.List.reverse : [a] -> [a]
  5. base.data.Heap.sortDescending : [a] -> [a]

.> view 4

  base.data.List.reverse : [a] -> [a]
  base.data.List.reverse as = List.foldLeft (acc a -> a +: acc) [] as
```

Here, we used the did a type-based, with`find`followed by a colon,`:`,to search for functions of type`[a] -> [a]`.We got a list of results, and then used the[`view`](https://www.unison-lang.org/learn/ucm-commands/view)command to look at the nicely formatted source code of one of these results. Let's introduce some Unison syntax:

-   `[List.reverse](https://share.unison-lang.org/@unison/p/code/latest/namespaces/public/;/terms/@pblsc3e1nibatbigr5lqtjvfucqa0ln2bdvpfa2gq5hfkn35nfnlsv274rljtbhr0mf014chvco3h2u8kbf1t8vpi8a31v84mfl507o) : [a] -> [a]`is the syntax for giving a type signature to a definition. We pronounce the`:`symbol as "has type", as in reverse has the type`[a] -> [a]`.
-   `[Nat]`is the syntax for the type consisting of lists of natural numbers (terms like`[0, 1, 2]`and`[3, 4, 5]`,and`[]`will have this type), and more generally`[Foo]`is the type of lists whose elements have some type`Foo`.
-   Any lowercase variable in a type signature is assumed to be[universally quantified](https://www.unison-lang.org/learn/language-reference/polymorphic-types),so`[a] -> [a]`really means and could be written`forall a . [a] -> [a]`,which is the type of functions that take a list whose elements are some (but any) type, and return a list of elements of that same type.
-   `List.reverse as`takes one parameter, called`as`.The stuff after the`=`is called thebodyof the function, and here it's a[block](https://www.unison-lang.org/learn/language-reference/blocks-and-statements),which is demarcated by whitespace.
-   `acc a -> ..`is the syntax for an anonymous function.
-   Function arguments are separated by spaces and function application binds tighter than any operator, so`f x y + g p q`parses as`(f x y) + (g p q)`.You can always use parentheses to control grouping more explicitly.

Try doing`view base.data.List.foldLeft`if you're curious to see how it's defined.

### 🖼 Check out the local codebase UI

We've been navigating the codebase via the UCM command line, but there's another option for exploring and viewing a Unison codebase: the Unison codebase UI.

The codebase UI is a graphical interface for exploring your codebase. You can search for terms, click-through to Unison code definitions, and read code documentation.

Type[`ui`](https://www.unison-lang.org/learn/ucm-commands/ui)in the UCM to open the local UI and search for`[Text.dropRightWhile](https://share.unison-lang.org/@unison/p/code/latest/namespaces/public/;/terms/@4mpdq11ep7rf66841m0sgd5ken3h9bnc0dn5hf11su5co52cl7k7sb0h6dn78ia38ltviqken3o42dnrfdtkcrp56uch92j2hdcguj8)`.Unison docs are automatically linked to the term and the source code is available for exploration.

### Names are stored separately from definitions so renaming is fast and 100% accurate

The Unison codebase, in its definition for`[List.reverse](https://share.unison-lang.org/@unison/p/code/latest/namespaces/public/;/terms/@pblsc3e1nibatbigr5lqtjvfucqa0ln2bdvpfa2gq5hfkn35nfnlsv274rljtbhr0mf014chvco3h2u8kbf1t8vpi8a31v84mfl507o)`,doesn't store names for the definitions it depends on (like the`[List.foldLeft](https://share.unison-lang.org/@unison/p/code/latest/namespaces/public/;/terms/@d45pkcvl6ftqlrvuedqoqokel7v7naugj7dq7079vkvk1p67p1uvb9bl7c59s6eltblacdtvh1u1ev1ps9gjt9tq5ehd4l7shcbcjro)`function); it references these definitions via their hash. As a result, changing the name(s) associated with a definition is easy.

Let's try this out.`[List.reverse](https://share.unison-lang.org/@unison/p/code/latest/namespaces/public/;/terms/@pblsc3e1nibatbigr5lqtjvfucqa0ln2bdvpfa2gq5hfkn35nfnlsv274rljtbhr0mf014chvco3h2u8kbf1t8vpi8a31v84mfl507o)`is defined using`[List.foldLeft](https://share.unison-lang.org/@unison/p/code/latest/namespaces/public/;/terms/@d45pkcvl6ftqlrvuedqoqokel7v7naugj7dq7079vkvk1p67p1uvb9bl7c59s6eltblacdtvh1u1ev1ps9gjt9tq5ehd4l7shcbcjro)`.Let's rename that to`List.foldl`to make it more familiar to Haskell fans. Try out the following command (you can use tab completion here if you like):

```
.> move.term base.data.List.foldLeft base.data.List.foldl

  Done.

.> view base.data.List.reverse

  base.data.List.reverse : [a] -> [a]
  base.data.List.reverse as =
    use base.data.List +:
    base.data.List.foldl (acc a -> a +: acc) [] as
```

Notice that[`view`](https://www.unison-lang.org/learn/ucm-commands/view)shows the`foldl`name now, so the rename has taken effect. Nice!

To make this happen, Unison just changed the name associated with the hash of`[List.foldLeft](https://share.unison-lang.org/@unison/p/code/latest/namespaces/public/;/terms/@d45pkcvl6ftqlrvuedqoqokel7v7naugj7dq7079vkvk1p67p1uvb9bl7c59s6eltblacdtvh1u1ev1ps9gjt9tq5ehd4l7shcbcjro)`in one place.The[`view`](https://www.unison-lang.org/learn/ucm-commands/view)command looks up the names for the hashes on the fly, right when it's printing out the code.

This is important: Unisonisn'tdoing a bunch of text mutation on your behalf, updating possibly thousands of files, generating a huge textual diff, and also breaking a bunch of downstream library users who are still expecting that definition to be called by the old name. That would be ridiculous, right?

So rename and move things around as much as you want. Don't worry about picking a perfect name the first time. Give the same definition multiple names if you want. It's all good!

The fact that Unison codebases are immutable and append-only means that we can "rewind" our codebase to an earlier point in time. Use the[`reflog`](https://www.unison-lang.org/learn/ucm-commands/reflog)command to see a log of the codebase changes. You should see some help text and a numbered list of hashes.

```
1. #2cbugd57qa : move.term .base.data.List.foldLeft .base.data.List.foldl
 2. #na6fel77ai : pull unison.public.base.latest .base
 3. #sjg2v58vn2 : (initial reflogged namespace)
```

Reflog keeps track of the history of the codebase by recording the hash of the rootnamespaceof your codebase. Namespace hashes change along with updates to the term and type definitions that they enclose. When we renamed`[List.foldLeft](https://share.unison-lang.org/@unison/p/code/latest/namespaces/public/;/terms/@d45pkcvl6ftqlrvuedqoqokel7v7naugj7dq7079vkvk1p67p1uvb9bl7c59s6eltblacdtvh1u1ev1ps9gjt9tq5ehd4l7shcbcjro)`,conceptually, the "state" of the codebase changed, but the log-based format of the codebase history means those changes are retrievable.

Let's try to undo the rename action. Use the[`reset-root`](https://www.unison-lang.org/learn/ucm-commands/reset-root)command to pick a prior codebase state to return to. We'll give it the hash of the codebase from just before the`move.term`command was issued.

```
.> reset-root #na6fel77ai

  Done.
```

Great! OK, go drink some water, 🚰 and then let's learn more about Unison namespaces and the expected codebase structure.

## Namespace conventions and searching the UCM

Thus far we've been exploring the`base`standard library that automatically downloads into the root of your codebase, but when you're writing your own Unison code, you won't want to add your functions and terms at the root. Instead, let's introduce the concept of "namespaces" by creating a`tour`namespace and establishing some conventions for organizing it.

Unisonnamespacesare mappings from names to definitions. Names in Unison look like this:`math.sqrt`,`.base.Int`,`base.Nat`,`base.Nat.*`,`++`,or`foo`.That is: an optional`.`,followed by one or more segments separated by a`.`,with the last segment allowed to be an operator name like`*`or`++`.

We often think of these names as forming a tree, much like a directory of files, and names are like file paths in this tree.[Absolute names](https://www.unison-lang.org/learn/language-reference/absolutely-qualified-identifiers)(like`.base.Int`)start with a`.`and are paths from the root of this tree andrelativenames (like`math.sqrt`)are paths starting from the current namespace, which you can set using the[`namespace`](https://www.unison-lang.org/learn/ucm-commands/cd)(or equivalently[`cd`](https://www.unison-lang.org/learn/ucm-commands/cd))command:

In the codebase manager console, create a`tour`namespace with the`cd`command. This is where you'll adding code for the remainder of this guide.

Notice the prompt changes to`.tour>`,indicating your current namespace is now`.tour`.When editing Unison code, and interacting with the UCM any relative names not locally bound in your file will be resolved by prefixing them with the current namespace of`.tour`.Your UCM commands and code are "scoped" to this namespace unless otherwise indicated with an absolute path.

Next, create a copy of the`base`directory by`fork`ingthe top-level`base`library into a namespace called`lib`.

```
.tour> fork .base lib.base
```

🧠

Unison expects namespaces directly under the root, like our`tour`namespace, to have one namespace called`lib`containing all the dependencies necessary for running the code in the namespace tree.

Earlier, when we used the`find`command, we were searching from the root of our codebase, but within the`tour`namespace, which might grow to contain multiple libraries, we'll want a different set of conventions for finding our own code versus searching through our dependencies.

Try using the`find`command again for`List.reverse`:

```
.tour> find List.reverse

I couldn't find matches in this namespace, searching in 'lib'...

1. lib.base.data.List.reverse : [a] -> [a]
```

When we'reinside the tour namespace,the UCM`find`command will search for non-dependency Unison code in the current namespace tree before searching through the lib directory.

If you want to perform a search which specifically includes the`lib`namespace, you can use the`find.all`UCM command.

```
.tour> find.all List.reverse

1. lib.base.data.List.reverse : [a] -> [a]
```

Notice how these results don't contain the top level`base`result? That's because`find`and`find.all`are both scoped to the`tour`namespace based on where we're searching!

If we want a global search of our codebase, we can use the`find.global`command.

Ok, that's enough preamble, let's start writing some Unison code!

### Unison's interactive scratch files

The codebase manager lets you make changes to your codebase and explore the definitions it contains, but it also listens for changes to any file ending in`.u`in the current directory. When any such file is saved (which we call a "scratch file"), Unison parses and typechecks that file. Let's try this out.

Keep your`ucm`terminal running and open up a file,`scratch.u`(or`foo.u`,or whatever you like) in your preferred text editor (if you want syntax highlighting for Unison files,[follow this link](https://www.unison-lang.org/learn/usage-topics/editor-setup)for instructions on setting up your editor).

Now put the following in your scratch file:

```
use base
square : Nat -> Nat
square x =
  use Nat *
  x * x
```

This defines a function called`square`.It takes an argument called`x`and it returns`x`multiplied by itself.

The first line,`use base`,tells Unison that you want to use short names for the base libraries in this file (which allows you to say`[Nat](https://share.unison-lang.org/@unison/p/code/latest/namespaces/public/;/types/@@Nat)`instead of having to say`base.Nat`).The UCM will prefer the`base`instance found in`lib`.

When you save the file, Unison replies:

```
✅

I found and typechecked these definitions in ~/unisoncode/scratch.u. If you do an
`add` or `update` , here's how your codebase would change:

  ⍟ These new definitions are ok to `add`:

    square : base.Nat -> base.Nat

Now evaluating any watch expressions (lines starting with `>`)… Ctrl+C cancels.
```

It typechecked the`square`function and inferred that it takes a natural number and returns a natural number, so it has the type`Nat -> Nat`.It also tells us that`square`is "ok to \`add\`." We'll do that shortly, but first, let's try calling our function right in the`scratch.u`file, just by starting a line with`>`:

```
use base

square : Nat -> Nat
square x = x * x

> square 4
```

And Unison replies:

That`6 |`is the line number from the file. The`> square 4`on line 6 of the file, starting with a`>`is called a "watch expression", and Unison uses these watch expressions instead of having a separate read-eval-print-loop (REPL). The code you are editing can be run interactively, right in the same spot as you are doing the editing, with a full text editor at your disposal, with the same definitions all in scope, without needing to switch to a separate tool.

The`use base`is awildcard use clause.This lets us use anything from the`base`namespace under the`lib`namespace unqualified. For example we refer to`base.Nat`as simply`Nat`.

Question:do we really want to reevaluate all watch expressions on every file save? What if they're expensive? Luckily, Unison keeps a cache of results for expressions it evaluates, keyed by the hash of the expression, and you can clear this cache at any time without ill effects. If a result for a hash is in the cache, Unison returns that instead of evaluating the expression again. So you can think of and use your`.u`scratch files a bit like spreadsheets, which only recompute the minimal amount when dependencies change.

🤓

There's one more ingredient that makes this work effectively, and that's functional programming. When an expression has no side effects, its result is deterministic, and you can cache it as long as you have a good key to use for the cache, like the Unison content-based hash. Unison's type system won't let you do I/O inside one of these watch expressions or anything else that would make the result change from one evaluation to the next.

Let's try out a few more examples:

```
-- A comment, ignored by Unison

> List.reverse [1,2,3,4]
> 4 + 6
> 5.0 / 2.0
> not true
```

```
✅

~/unisoncode/scratch.u changed.

Now evaluating any watch expressions (lines starting with
`>`)… Ctrl+C cancels.

  6 | > List.reverse [1,2,3,4]
        ⧩
        [4, 3, 2, 1]

  7 | > 4 + 6
        ⧩
        10

  8 | > 5.0 / 2.0
        ⧩
        2.5

  9 | > not true
        ⧩
        false
```

## Testing your code

Let's add a test for our`square`function:

```
square : Nat -> Nat
square x = x * x

test> square.tests.ex1 =
   use Nat ==
   check (square 4 == 16)
```

Save the file, and Unison comes back with:

```
8 | test> square.tests.ex1 = check (square 4 == 16)

✅ Passed : Proved.
```

Some syntax notes:

-   The`test>`prefix tells Unison that what follows is a test watch expression. Note that we're also giving a name to this expression,`square.tests.ex1`.
-   Note: there's nothing special about the name`square.tests.ex1`;we could call those bindings anything we wanted. Here we use the convention that tests for a definition`foo`go in`foo.tests`.
-   Note: these terms are being written while the UCM prompt is still reading`.tour>`,so adding or updating them would mean that the fully qualified name for, say,`square`would be`.tour.square`.

The`[test.check](https://share.unison-lang.org/@unison/p/code/latest/namespaces/public/;/terms/@9ail3d2atr1m036tttqjgt2cd2q5ncadg8785h2j9ad9qt4erqfc4r8jm4mkdhh75uen4fob74rlanem97atc5j3l94s1olqtq3iaa8)`function has the signature`[test.check](https://share.unison-lang.org/@unison/p/code/latest/namespaces/public/;/terms/@9ail3d2atr1m036tttqjgt2cd2q5ncadg8785h2j9ad9qt4erqfc4r8jm4mkdhh75uen4fob74rlanem97atc5j3l94s1olqtq3iaa8) : [Boolean](https://share.unison-lang.org/@unison/p/code/latest/namespaces/public/;/types/@@Boolean) -> [[test.Result](https://share.unison-lang.org/@unison/p/code/latest/namespaces/public/;/types/@aql7qk3iud6vs4cvu43aimopoosgk0fnipibdkc3so13adencmibgfn0u5c01r0adei55nkl3ttsjhl8gbj7tr4gnpj63g64ftbq6s0)]`.It takes a`[Boolean](https://share.unison-lang.org/@unison/p/code/latest/namespaces/public/;/types/@@Boolean)`expression and gives back a list of test results, of type`[base.test.Result]`(try`view test.Result`).In this case there was only one result, and it was a passed test.

### A property-based test

Let's test this a bit more thoroughly.`square`should have the property that`square a * square b == square (a * b)`for all choices of`a`and`b`.The testing library supports writing property-based tests like this. There's some new syntax here, explained afterwards:

```
use base

square : Nat -> Nat
square x = x * x

use test

test> square.tests.ex1 = check (square 4 == 16)

test> square.tests.prop1 =
  go _ = a = !gen.natInOrder
         b = !gen.natInOrder
         expect (square a * square b == square (a * b))
  runs 100 go
```

```
11 |   go _ = a = !natInOrder

✅ Passed : Passed 100 tests.
```

This will test our function with a bunch of different inputs.

#### Syntax notes

-   The Unison block, which begins after an`=`,can have any number ofbindings(like`a = …`)all at the same indentation level, terminated by a single expression (here`expect (square ..)`),which is the result of the block.
-   You can call a function parameter`_`if you just plan to ignore it. Here,`go`ignores its argument; its purpose is just to make`go`[lazily evaluated](https://www.unison-lang.org/learn/fundamentals/values-and-functions/delayed-computations)so it can be run multiple times by the`runs`function.
-   `[natInOrder](https://share.unison-lang.org/@unison/p/code/latest/namespaces/public/;/terms/@6qtavmudfs4pb82gvknmh5o8mo8e7lovnrkoobbgsfat1bt6d6ppelgderbq7l1o064nsr7cejfcu09i4qtku9qbaqsh7muf0vor7k0)`is a[delayed computation](https://www.unison-lang.org/learn/fundamentals/values-and-functions/delayed-computations).A delayed computation is one in which the result is not computed right away. The signature for a delayed computation can be thought of as a function with no arguments, returning the eventual result:`() -> a`.The`!`in`!gen.natInOrder`evaluates the delayed computation, summoning a value of type`[Nat](https://share.unison-lang.org/@unison/p/code/latest/namespaces/public/;/types/@@Nat)`.
-   `natInOrder`comes from`test`\-`test.gen.natInOrder`.It's a "generator" of natural numbers.`!natInOrder`generates one of these numbers starting at 0 and incrementing by one each time it is called.

## Adding code to the codebase

The`square`function and the tests we've written for it are not yet part of the codebase. So far they only exist in our scratch file. Let's add them now. Switch to the Unison console and type`add`.You should get something like:

```
.tour> add

  ⍟ I've added these definitions:

    square             : Nat -> Nat
    square.tests.ex1   : [Result]
    square.tests.prop1 : [Result]
```

You've just added a new function and some tests to your Unison codebase under the`tour`namespace. Try typing`view square`or`view square.tests.prop1`.Notice that Unison inserts precise`use`statements when rendering your code.`use`statements aren't part of your code once it's in the codebase. When rendering code, a minimal set of`use`statements is inserted automatically by the code printer, so you don't have to be precise with your`use`statements.

If you type`test`at the Unison prompt, it will "run" your test suite:

```
.tour> test

  Cached test results (`help testcache` to learn more)

  ◉ square.tests.ex1      : Proved.
  ◉ square.tests.prop1    : Passed 100 tests.

  ✅ 2 test(s) passing

  Tip: Use view square.tests.ex1 to view the source of a test.
```

But actually, it didn't need to run anything! All the tests had been run previously and cached according to their Unison hash. In a purely functional language like Unison, tests like these are deterministic and can be cached and never run again. No more running the same tests over and over again!

## Moving and renaming terms

When we added`square`,we were at the`tour`namespace, so`square`and its tests are at`tour.square`.We can also move the terms and namespaces to different locations in our codebase with the`move`commands.

```
.tour> move.term .tour.square mySquare

  Done.

.tour> find

  1.  mySquare : base.Nat -> base.Nat

.tour> move.namespace square.tests mySquare.tests

  Done.
```

We're using`.tour.square`to refer to the`square`definition with an absolute path, and then moving it to therelativename`mySquare`.When you're done shuffling some things around, you can use`find`with no arguments to view all the definitions under the current namespace:

```
.tour> find

  1. mySquare : Nat -> Nat
  2. mySquare.tests.ex1 : [Result]
  3. mySquare.tests.prop1 : [Result]
```

Also notice that we don't need to rerun our tests after this reshuffling. The tests are still cached:

```
.tour> test

  Cached test results (`help testcache` to learn more)

  ◉ mySquare.tests.ex1       : Proved.
  ◉ mySquare.tests.prop1     : Passed 100 tests.

  ✅ 2 test(s) passing

  Tip:  Use view square.tests.ex1 to view the source of a test.
```

We get this for free because the test cache is keyed by the hash of the test, not by what the test is called.

When you're starting out writing some code, it can be nice to just put it in a temporary namespace, perhaps called`temp`or`scratch`.Later, without breaking anything, you can move that namespace or bits and pieces of it elsewhere, using the[`move.term`](https://www.unison-lang.org/learn/ucm-commands/move/term),[`move.type`](https://www.unison-lang.org/learn/ucm-commands/move/typez),and[`move.namespace`](https://www.unison-lang.org/learn/ucm-commands/move/namespace_)commands.

## Modifying existing definitions

Instead of starting a function from scratch, often you just want to slightly modify something that already exists. Here we'll make a change to the implementation of our`mySquare`function.

### Using the`edit`command

Try doing`edit mySquare`from your prompt (note you can use tab completion):

```
.tour> edit mySquare
  ☝️

  I added these definitions to the top of ~/unisoncode/scratch.u

    mySquare : Nat -> Nat
    mySquare x =
      use Nat *
      x * x

  You can edit them there, then do `update` to replace the definitions currently in this branch.
```

This copies the pretty-printed definition of`mySquare`into your scratch file "above the fold." That is, it adds a line starting with`---`and puts whatever was already in the file below this line. Unison ignores any file contents below the fold.

Let's edit`mySquare`and instead define`mySquare x`(just for fun) as the sum of the first`x`odd numbers (here's a[nice geometric illustration of why this gives the same results](https://math.stackexchange.com/a/639079)):

```
use base

mySquare : Nat -> Nat
mySquare x =
  sum (map (x -> x * 2 + 1) (range 0 x))

sum : [Nat] -> Nat
sum = foldLeft (+) 0
```

```
✅

I found and typechecked these definitions in ~/unisoncode/scratch.u. If you do an
''add'' or ''update'' , here's how your codebase would change:

    ⍟ These new definitions are ok to `add`:

      sum : [Nat] -> Nat

    ⍟ These names already exist. You can `update` them to your new definition:

      mySquare : Nat -> Nat
```

### Adding an updated definition to the codebase

Notice the message says that`mySquare`is ok to[`update`](https://www.unison-lang.org/learn/ucm-commands/update).Let's try that:

```
.tour> update

  ⍟ I've added these definitions:

    sum : [Nat] -> Nat

  ⍟ I've updated these names to your new definition:

    mySquare : Nat -> Nat
```

### Only affected tests are rerun on[`update`](https://www.unison-lang.org/learn/ucm-commands/update)

If we rerun the tests, the tests won't be cached this time, since one of their dependencies has actually changed:

```
.tour> test

  ✅

    New test results:

  ◉ mySquare.tests.ex1      : Proved.
  ◉ mySquare.tests.prop1    : Passed 100 tests.

  ✅ 2 test(s) passing

  Tip: Use view mySquare.tests.ex1 to view the source of a test.
```

Notice the message indicates that the tests weren't cached. If we do`test`again, we'll get the newly cached results.

The dependency tracking for determining whether a test needs rerunning is 100% accurate and is tracked at the level of individual definitions. You'll only rerun a test if one of the individual definitions it depends on has changed.

## Publishing code and installing Unison libraries

🎉

The last you'll need to do to get set up to write Unison code is to sign up for Unison-share! Head to[Unison Share](https://share.unison-lang.org/)and follow the instructions there to link your local codebase for code hosting!

Code is published to Unison's own code hosting solution,[Unison Share](https://share.unison-lang.org/),using the[`push`](https://www.unison-lang.org/learn/ucm-commands/push)command and libraries are installed via the[`pull`](https://www.unison-lang.org/learn/ucm-commands/pull)command. There's no separate tooling needed for managing dependencies or publishing code, and you'll never encounter dependency conflicts in Unison.

> Saved from https://www.unison-lang.org/learn/tour/ on 2023-04-16T15:03:54-05:00