---
title: GitHub - marcusbuffett/pipe-rename: Rename your files using your favorite text editor
pageTitle: GitHub - marcusbuffett/pipe-rename: Rename your files using your favorite text editor
length: 3763
author: marcusbuffett
timestamp: 2023-04-16T15:03:17-05:00
markdownload-tags: []
markdownload-source: https://github.com/marcusbuffett/pipe-rename
markdownload-hostname: github.com
---

# GitHub - marcusbuffett/pipe-rename: Rename your files using your favorite text editor

## Excerpt
> Rename your files using your favorite text editor. Contribute to marcusbuffett/pipe-rename development by creating an account on GitHub.

---
## pipe-rename

[![Crates.io][fig1]](https://crates.io/crates/pipe-rename)

`pipe-rename` takes a list of files as input, opens your `$EDITOR` of choice, then renames those files accordingly.

[![][fig2]](https://github.com/marcusbuffett/pipe-rename/blob/master/renamer.gif)

## Installation

`cargo install pipe-rename`

This will install the `renamer` binary.

## Usage

Usage is simple, just pipe a list of files into `renamer`. This will open your `$EDITOR` (or vim, if not set or passed with `--editor`), and once your editor exits it will detect which files were renamed:

You can also supply filenames as positional arguments. To rename `.txt` files in the current directory:

The default behavior is to rename files, but you can override this. If you want to run `git mv old new` on each rename, you can do something like this:

```shell
ls | renamer --rename-command "git mv"
```

## Helptext

```
Takes a list of files and renames/moves them by piping them through an external editor

USAGE:
    renamer [OPTIONS] [FILES]...

ARGS:
    <FILES>...


OPTIONS:
    -c, --rename-command <COMMAND>
            Optionally set a custom rename command, like 'git mv'

    -e, --editor <EDITOR>
            Optionally set an editor, overriding EDITOR environment variable and default

    -f, --force
            Overwrite existing files

    -h, --help
            Print help information

    -p, --pretty-diff
            Prettify diffs

    -u, --undo
            Undo the previous renaming operation

    -V, --version
            Print version information

    -y, --yes
            Answer all prompts with yes
```

### Caveat emptor

**NB:** it makes sense to be aware of the issues `ls` can cause in this context, depending on the `ls` flavor (or substitute, such as `lsd`, `exa` ...) used. Please read [this document](https://web.archive.org/web/20230102124738/http://mywiki.wooledge.org/ParsingLs) for more information.

While your shell will pass the file names individually, no matter if they contain whitespace, an `ls` that fails to detect the pipe and print one file name per line will cause issues. Unfortunately `ls -Q` also isn't a solution here, because unlike the shell -- which will strip quotes prior to passing them to invoked commands -- `renamer` won't handle the quoted names and will probably complain about non-existent files, too.

### Advanced usage

If you have tools like GNU `find` at your disposal, you can also use the following method:

```shell
find -type f -exec renamer {} +
```

This would execute `renamer` with all of the files matched by `find`. You can use additional `find` predicates such as `-name` or `-ipath` to limit which files to rename. There is, however, one caveat: on large lists of files you may encounter multiple invocations of `renamer` -- and thus your editor -- due to how `find ... -exec {} +` works. It will pass as many file names on the command line as it can fit but it is limited by `ARG_MAX` (see `getconf ARG_MAX` output for how long the overall command line length can be on your system).

Other `find` flavors would allow the following, but it would invoke `renamer` -- and thus your editor -- _once for every single found file_:

```shell
find -type f -exec renamer {} \;
```

In order to sidestep this issue, you can employ `xargs` in conjunction with `find` like so (`-print` is implied for `find`):

```
find -type f | xargs renamer --editor vim
```

The part past `xargs` is the invocation of `renamer` without the file names. It exists just to demonstrate how you would pass arguments to `renamer` using this method.

If your files contain wonky characters you could also try:

```
find -type f -print0 | xargs -0 renamer --editor vim
```

Alas, this could be asking for trouble. If your file names contain line breaks, for example, this could confuse `renamer` which expects a single file name per line when re-reading the edited file.

## Contributors ![sparkles][fig3]

<table><tbody><tr><td><a href="https://mbuffett.com/" rel="nofollow"><img src="https://avatars3.githubusercontent.com/u/1834328?v=4?s=100" alt="" width="100px;"><br><sub><b>Marcus Buffett</b></sub></a><br><a href="https://github.com/marcusbuffett/pipe-rename#ideas-marcusbuffett" title="Ideas, Planning, &amp; Feedback"><g-emoji alias="thinking" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/1f914.png"><img alt="thinking" src="https://github.githubassets.com/images/icons/emoji/unicode/1f914.png" width="20" height="20"></g-emoji></a> <a href="https://github.com/marcusbuffett/pipe-rename/commits?author=marcusbuffett" title="Code"><g-emoji alias="computer" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/1f4bb.png"><img alt="computer" src="https://github.githubassets.com/images/icons/emoji/unicode/1f4bb.png" width="20" height="20"></g-emoji></a></td><td><a href="https://git.ireas.org/" rel="nofollow"><img src="https://avatars2.githubusercontent.com/u/165115?v=4?s=100" alt="" width="100px;"><br><sub><b>Robin Krahl</b></sub></a><br><a href="https://github.com/marcusbuffett/pipe-rename#ideas-robinkrahl" title="Ideas, Planning, &amp; Feedback"><g-emoji alias="thinking" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/1f914.png"><img alt="thinking" src="https://github.githubassets.com/images/icons/emoji/unicode/1f914.png" width="20" height="20"></g-emoji></a> <a href="https://github.com/marcusbuffett/pipe-rename/commits?author=robinkrahl" title="Code"><g-emoji alias="computer" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/1f4bb.png"><img alt="computer" src="https://github.githubassets.com/images/icons/emoji/unicode/1f4bb.png" width="20" height="20"></g-emoji></a> <a href="https://github.com/marcusbuffett/pipe-rename/issues?q=author%3Arobinkrahl" title="Bug reports"><g-emoji alias="bug" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/1f41b.png"><img alt="bug" src="https://github.githubassets.com/images/icons/emoji/unicode/1f41b.png" width="20" height="20"></g-emoji></a></td><td><a href="https://timkovi.ch/" rel="nofollow"><img src="https://avatars.githubusercontent.com/u/651077?v=4?s=100" alt="" width="100px;"><br><sub><b>Max Timkovich</b></sub></a><br><a href="https://github.com/marcusbuffett/pipe-rename#ideas-mtimkovich" title="Ideas, Planning, &amp; Feedback"><g-emoji alias="thinking" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/1f914.png"><img alt="thinking" src="https://github.githubassets.com/images/icons/emoji/unicode/1f914.png" width="20" height="20"></g-emoji></a> <a href="https://github.com/marcusbuffett/pipe-rename/commits?author=mtimkovich" title="Code"><g-emoji alias="computer" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/1f4bb.png"><img alt="computer" src="https://github.githubassets.com/images/icons/emoji/unicode/1f4bb.png" width="20" height="20"></g-emoji></a></td><td><a href="https://github.com/bew"><img src="https://avatars.githubusercontent.com/u/9730330?v=4?s=100" alt="" width="100px;"><br><sub><b>Benoit de Chezelles</b></sub></a><br><a href="https://github.com/marcusbuffett/pipe-rename#ideas-bew" title="Ideas, Planning, &amp; Feedback"><g-emoji alias="thinking" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/1f914.png"><img alt="thinking" src="https://github.githubassets.com/images/icons/emoji/unicode/1f914.png" width="20" height="20"></g-emoji></a></td><td><a href="https://assarbad.net/" rel="nofollow"><img src="https://avatars.githubusercontent.com/u/3238620?v=4?s=100" alt="" width="100px;"><br><sub><b>Oliver Schneider</b></sub></a><br><a href="https://github.com/marcusbuffett/pipe-rename#ideas-assarbad" title="Ideas, Planning, &amp; Feedback"><g-emoji alias="thinking" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/1f914.png"><img alt="thinking" src="https://github.githubassets.com/images/icons/emoji/unicode/1f914.png" width="20" height="20"></g-emoji></a> <a href="https://github.com/marcusbuffett/pipe-rename/commits?author=assarbad" title="Code"><g-emoji alias="computer" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/1f4bb.png"><img alt="computer" src="https://github.githubassets.com/images/icons/emoji/unicode/1f4bb.png" width="20" height="20"></g-emoji></a></td></tr></tbody></table>

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!

[fig1]: https://camo.githubusercontent.com/fcf06859c2bbb059f93c18e64e8c22b3586493cdf61a8c62e88aecb943f11f64/68747470733a2f2f696d672e736869656c64732e696f2f6372617465732f762f706970652d72656e616d65
[fig2]: https://github.com/marcusbuffett/pipe-rename/raw/master/renamer.gif
[fig3]: https://github.githubassets.com/images/icons/emoji/unicode/2728.png
[fig4]: https://avatars3.githubusercontent.com/u/1834328?v=4?s=100
[fig5]: https://github.githubassets.com/images/icons/emoji/unicode/1f914.png
[fig6]: https://github.githubassets.com/images/icons/emoji/unicode/1f4bb.png
[fig7]: https://avatars2.githubusercontent.com/u/165115?v=4?s=100
[fig8]: https://github.githubassets.com/images/icons/emoji/unicode/1f914.png
[fig9]: https://github.githubassets.com/images/icons/emoji/unicode/1f4bb.png
[fig10]: https://github.githubassets.com/images/icons/emoji/unicode/1f41b.png
[fig11]: https://avatars.githubusercontent.com/u/651077?v=4?s=100
[fig12]: https://github.githubassets.com/images/icons/emoji/unicode/1f914.png
[fig13]: https://github.githubassets.com/images/icons/emoji/unicode/1f4bb.png
[fig14]: https://avatars.githubusercontent.com/u/9730330?v=4?s=100
[fig15]: https://github.githubassets.com/images/icons/emoji/unicode/1f914.png
[fig16]: https://avatars.githubusercontent.com/u/3238620?v=4?s=100
[fig17]: https://github.githubassets.com/images/icons/emoji/unicode/1f914.png
[fig18]: https://github.githubassets.com/images/icons/emoji/unicode/1f4bb.png

> Saved from https://github.com/marcusbuffett/pipe-rename on 2023-04-16T15:03:17-05:00