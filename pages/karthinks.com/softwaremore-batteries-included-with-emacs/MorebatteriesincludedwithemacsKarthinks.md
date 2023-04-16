---
title: More batteries included with emacs
pageTitle: More batteries included with emacs | Karthinks
length: 13046
author: 
timestamp: 2023-04-16T15:01:50-05:00
markdownload-tags: []
markdownload-source: https://karthinks.com/software/more-batteries-included-with-emacs/
markdownload-hostname: karthinks.com
---

# More batteries included with emacs | Karthinks

## Excerpt
> Continuing from last time, here are a dozen more tricks Emacs has up its sleeve that it’s shy about telling you. We continue to chip away at Emacs' discoverability problem, one demo at a time.
Same rules as before:
  No packages, stock Emacs only
  No steep learning curves. Learn each feature in under five minutes or bust. (Up from two minutes last time.)
  No gimmicks.

---
Continuing from [last time](https://karthinks.com/software/batteries-included-with-emacs/), here are a dozen more tricks Emacs has up its sleeve that it’s shy about telling you. We continue to chip away at Emacs' discoverability problem, one demo at a time.

Same rules as before:

-   No packages, **stock Emacs only**
    
-   No steep learning curves. **Learn each feature in under five minutes or bust**. (Up from two minutes last time.)
    
-   **No gimmicks**. No doctor, tetris, snake, dunnet…
    
-   **Just the deltas**. No commonly mentioned packages like flymake, doc-view, outline-minor-mode, gnus or eww. Nothing that Emacs brings up automatically or a nonspecific Google search gets you.
    
-   Assume a modern Emacs, 26.3+.
    
    Also, if you’re new to Emacs:
    
    | Emacs jargon | Modern parlance |
    | --- | --- |
    | `M-x` | Alt + x |
    | `C-x` | Ctrl + x |
    | Frame | Emacs window |
    | Window | split/pane |
    | Buffer | Contiguous chunk of text/data |
    | Point | Cursor position in buffer |
    | Active Region | Text selection |
    | Region | Text selection (not highlighted) |
    | Face | Font, color and display properties |
    

(Sorry.)

Okay? Let’s go:

## Strokes (`M-x strokes-help`)

Control Emacs with mouse gestures:

 Your browser does not support the video tag.

Why are you using Emacs with a mouse, you ask.

In this demo I used gestures to handle window management: ![][fig1]

This is because I lack imagination. Consider: You can bind _any_ gesture to _any_ Emacs command.

The real utility of this feature is when you find yourself going back and forth between different interaction paradigms, like a web browser and Emacs, requiring frequent context switching between full keyboard and keyboard/mouse based interaction.

Want to `org-capture` or `org-roam-insert` something from your browser? Want to quickly recompile a TeX document while you’re busy mousing through the corresponding PDF? Need to tweak some markup in your document while you work on graphics in Inkscape? [Clone a repository you found online](http://xenodium.com/emacs-clone-git-repo-from-clipboard/)? `strokes-mode`.

The second use case for Strokes is to control Emacs during reading-centric activities, like email or RSS. Star or delete messages, pause or skip music tracks (with some elisp around `playerctl`), navigate info nodes, or cycle through Elfeed searches.

`M-x strokes-help` will get you started. As with most things Emacs, there’s more to the library than meets the eye, like support for multi-part strokes you can use to edit documents in Chinese. The buffer showing the gesture being traced is for demo purposes only, you can customize `strokes-use-strokes-buffer`:

```
(global-set-key (kbd "<down-mouse-3>") 'strokes-do-stroke) ; Draw strokes with RMB
(setq strokes-use-strokes-buffer nil) ; Don't draw strokes to the screen
```

___

## Minibuffer completion styles (`C-h v completion-styles`)

The default minibuffer experience is annoying enough to send anyone into the arms of `ido`, `ivy` or `helm`. But Emacs does a lot better in the tab-completions department than it lets on. The minibuffer can match by substrings, regexps, initials and even (as of Emacs 27+) fuzzily, and it can do all of them at once if you don’t mind a giant pile of matches.

You can do worse than

```
(setq completion-styles '(initials partial-completion flex)) ; > Emacs 27.1
(setq completion-cycle-threshold 10)
```

Now `M-x ohba` tab-expands to `org-hide-block-all`, `M-x qrr` to `query-replace-regexp` and so on, allowing you to tab-cycle between up to ten completions. Use tab to match file names fuzzily and expand short paths (`~/.l/sh/g`) to long ones (`~/.local/share/git`).

Stretching the five minute rule a bit: You probably want different matching rules for different categories, though. Flex matching on extended commands (`M-x`) can dump hundreds of irrelevant matches on short input, for instance. You can customize `completion-category-overrides` to set the matching style by the category being completed.

Finally, note that completion styles dictate how a candidate pool is found. Incremental completion systems like `ido` specify how found candidates are displayed and chosen. These are orthogonal functions, so any set of completion styles should work with any completion system. Unfortunately this isn’t the case, because `ivy` and `helm` are off doing their own thing. Emacs’ built in completion styles do work with `icomplete`, `ido` and possibly `selectrum` though.

___

## Fake Ido ( `M-x fido-mode`)

Improves the other half of the default minibuffer experience: interacting with selection candidates. Navigating to, from and inside the default completions buffer takes too many key presses. `fido-mode` brings `ido` like selection to every command in Emacs that uses `completing-read`, which is most of them.

Of course, `ido` is also built in, but you know all about it. It also takes some effort to get ido to work with every command. `fido-mode` is set-and-forget, but it’s Emacs 27+ only.

___

## Easier rectangle editing (`M-x cua-selection-mode`)

 Your browser does not support the video tag.

The default rectangle commands in Emacs (`C-x r` map) leave a little to be desired in terms of interactivity. Emacs has fully featured rectangle editing, but it’s presented in an odd sort of way, as a subfeature of Common User Access. It’s pretty nifty:

 Your browser does not support the video tag.

Start or clear a rectangle selection with `C-<return>`. Cycle through rectangle corners with `<return>`. Yes, you’ll need to rebind it if you use Org or ESS.

It’s a very handy feature, but note that `cua-selection-mode` does _not_ mesh well with undo. If you use `undo-tree`, it can lock up trying to undo a `cua-selection` edit.

___

## speedbar (`M-x speedbar`)

A file explorer and much, much more in Emacs. If you need the Neotree or dired-sidebar package to do anything in Emacs, check out speedbar.

Speedbar pops up a side frame that looks like a file browser, which it is. But it also integrates with `imenu` to show you headings/tags inside files, and with `vc` to show you the status of your files:

![Figure 1: Speedbar showing tags and VC status][fig2]

Figure 1: Speedbar showing tags and VC status

![Figure 2: Speedbar showing headings in an org file][fig3]

Figure 2: Speedbar showing headings in an org file

From inside speedbar you can act on files with (frustratingly, _almost_ dired-like) keybindings. Most regular commands run from speedbar will be run in the associated buffer.

It tracks the active buffer to show you appropriate hierarchical information. Here I moved point into `info` and `latex-mode` buffers:

![][fig4] ![][fig5]

Perhaps you’d like a list of your buffers instead. Switch between buffer and file view with `b` / `f`. (You can expand a buffer entry for tag/headings.)

![][fig6]

___

## Orgtbl minor mode (`M-x orgtbl-mode`)

Org-mode is its own thing, a full fledged notetaking, publishing, literate-programming, task tracking application with a growing ecosystem that’s parallel to Emacs’.<sup id="fnref:1"><a href="https://karthinks.com/software/more-batteries-included-with-emacs/#fn:1" role="doc-noteref">1</a></sup>

But it does provide a handy tool that’s useful in any mode: Making tables. If you’re one of the dozen Emacs users who haven’t hopped on the Org train, this one’s for you.

With `orgtbl-mode`, you can make tables by starting a line with the pipe character and hitting tab to make a field–like you would in `org-mode`–but **without getting in the way of your major mode**. You can just leave it on and get on with your work.

 Your browser does not support the video tag.

Here I table some parameters in matlab-mode as a comment, but even more lazily than prescribed above by calling `orgtbl-create-or-convert-from-region` (`C-c |`) on some data.

With some work, you can export the table in place to html or LaTeX, but setting this up would break the five minute rule. You can also use the table as a spreadsheet, which is beyond me.

___

## Regexp builder (`M-x re-builder`)

![][fig7] Emacs’ regular expressions syntax can be idiosyncratic if you’re used to [PCRE](https://www.pcre.org/). Are unescaped parens matched literally or do they act as capture groups? Does + need to be escaped to match multiple occurrences of a character? How do I match whitespace again?<sup id="fnref:2"><a href="https://karthinks.com/software/more-batteries-included-with-emacs/#fn:2" role="doc-noteref">2</a></sup>

I can never remember. For regularly confused users like me there is `re-builder`. Build your regular expressions interactively, one character at a time. Save it to the kill ring for later use. This is a godsend when testing regexes for non-interactive use, like when writing elisp helpers.

A cleaner approach to regular expressions in Emacs, as most package maintainers will tell you, is to use the `rx` library instead. `rx` translates regular expressions in sexp form to a regexp string:

```
(rx (and "("
         (or "use-package" "require")
         (+ space)
         (group (syntax symbol))))
```

```
(\(?:\(?:requir\|use-packag\)e\)[[:space:]]+\(\s_\)
```

___

## Future history (`M-n` in prompts)

`M-p` and `M-n` cycle through history items in minibuffer prompts. So what happens when you press `M-n` when you’re at a blank prompt?

Emacs tries to Do What You Mean. If the cursor is over a file name, for instance, `M-n` at the find-file prompt inserts the file path at point into the minibuffer. From the manual:

> The “future history” for file names includes several possible alternatives you may find useful, such as the file name or the URL at point in the current buffer. The defaults put into the “future history” in this case are controlled by the functions mentioned in the value of the option file-name-at-point-functions.

Like the DWIM behavior of Emacs commands on regions, many utilities account for `future-history`. You can usually drag whatever relevant object point is at into the minibuffer prompt with `M-n`.

___

## Transpose regions (`M-x transpose-region`)

Lurking in the bevy of transposition commands in Emacs is `transpose-region`, which does what it says on the tin<sup id="fnref:3"><a href="https://karthinks.com/software/more-batteries-included-with-emacs/#fn:3" role="doc-noteref">3</a></sup>:

 Your browser does not support the video tag.

Exchange any two non-overlapping regions in a buffer. You’ll need to assign a keybinding though.

```
(global-set-key (kbd "C-x C-M-t") 'transpose-regions)
```

Also of note: `transpose-paragraphs`, which sounds like a text-mode utility but works pretty well on adjacent blocks of code.

___

## View mode, again (`M-x view-mode`)

View-mode got an airing [the last time around](https://karthinks.com/software/batteries-included-with-emacs/#view-mode--m-x-view-mode) but I like it so much I will repeat myself. **Turn Emacs into a pager.**

By default, pressing `v` in dired will open a file in view-mode. You can dismiss the window and buffer with `q`, so regular buffers can essentially function as info or help buffers do with `View`. You can scroll or isearch without pressing any modifier keys, and all advanced navigation commands (like xref) are still available, so this is the perfect way to explore a code repository.

___

## Newsticker (`M-x newsticker-treeview`)

AKA I’m not brave enough to use GNUS! ![][fig8]

Yes, Emacs has a traditionally styled RSS reader built in. You can organize feeds by folders, peruse with one button, the usual. This is the only feature in this list I no longer use, because Elfeed’s design syncs with my brain much better. That said, Newsticker is excellent for following high volume feeds like subreddits since it doesn’t maintain a database:

> You can mark \[an item\] as “old” when you have read it or – if you want to keep it – you can mark it as “immortal”. You can do that manually and you can define filters which do that automatically, see below. When a headline has vanished from the feed it is automatically marked as “obsolete” unless it has the status “immortal”. “Obsolete” headlines get removed automatically after a certain time.

Another advantage over GNUS is that it needs very little configuration<sup id="fnref:4"><a href="https://karthinks.com/software/more-batteries-included-with-emacs/#fn:4" role="doc-noteref">4</a></sup>. I used it for a while and never read the info page. `newsticker-add-url` to add a source (atom/RSS feed), `newsticker-treeview` to browse.

___

## Semantic and Semantic decorators (`M-x semantic-decoration-mode`)

Get Emacs to decorate your python code: ![][fig9]

Doing this is like pointing a firehose at a matchstick: Semantic is [part of CEDET](http://alexott.net/en/writings/emacs-devenv/EmacsCedet.html#sec5), a large collection of (mostly) built-in tools to try to turn Emacs into a full-fledged IDE. It… doesn’t work great on a code base of serious size or import, but it’s good at analyzing exploratory code/small utilities.

CEDET is written as plumbing, and unless you’re writing a package you probably shouldn’t need to do anything beyond turning on/off minor modes to use it.

Ultimately Semantic is inadequate on account of the XY problem. LSP handles everything it’s supposed to do and does it better, but Semantic is built-in and there if you just want some code-awareness in your Emacs.

___

## Next Time

This was my attempt at covering built-in features of Emacs I use regularly that I don’t see being talked about much. There are many more, like `project`, `flymake` and `outline-minor-mode` that have received a surge of interest in recent times<sup id="fnref:5"><a href="https://karthinks.com/software/more-batteries-included-with-emacs/#fn:5" role="doc-noteref">5</a></sup>. There are also features I know of that aren’t talked about much, like `ede-mode`, `2C-two-columns`, `skeleton` and `tempo`. I don’t bring these up since I don’t have any experience with them.

So this concludes my list.

Something entirely different next time: Typing equations in Emacs faster than you can write on paper.

___

1.  I hear the next major version will make you pancakes. [↩︎](https://karthinks.com/software/more-batteries-included-with-emacs/#fnref:1)
    
2.  Matched literally, no and “\\s-” respectively. [↩︎](https://karthinks.com/software/more-batteries-included-with-emacs/#fnref:2)
    
3.  Vim users might be familiar with the popular `exchange` (`evil-exchange`) plugin that exchanges regions of text. [↩︎](https://karthinks.com/software/more-batteries-included-with-emacs/#fnref:3)
    
4.  Which doesn’t mean it’s not there. Let’s say I resisted the urge to customize Newsticker because I didn’t want to waste time while I was busy wasting time. [↩︎](https://karthinks.com/software/more-batteries-included-with-emacs/#fnref:4)
    
5.  Interest from developers and from users. Don’t you love that the distinction is fuzzy in Emacs land? [↩︎](https://karthinks.com/software/more-batteries-included-with-emacs/#fnref:5)
    

[fig1]: https://karthinks.com/img/strokes-list.png
[fig2]: https://karthinks.com/img/speedbar-tags-demo.png
[fig3]: https://karthinks.com/img/speedbar-org-demo.png
[fig4]: https://karthinks.com/img/speedbar-info-demo.png
[fig5]: https://karthinks.com/img/speedbar-latex-demo.png
[fig6]: https://karthinks.com/img/speedbar-buffers-demo.png
[fig7]: https://karthinks.com/img/re-builder-demo.png
[fig8]: https://karthinks.com/img/newsticker-demo.png
[fig9]: https://karthinks.com/img/semantic-decorators-demo.png

> Saved from https://karthinks.com/software/more-batteries-included-with-emacs/ on 2023-04-16T15:01:50-05:00