---
title: How to use autocomplete at the command line for dotnet, git, winget, and more!
pageTitle: How to use autocomplete at the command line for dotnet, git, winget, and more! - Scott Hanselman's Blog
length: 3877
author: 
timestamp: 2023-04-16T15:00:09-05:00
markdownload-tags: [Open Source]
markdownload-source: https://www.hanselman.com/blog/
markdownload-hostname: www.hanselman.com
---

# How to use autocomplete at the command line for dotnet, git, winget, and more! - Scott Hanselman's Blog

## Excerpt
> Many years ago .NET Core added Command line 'tab' completion for .NET Core CLI ...

---
Many years ago .NET Core added [Command line "tab" completion for .NET Core CLI in PowerShell or bash](https://www.hanselman.com/blog/CommandLineTabCompletionForNETCoreCLIInPowerShellOrBash.aspx) but few folks have taken the moment it takes to set up.

I enjoy setting up and [making my prompt/command line/shell/terminal experience](https://www.hanselman.com/blog/HowToUseOpenResizeAndSplitPanesInTheWindowsTerminal.aspx) as useful (and [pretty](https://www.hanselman.com/blog/HowToMakeAPrettyPromptInWindowsTerminalWithPowerlineNerdFontsCascadiaCodeWSLAndOhmyposh.aspx)) as possible. You have lots of command line shells to choose from on Windows!

Keep in mind these are SHELLs not terminals or alternative consoles. You can run all of these in the [Windows Terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701?WT.mc_id=-blog-scottha).

-   Classic cmd.exe command prompt (fake DOS!) - Still useful and [Clink](http://mridgers.github.io/clink/) can make it more bash-y without going full bash.
-   [Yori](https://www.hanselman.com/blog/YoriTheQuietLittleCMDReplacementThatYouNeedToInstallNOW.aspx) from Malcolm Smith
-   [Starship](https://starship.rs/) - more on this later, it's somewhat unique
-   Windows PowerShell - a classic because...
-   [PowerShell 7](https://docs.microsoft.com/en-us/powershell/scripting/whats-new/what-s-new-in-powershell-70?view=powershell-7&WT.mc_id=-blog-scottha#where-can-i-install-powershell) is out and runs literally anywhere, including ARM machines!

I tend to use PowerShell 7 (formerly PowerShell Core) as my main prompt because it's a cross-OS prompt. I can use the same prompt, same functions, same everything on Windows and Linux.

[![][fig1]](https://www.hanselman.com/blog/HowToMakeAPrettyPromptInWindowsTerminalWithPowerlineNerdFontsCascadiaCodeWSLAndOhmyposh.aspx)

But it's command-line autocompletion that brings me the most joy!

-   git ch<TAB> -> git checkout st<TAB> -> git checkout staging
-   dotnet bu<TAB> -> dotnet build
-   dotnet --list-s<TAB> -> dotnet --list-sdks
-   winget in<TAB> -> winget install -> winget install WinDi<TAB> -> winget install WinDirStat

Once you have successfully tab'ed you way to glory it's hard to stop. With PowerShell and its cousins this is made possible with Register-ArgumentCompleter. Here's what it looks like for the [dotnet](http://www.dot.net/?WT.mc_id=-blog-scottha) CLI.

`Register``-ArgumentCompleter` `-Native` `-CommandName` `dotnet` `-ScriptBlock` `{`

    `param(``$commandName``,` `$wordToComplete``,` `$cursorPosition``)`

        `dotnet complete -``-position` `$cursorPosition` `"$wordToComplete"` `|` `ForEach-Object` `{`

           `[System.Management.Automation.CompletionResult]::new($_, $_,` `'ParameterValue'``, $_)`

        `}`

`}`

Looks like a lot, but the only part that matters is that when it sees the command "dotnet" and some partial text and the user presses TAB, it will call "dotnet complete" passing in the cursorPosition and the wordToComplete.

> **NOTE:** If you understand how this works, you can easily make your own Argument Completer for those utilities that you use all the time at work! You can make them for the folks at work who use your utilities!

You never actually see this call to "dotnet complete." You just see yourself typing dotnet bui<TAB> and getting a series of choices to tab through!

Here's what happens behind the scenes:

```
>dotnet complete --position 3 buibuildbuild-servermsbuild
```

You can add these to your $profile. Usually I run 'notepad $profile" at the command line and it will autocreate the correct file in the correct location.

This is a super powerful pattern! You can get [autocomplete in Git in PowerShell](https://git-scm.com/book/ms/v2/Appendix-A%3A-Git-in-Other-Environments-Git-in-Powershell) with [PoshGit](https://github.com/dahlbyk/posh-git?WT.mc_id=-blog-scottha) as well as in [WinGet](https://github.com/microsoft/winget-cli/blob/master/doc/Completion.md?WT.mc_id=-blog-scottha)!

What are some more obscure autocompletes that you have added to your PowerShell profile?

> **ACTION:** Finally, please take a moment and [subscribe to my YouTube](https://www.youtube.com/shanselman?sub_confirmation=1) or head over to [http://computerstufftheydidntteachyou.com](http://computerstufftheydidntteachyou.com/) and explore! I'd love to hit 100k subs over there. I heard they give snacks.

___

**Sponsor:** Suffering from a lack of clarity around software bugs? Give your customers the experience they deserve and expect with [error monitoring](https://hnsl.mn/2F7ZeQ5) from Raygun.com. Installs in minutes, try it today!

#### About Scott

Scott Hanselman is a former professor, former Chief Architect in finance, now speaker, consultant, father, diabetic, and Microsoft employee. He is a failed stand-up comic, a cornrower, and a book author.

[![facebook][fig2]](https://facebook.com/shanselman) [![twitter][fig3]](https://twitter.com/shanselman) [![subscribe][fig4]](http://feeds.hanselman.com/ScottHanselman)  
[About](http://hanselman.com/about)   [Newsletter](http://www.hanselman.com/newsletter)

**Hosting By**  
[![Hosted in an Azure App Service][fig5]](https://azure.microsoft.com/free?WT.mc_id=-blog-scottha)

[fig1]: https://hanselmanblogcontent.azureedge.net/Windows-Live-Writer/63963d6f2af3_12BCC/image_e2447ddd-416e-4036-9584-e728455e6d9d.png
[fig2]: https://images.hanselman.com/main/icon-fb.png
[fig3]: https://images.hanselman.com/main/icon-twitter.png
[fig4]: https://images.hanselman.com/main/icon-rss.png
[fig5]: https://images.hanselman.com/main/azure-250x250.png

> Saved from https://www.hanselman.com/blog/ on 2023-04-16T15:00:09-05:00