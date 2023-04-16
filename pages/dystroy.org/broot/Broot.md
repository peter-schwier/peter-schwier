---
title: Broot
pageTitle: Broot
length: 5237
author: 
timestamp: 2023-04-16T15:01:43-05:00
markdownload-tags: []
markdownload-source: https://dystroy.org/broot/
markdownload-hostname: dystroy.org
---

# Broot

## Excerpt
> Broot, a tree oriented file manager

---
![][fig1]  
[Install Broot](https://dystroy.org/broot/install)

## Get an overview of a directory, even a big one

`br -s`

![overview][fig2]

Notice the _unlisted_?

That's what makes it usable where the old `tree` command would produce pages of output.

`.gitignore` files are properly dealt with to put unwanted files out of your way.

As you sometimes want to see gitignored files, or hidden ones, you'll soon get used to the alti and alth shortcuts to toggle those visibilities.

(you can ignore them though, see [documentation](https://dystroy.org/navigation/#toggles)).

## Find a directory then `cd` to it

type a few letters

![cd][fig3]

Hit altenter and you're back to the terminal in the desired location.

This way, you can navigate to a directory with the minimum amount of keystrokes, even if you don't exactly remember where it is.

Broot is fast and doesn't block (any keystroke interrupts the current search to start the next one).

Most useful keys for this:

-   the letters of what you're looking for
-   enter on the root line to go up to the parent (staying in broot)
-   enter to focus a directory (staying in broot)
-   esc to get back to the previous state or clear your search
-   ↓ and ↑ may be used to move the selection
-   altenter to get back to the shell having `cd` to the selected directory
-   alth to toggle showing hidden files (the ones whose name starts with a dot)
-   alti to toggle showing gitignored files
-   `:q` if you just want to quit (you can use ctrlq if you prefer)

## Never lose track of file hierarchy while you search

![search][fig4]

Broot tries to select the most relevant file. You can still go from one match to another one using tab or arrow keys.

You may also search with a regular expression. To do this, add a `/` before the pattern.

And you have [other types of searches](https://dystroy.org/broot/input/#the-filtering-pattern), for example searching on file content (start with `c/`):

![content search][fig5]

You may also apply logical operators or combine patterns, for example searching `test` in all files except json ones could be `!/json$/&c/test` and searching `carg` both in file names and file contents would be `carg|c/carg`.

Once the file you want is selected you can

-   hit enter (or double-click) to open it in your system's default program
-   hit altenter to open it in your system's default program and close broot
-   hit ctrl→ to preview it (and then a second time to go inside the preview)
-   type a verb. For example `:e` opens the file in your preferred editor (which may be a terminal one)

[blog: a broot content search workflow](https://dystroy.org/blog/broot-c-search/)

## Manipulate your files

Most often, when not using broot, you move your files in the blind. You do a few `ls` before, then your manipulation, and maybe you check after.

You can instead do it without losing the view of the file hierarchy.

![mv][fig6]

Move, copy, rm, mkdir, are built in and you can add your own shortcuts.

Here's chmod:

![chmod][fig7]

## Manage files with panels

When a directory is selected, do ctrl→ and you open another panel (you may open other ones, or navigate between them, with ctrl← and ctrl→).

![custom colors tree][fig8]

(yes, colors are fully customizable)

You can for example copy or move elements between panels:

![cpp][fig9]

If you like you may do it Norton Commander style by binding `:copy_to_panel` to F5 and `:move_to_panel` to F6.

## Preview files

Hit ctrl→ when a file is selected and the preview panel appears.

![preview][fig10]

![preview][fig11]

The preview panel stays synchronized with the selection in tree panels.

Broot displays images in high resolution when the terminal supports Kitty's graphics protocol (compatible terminals: [Kitty](https://sw.kovidgoyal.net/kitty/index.html), [WezTerm](https://wezfurlong.org/wezterm/)):

![kitty preview][fig12]

## Apply a standard or personal command to a file

![size][fig13]

Just find the file you want to edit with a few keystrokes, type `:e`, then enter.

You can add verbs or configure the existing ones; see [documentation](https://dystroy.org/broot/conf_file/#verbs-shortcuts-and-keys).

And you can add shortcuts, for example a ctrl sequence or a function key

## Apply commands on several files

Add files to the [staging area](https://dystroy.org/broot/staging-area) then execute any command on all of them.

![staging mv][fig14]

## Replace `ls` (and its clones):

If you want to display _sizes_, _dates_ and _permissions_, do `br -sdp` which gets you this:

![replace ls][fig15]

You may also toggle options with a few keystrokes while inside broot. For example hitting a space, a `d` then enter shows you the dates. Or hit alth and you see hidden files.

## Sort, see what takes space:

You may sort by launching broot with `--sort-by-size` or `--sort-by-date`. Or you may, inside broot, type a space, then `sd`, and enter and you toggled the `:sort_by_date` mode.

When sorting, the whole content of directories is taken into account. So if you want to find on Monday morning the most recently modified files, launch `br --sort-by-date ~`.

If you start broot with the `--whale-spotting` option (or its shortcut `-w`), you get a mode tailored to "whale spotting" navigation, making it easy to determine what files or folders take space.

![size][fig16]

And you keep all broot tools, like filtering or the ability to delete or open files and directories.

If you hit `:fs`, you can check the usage of all filesystems, so that you focus on cleaning the full ones.

![fs][fig17]

Sizes, dates, files counts, are computed in the background, you don't have to wait for them when you navigate.

## Check git statuses:

![size][fig18]

Use `:gf` to display the statuses of files (what are the new ones, the modified ones, etc.), the current branch name and the change statistics.

And if you want to see _only_ the files which would be displayed by the `git status` command, do `:gs`. From there it's easy to edit, or diff, selected files.

[blog: use broot and meld to diff before commit](https://dystroy.org/blog/gg/)

## More...

See [how to install](https://dystroy.org/broot/install), [configure](https://dystroy.org/broot/conf_file), or [use](https://dystroy.org/broot/launch).

[fig1]: https://dystroy.org/broot/img/vache.svg
[fig2]: https://dystroy.org/broot/img/20230129-overview.png
[fig3]: https://dystroy.org/broot/img/20191112-cd.png
[fig4]: https://dystroy.org/broot/img/20210424-mycnf.png
[fig5]: https://dystroy.org/broot/img/20200620-content-memm.png
[fig6]: https://dystroy.org/broot/img/20191112-mv.png
[fig7]: https://dystroy.org/broot/img/20201020-chmod.png
[fig8]: https://dystroy.org/broot/img/20200525-colored-panels.png
[fig9]: https://dystroy.org/broot/img/20200525-cpp.png
[fig10]: https://dystroy.org/broot/img/20200716-preview.png
[fig11]: https://dystroy.org/broot/img/2020081609-preview-image.png
[fig12]: https://dystroy.org/broot/img/20201127-kitty-preview.png
[fig13]: https://dystroy.org/broot/img/20191112-edit.png
[fig14]: https://dystroy.org/broot/img/20210424-staging-mv.png
[fig15]: https://dystroy.org/broot/img/20210425-sdp.png
[fig16]: https://dystroy.org/broot/img/20201020-whale-spotting.png
[fig17]: https://dystroy.org/broot/img/20201020-fs.png
[fig18]: https://dystroy.org/broot/img/20200203-git.png

> Saved from https://dystroy.org/broot/ on 2023-04-16T15:01:43-05:00