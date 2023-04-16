---
title: Emacs as a supplemental editor
pageTitle: Emacs as a supplemental editor | Wilcox Development Solutions Blog
length: 8856
author: 
timestamp: 2023-04-16T15:02:52-05:00
markdownload-tags: []
markdownload-source: https://blog.wilcoxd.com/2020/07/06/emacs-as-a-supplimental-editor/
markdownload-hostname: blog.wilcoxd.com
---

# Emacs as a supplemental editor | Wilcox Development Solutions Blog

## Excerpt
> The Problem In 2020 I realized I had a problem. I was in four different OSes: Windows, Linux, iOS and some Mac OS X. This wasn’t so much a problem, except that I would find myself with the following problems: The tool I wanted to use wasn’t in the environment at hand I wanted a good shell, no matter where I was I wanted a good CLI experience, and…

---
## The Problem

In 2020 I realized I had a problem. I was in four different OSes: Windows, Linux, iOS and some Mac OS X. This wasn’t so much a problem, except that I would find myself with the following problems:

1.  The tool I wanted to use wasn’t in the environment at hand
2.  I wanted a good shell, no matter where I was
3.  I wanted a good CLI experience, and ideally close to the same command interface language, wherever

I’ve written custom tools in [Pharo Smalltalk](https://pharo.org/), and Pharo is spectacular: an OS indepedent, persistent memory environment with a rapid development language. It’s wonderful for building tools that live inside and can be controlled inside it’s own world.

This approach of creating an isolating environment is great for things that can be isolated. I’ve even created an entire, custom note organizing environment in Smalltalk, complete with UI.

But I’m a developer, where I edit text, entering commands on the command line, interacting with files on the file system. And, for all its wondeful nature, Pharo is not a text editor.

For command line text editing I’ve traditionally used Vim. This was mostly accidental: it was the first CLI text editor that I figured out how to exit. (Really, yes, contrary to the joke about how nobody knows how to exit VIM).

VIM is great but I knew it dependend on the underlaying OS. Which is great, if you can always assume POSIX underneight. I can’t. I don’t need a code editor: [Visual Studio Code](https://code.visualstudio.com/) is _real_ good for that.

So what I really needed was a good text editor that… happens to be an OS.

## Emacs usage one: eshell

This is what got me very interested in Emacs in the first place: I read [Mastering Emacs: A Complete Guide To eShell](https://masteringemacs.org/article/complete-guide-mastering-eshell). A few quotes popped out at me:

> Given Emacs’s UNIX origin, Eshell emulates traditional UNIX shells like bash and the GNU toolchain. This is good news if you are using Windows and cannot be bothered fidgeting with cygwin

I learned enough to launch emacs, in complete GUI mode, and tried eshell. I loved that I could pipe output of commands into a new Emacs buffer. I could also - and here’s the kicker - use the mouse to move the text insert cursor, easily copy and paste lines from the shell output. I had unix sh compatible syntax, including translation of Windows paths for Unix paths. Standard unix tools were reimplemented in Lisp, so I didn’t need to think about installing oddball Unix tools on my Windows (non WSL) system.

I had a real, portable Unix workstation.

I tried eshell over a couple of weekends: 5 minutes here, 5 minutes there, because I was also quarantine homeschooling at the same time (you from the future, those of us in 2020 salute you), and it worked pretty well. Well enough to make it my primary shell environment at work.

For the most part, on a standard Unix, the standard unix tools are called. But, when they don’t exist (like on Windows), eshell falls back to the re-implementations. But eshell does something super elegant here: in addition to being able to run every command on your machine, and some emulated commands, you can also directly run Emacs Elisp.

So, even if `read` isn’t implemented correctly on eshell on Windows, for example, `read-string`, the Elisp command, is availiable.

I spent an evening writing a shell script in eshell: the combination makes it look like a very odd hodge-podge of Lisp and Bash… but there is advantages of a rapid DSL like this. There is, also, a disadvantage.

## Disadvantages

Using eshell as my primary shell limits me to a sometimes odd subset of `sh`, and means that if I want to share a workflow I wrote in eshell, which uses some magic lisp command in the middle of the pipeline, that I am out of luck.

What’s more likely to happen, if I want to write a shareable program, with a pipeline bigger than a few commands, that I break out Powershell and do a lot of Powershell command calling from eshell. (At work I’m supporting two, soon to be three, platforms and two are Windows.)

It means, for sharable programs, the bar between hacking something fast and hacking something for generic use happens faster by necessity. I really wish it was more gradual - see [The Rash Paper](https://dl.acm.org/doi/pdf/10.1145/3278122.3278129) has some good thoughts here (and they even mention eshell!). Alas, Rash is likely a step too far - no history of Lisp usage at work, and some other things holding it back. While also being a step too little - seemingly still hampered with the commandline as REPL paradigm.

## Emacs usage two: BBEdit’s Text Filters and Shell Worksheet support

I love [BBEdit](https://www.barebones.com/products/bbedit/), but it’s only on one of my three OSes.

I reproduced BBEdit’s scripts and test filter support, and that will be the entire separate blog post. I’m relatively happy with what I’ve built on that front. I was never able to figure out how to easily do these things in my primary code editor, Visual Studio Code.

[see my emacs configuration](https://github.com/rwilcox/emacs.d/blob/master/functional_areas/bbedit.el)

## Emacs usage three: Soulver

## Hacked Together with some elisp

I love [soulver](https://www.acqualia.com/soulver/) - I want a calculator that is a worksheet, with variables and everything. I se Soulver very lightly. I do, however, use it every month to balance my monthly bill budget.

I created a Emacs script to help me with that. Fake example follows.

```
(set 'monthly-rent 1000)

(set 'cable         250)
(set 'phone         200)
 
(set 'output-num (- 1500 monthly-rent cable phone))

(set 'soulver-output (format "\n;; %S" output-num))
```

Which, when I run my `M-x soulver/script-eval-elisp-and-append-output-variable` will add the value of `soulver-output` to the current buffer.

[Source](https://github.com/rwilcox/emacs.d/blob/master/functional_areas/soulver.el#L17)

If I wanted to get even more complicted I could call [calc-eval](https://www.gnu.org/software/emacs/manual/html_node/calc/Calling-Calc-from-Your-Programs.html). My Soulver needs are never more than adding columns of number together, and maybe subtracting them, so I likely won’t need this.

## Fancy Fancy Literate Calc package

Since I wrote my lisp script, Robert Schroer wrote [literate-calc-mode](https://github.com/sulami/literate-calc-mode.el), which gives me an exen better experience.

```
Monthly Budget = 1500  => Monthly Budget: 1500
Rent = 1000   => Rent: 1000
Cable = 250  => Cable: 250
Cell = 200   => Cell: 200

Remainder = Monthly Budget - (Rent + Cable + cell)  => Remainder: 50
```

The => part of the line is added by literale-calc-mode.

M-x literate-calc-mode

## Conclusion

I started a relatively easy, but still couple weekends long, project of [converting my blog to Gatsby.js](https://blog.wilcoxd.com/2020/07/03/moving-to-gatsby/), and used Emacs / eshell to drive it. That got me learning just enough Emacs and started me down the journey of configuring my init.el file. I’m still running a pretty vanilla Emacs install, with maybe 12-20 custom functions.

That experiment worked enough when I’m using Emacs day to day on my work machine, and have moved all my machines to my Emacs setup. In day to day use at work I’m using Emacs for four things:

1.  Eshell
2.  complicated text parsing from eshell results
3.  Creation of scratch notes in a new buffer (I have 5 vSC windows open at a time, with 5+ tabs in each, I have a problem finding random snippets of text I’m writing down sometimes.
4.  Markdown mode.

## Closing Thoughts On Emacs in the 2020s

The programability for Emacs is amazing. I wrote TextMate bundles back in the day, then BBEdit bundles, but never got the hang on Visual Studio: Code plugins. But with Emacs I’m back to customizing my editor with extra functionality that may only be useful to me.

There are a couple disadvantages to my setup. First, the out of the box Emacs (blank) distribution gave me almost nothing I would expect from a modern editor. Let me list the non-obvious things:

1.  Everyone uses melpha, not elpha, so go set that up.
2.  Scrolling with a scroll-wheel is jankey out of the box.
3.  “I have to install an extra package for a Recent Files menu?!!”
4.  “OK, where do I go to see all the occurances of this search term in the buffer?”
5.  “What, no tabs?!” (Yes, I know this is coming in Emacs 27, whenever that is coming)

Now, I wanted to learn Emacs by growing my own out of parts I know I would use, instead of opting for someone else’s setup, like DOOMMacs or SpaceMacs. The journey was part of the destination, I guess, just midly wish it hadn’t been quite so long.

The last disadvantage is now I have a text editor and setup consistent across three OSes: Linux, Windows and OS X. But only three: what I’m out is support for iOS. I can not, for example, take an run my literate-calc monthly budget document on iOS. Granted, that’s almost all I want to do with emacs on iOS, as my Editorial workflow is pretty solid and I don’t see a reason to have, or want, a shell in iOS anyway. That’s not how I use iOS devices: light text editing of blog entries yes, tranforming shell output into additional commands I send to a shell? No.

(I guess Visual Studio Code, with its new web exerience, can fufill **all** these OSes. But fails down the things I can find only in Emacs: a cross-platform shell experience, text filters, etc.)

Likewise, for now, I’m happy with Emacs being my toolkit text editor / OS for text processing and shell work, and have a seperate editor be the IDE daily driver.

> Saved from https://blog.wilcoxd.com/2020/07/06/emacs-as-a-supplimental-editor/ on 2023-04-16T15:02:52-05:00