---
title: GitHub - alacritty/alacritty: A cross-platform, OpenGL terminal emulator.
pageTitle: GitHub - alacritty/alacritty: A cross-platform, OpenGL terminal emulator.
length: 3144
author: alacritty
timestamp: 2023-04-16T15:03:10-05:00
markdownload-tags: []
markdownload-source: https://github.com/alacritty/alacritty
markdownload-hostname: github.com
---

# GitHub - alacritty/alacritty: A cross-platform, OpenGL terminal emulator.

## Excerpt
> A cross-platform, OpenGL terminal emulator. Contribute to alacritty/alacritty development by creating an account on GitHub.

---
[![Alacritty Logo][fig1]](https://raw.githubusercontent.com/alacritty/alacritty/master/extra/logo/compat/alacritty-term%2Bscanlines.png)

## Alacritty - A fast, cross-platform, OpenGL terminal emulator

[![Alacritty - A fast, cross-platform, OpenGL terminal emulator][fig2]](https://user-images.githubusercontent.com/8886672/103264352-5ab0d500-49a2-11eb-8961-02f7da66c855.png)

## About

Alacritty is a modern terminal emulator that comes with sensible defaults, but allows for extensive [configuration](https://github.com/alacritty/alacritty#configuration). By integrating with other applications, rather than reimplementing their functionality, it manages to provide a flexible set of [features](https://github.com/alacritty/alacritty/blob/master/docs/features.md) with high performance. The supported platforms currently consist of BSD, Linux, macOS and Windows.

The software is considered to be at a **beta** level of readiness; there are a few missing features and bugs to be fixed, but it is already used by many as a daily driver.

Precompiled binaries are available from the [GitHub releases page](https://github.com/alacritty/alacritty/releases).

## Features

You can find an overview over the features available in Alacritty [here](https://github.com/alacritty/alacritty/blob/master/docs/features.md).

## Further information

-   [Announcing Alacritty, a GPU-Accelerated Terminal Emulator](https://jwilm.io/blog/announcing-alacritty/) January 6, 2017
-   [A talk about Alacritty at the Rust Meetup January 2017](https://www.youtube.com/watch?v=qHOdYO3WUTk) January 19, 2017
-   [Alacritty Lands Scrollback, Publishes Benchmarks](https://jwilm.io/blog/alacritty-lands-scrollback/) September 17, 2018

## Installation

Alacritty can be installed by using various package managers on Linux, BSD, macOS and Windows.

Prebuilt binaries for macOS and Windows can also be downloaded from the [GitHub releases page](https://github.com/alacritty/alacritty/releases).

For everyone else, the detailed instructions to install Alacritty can be found [here](https://github.com/alacritty/alacritty/blob/master/INSTALL.md).

### Requirements

-   At least OpenGL ES 2.0
-   \[Windows\] ConPTY support (Windows 10 version 1809 or higher)

## Configuration

You can find the default configuration file with documentation for all available fields on the [GitHub releases page](https://github.com/alacritty/alacritty/releases) for each release.

Alacritty doesn't create the config file for you, but it looks for one in the following locations:

1.  `$XDG_CONFIG_HOME/alacritty/alacritty.yml`
2.  `$XDG_CONFIG_HOME/alacritty.yml`
3.  `$HOME/.config/alacritty/alacritty.yml`
4.  `$HOME/.alacritty.yml`

### Windows

On Windows, the config file should be located at:

`%APPDATA%\alacritty\alacritty.yml`

## Contributing

A guideline about contributing to Alacritty can be found in the [`CONTRIBUTING.md`](https://github.com/alacritty/alacritty/blob/master/CONTRIBUTING.md) file.

## FAQ

**_Is it really the fastest terminal emulator?_**

Benchmarking terminal emulators is complicated. Alacritty uses [vtebench](https://github.com/alacritty/vtebench) to quantify terminal emulator throughput and manages to consistently score better than the competition using it. If you have found an example where this is not the case, please report a bug.

Other aspects like latency or framerate and frame consistency are more difficult to quantify. Some terminal emulators also intentionally slow down to save resources, which might be preferred by some users.

If you have doubts about Alacritty's performance or usability, the best way to quantify terminal emulators is always to test them with **your** specific usecases.

**_Why isn't feature X implemented?_**

Alacritty has many great features, but not every feature from every other terminal. This could be for a number of reasons, but sometimes it's just not a good fit for Alacritty. This means you won't find things like tabs or splits (which are best left to a window manager or [terminal multiplexer](https://github.com/tmux/tmux)) nor niceties like a GUI config editor.

## IRC

Alacritty discussions can be found in `#alacritty` on Libera.Chat.

## License

Alacritty is released under the [Apache License, Version 2.0](https://github.com/alacritty/alacritty/blob/master/LICENSE-APACHE).

[fig1]: https://raw.githubusercontent.com/alacritty/alacritty/master/extra/logo/compat/alacritty-term%2Bscanlines.png
[fig2]: https://user-images.githubusercontent.com/8886672/103264352-5ab0d500-49a2-11eb-8961-02f7da66c855.png

> Saved from https://github.com/alacritty/alacritty on 2023-04-16T15:03:10-05:00