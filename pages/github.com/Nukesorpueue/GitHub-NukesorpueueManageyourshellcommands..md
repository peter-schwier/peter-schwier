---
title: GitHub - Nukesor/pueue: Manage your shell commands.
pageTitle: GitHub - Nukesor/pueue: Manage your shell commands.
length: 9811
author: Nukesor
timestamp: 2023-04-16T15:03:34-05:00
markdownload-tags: []
markdownload-source: https://github.com/Nukesor/pueue
markdownload-hostname: github.com
---

# GitHub - Nukesor/pueue: Manage your shell commands.

## Excerpt
> :stars: Manage your shell commands. Contribute to Nukesor/pueue development by creating an account on GitHub.

---
## Pueue

[![GitHub Actions Workflow][fig1]](https://github.com/Nukesor/pueue/actions) [![Crates.io][fig2]](https://crates.io/crates/pueue) [![License: MIT][fig3]](https://opensource.org/licenses/MIT) [![Downloads][fig4]](https://github.com/nukesor/pueue/releases) [![codecov][fig5]](https://codecov.io/gh/nukesor/pueue)

[![Pueue][fig6]](https://raw.githubusercontent.com/Nukesor/images/main/pueue-v2.0.0.gif)

Pueue is a command-line task management tool for sequential and parallel execution of long-running tasks.

Simply put, it's a tool that **p**rocesses a q**ueue** of shell commands. On top of that, there are a lot of convenient features and abstractions.

Since Pueue is not bound to any terminal, you can control your tasks from any terminal on the same machine. The queue will be continuously processed, even if you no longer have any active ssh sessions.

-   [Features](https://github.com/Nukesor/pueue#features)
-   [Installation](https://github.com/Nukesor/pueue#installation)
-   [How to use it](https://github.com/Nukesor/pueue#how-to-use-it)
-   [Similar Projects](https://github.com/Nukesor/pueue#similar-projects)
-   [Design Goals](https://github.com/Nukesor/pueue#design-goals)
-   [Contributing](https://github.com/Nukesor/pueue#contributing)

## Features

-   Scheduling
    -   Add tasks as you go.
    -   Run multiple tasks at once. You decide how many tasks should run concurrently.
    -   Change the order of the scheduled tasks.
    -   Specify dependencies between tasks.
    -   Schedule tasks to run at a specific time.
-   Process interaction
    -   Easy output inspection.
    -   Send input to running processes.
    -   Pause/resume tasks, when you need some processing power right NOW!
-   Task groups (multiple queues)
    -   Each group can have several tasks running in parallel.
    -   Pause/start tasks by a group.
-   Background process execution
    -   The `pueued` daemon runs in the background. No need to be logged in.
    -   Commands are executed in their respective working directories.
    -   The current environment variables are copied when adding a task.
    -   Commands are run in a shell which allows the full feature set of shell coding.
-   Consistency
    -   The queue is always saved to disk and restored on kill/system crash.
    -   Logs are persisted onto the disk and survive a crash.
-   Miscellaneous
    -   A callback hook to, for instance, set up desktop notifications.
    -   JSON output for `log` and `status` for external scripting.
    -   A `wait` subcommand to allow external scripts to wait for a group (or everything) to finish.
-   A lot more. Check the -h options for each subcommand for detailed options.
-   Cross Platform
    -   Linux is fully supported and battle-tested.
    -   MacOS is fully supported and on par with Linux.
    -   Windows is fully supported and working fine for quite a while.
-   [Why should I use it](https://github.com/Nukesor/pueue/wiki/FAQ#why-should-i-use-it)
-   [Advantages over Using a Terminal Multiplexer](https://github.com/Nukesor/pueue/wiki/FAQ#advantages-over-using-a-terminal-multiplexer)

## Installation

There are a few different ways to install Pueue.

#### Package Manager

[![Packaging status][fig7]](https://repology.org/project/pueue/versions)

The preferred way to install Pueue is to use your system's package manager.  
This will usually deploy service files and completions automatically.

Pueue has been packaged for quite a few distributions, check the table on the right for more information.

#### Prebuild Binaries

Statically linked (if possible) binaries for Linux (incl. ARM), Mac OS and Windows are built on each release.  
You can download the binaries for the client and the daemon (`pueue` and `pueued`) for each release on the [release page](https://github.com/Nukesor/pueue/releases).  
Just download both binaries for your system, rename them to `pueue` and `pueued` and place them in your $PATH/program folder.

#### Via Cargo

Pueue is built for the current `stable` Rust version. It might compile on older versions, but this isn't tested or officially supported.

```shell
cargo install --locked pueue
```

This will install Pueue to `$CARGO_HOME/bin/pueue` (default is `~/.cargo/bin/pueue`)

#### From Source

Pueue is built for the current `stable` Rust version. It might compile on older versions, but this isn't tested or officially supported.

```shell
git clone git@github.com:Nukesor/pueue
cd pueue
cargo build --release --locked --path ./pueue
```

The final binaries will be located in `target/release/{pueue,pueued}`.

## How to Use it

Check the wiki to [get started](https://github.com/Nukesor/pueue/wiki/Get-started) :).

There are also detailed sections for (hopefully) every important feature:

-   [Configuration](https://github.com/Nukesor/pueue/wiki/Configuration)
-   [Groups](https://github.com/Nukesor/pueue/wiki/Groups)
-   [Miscellaneous](https://github.com/Nukesor/pueue/wiki/Miscellaneous)
-   [Connect to remote](https://github.com/Nukesor/pueue/wiki/Connect-to-remote)

On top of that, there is a help option (-h) for all commands.

```
Pueue client 2.0.0
Arne Beer <contact@arne.beer>
Interact with the Pueue daemon

USAGE:
    pueue [OPTIONS] [SUBCOMMAND]

OPTIONS:
    -c, --config <CONFIG>      Path to a specific pueue config file to use. This ignores all other
                               config files
    -h, --help                 Print help information
    -p, --profile <PROFILE>    The name of the profile that should be loaded from your config file
    -v, --verbose              Verbose mode (-v, -vv, -vvv)
    -V, --version              Print version information

SUBCOMMANDS:
    add              Enqueue a task for execution
    clean            Remove all finished tasks from the list
    completions      Generates shell completion files. This can be ignored during normal
                     operations
    edit             Edit the command or path of a stashed or queued task.
                     The command is edited by default.
    enqueue          Enqueue stashed tasks. They'll be handled normally afterwards
    follow           Follow the output of a currently running task. This command works like tail
                     -f
    format-status    Accept a list or map of JSON pueue tasks via stdin and display it just like
                     "status". A simple example might look like this: pueue status --json | jq
                     -c '.tasks' | pueue format-status
    group            Use this to add or remove groups. By default, this will simply display all
                     known groups
    help             Print this message or the help of the given subcommand(s)
    kill             Kill specific running tasks or whole task groups. Kills all tasks of the
                     default group when no ids are provided
    log              Display the log output of finished tasks. When looking at multiple logs,
                     only the last few lines will be shown. If you want to "follow" the output
                     of a task, please use the "follow" subcommand
    parallel         Set the amount of allowed parallel tasks. By default, adjusts the amount of
                     the default group
    pause            Either pause running tasks or specific groups of tasks.
                     By default, pauses the default group and all its tasks.
                     A paused queue (group) won't start any new tasks.
    remove           Remove tasks from the list. Running or paused tasks need to be killed first
    reset            Kill all tasks, clean up afterwards and reset EVERYTHING!
    restart          Restart task(s). Identical tasks will be created and by default enqueued.
                     By default, a new task will be created
    send             Send something to a task. Useful for sending confirmations such as 'y\n'
    shutdown         Remotely shut down the daemon. Should only be used if the daemon isn't
                     started by a service manager
    start            Resume operation of specific tasks or groups of tasks.
                     By default, this resumes the default group and all its tasks.
                     Can also be used force-start specific tasks.
    stash            Stashed tasks won't be automatically started. You have to enqueue them or
                     start them by hand
    status           Display the current status of all tasks
    switch           Switches the queue position of two commands. Only works on queued and
                     stashed commands
    wait             Wait until tasks are finished. This can be quite useful for scripting. By
                     default, this will wait for all tasks in the default group to finish. Note:
                     This will also wait for all tasks that aren't somehow 'Done'. Includes:
                     [Paused, Stashed, Locked, Queued, ...]
```

## Design Goals

Pueue is designed to be a convenient helper tool for a single user.

It's supposed to work stand-alone and without any external integration. The idea is to keep it simple and to prevent feature creep.

Hence, these features won't be included as they're out of scope:

-   Distributed task management/execution.
-   Multi-user task management.
-   Sophisticated task scheduling for optimal load balancing.
-   Tight system integration or integration with external tools.

There seems to be the need for some solution that satisfies all these points mentioned above, but that isn't Pueue's job.

## Similar Projects

**GNU Parallel**

A robust and featureful parrallel processor with text-based joblog and n-retries. [GNU Parallel](https://www.gnu.org/software/parallel/parallel_tutorial.html) is able to scale to multi-host parallelization and has complex code to have deep integration across different tools and shells, as well as other advanced features. `Pueue` differentiates itself from GNU Parallel by focusing more on visibility across many different long running commands, and creating a central location for commands to be stored, rather than GNU Parallel's focus on chunking a specific task.

**nq**

A very lightweight job queue systems which require no setup, maintenance, supervision, or any long-running processes.  
[Link to project](https://github.com/leahneukirchen/nq)

**task-spooler**

_task spooler_ is a Unix batch system where the tasks spooled run one after the other.  
Links to [ubuntu manpage](http://manpages.ubuntu.com/manpages/xenial/man1/tsp.1.html) and a [fork on Github](https://github.com/xenogenesi/task-spooler). The original website seems to be down.

## Contributing

Feature requests and pull requests are very much appreciated and welcome!

Anyhow, please talk to me a bit about your ideas before you start hacking! It's always nice to know what you're working on and I might have a few suggestions or tips :)

Depending on the type of your contribution, you should branch of from either the `main` branch or the `development` branch.

-   Bug fixes or critical library updates should branch of `main` and be merged into `main`. New patch level releases will be published for this kind of issues. Any patches in `main` will also regularily be merged into `development`.
-   Everything else, such as new features, refactorings, or breaking changes, should branch of `development` and be merged into `development`. Once a new minor or major version has been published, `development` will then be merged into `main`.

There's also the [Architecture Guide](https://github.com/Nukesor/pueue/blob/main/ARCHITECTURE.md), which is supposed to give you a brief overview and introduction to the project.

Copyright © 2019 Arne Beer ([@Nukesor](https://github.com/Nukesor))

[fig1]: https://github.com/nukesor/pueue/workflows/Test%20build/badge.svg
[fig2]: https://camo.githubusercontent.com/be71efc921a98a53f3b2ca9a02183a8a149519387cba64b41037361dcb035c1d/68747470733a2f2f696d672e736869656c64732e696f2f6372617465732f762f7075657565
[fig3]: https://camo.githubusercontent.com/78f47a09877ba9d28da1887a93e5c3bc2efb309c1e910eb21135becd2998238a/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f4c6963656e73652d4d49542d79656c6c6f772e737667
[fig4]: https://camo.githubusercontent.com/987c6ebccafbad64262514a3582e5babe15aa2f9065f4c3cc5796edaff488264/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f646f776e6c6f6164732f6e756b65736f722f70756575652f746f74616c2e737667
[fig5]: https://camo.githubusercontent.com/f24cd14c9e2ecea8d8790530b4f61de0ca422106c79b39e798445aa62be58b3e/68747470733a2f2f636f6465636f762e696f2f67682f6e756b65736f722f70756575652f6272616e63682f6d61696e2f67726170682f62616467652e737667
[fig6]: https://raw.githubusercontent.com/Nukesor/images/main/pueue-v2.0.0.gif
[fig7]: https://camo.githubusercontent.com/f5992935115cfff3ca93bd17b45c3be9f85337a20642e4b8f39546fe37627fa4/68747470733a2f2f7265706f6c6f67792e6f72672f62616467652f766572746963616c2d616c6c7265706f732f70756575652e737667

> Saved from https://github.com/Nukesor/pueue on 2023-04-16T15:03:34-05:00