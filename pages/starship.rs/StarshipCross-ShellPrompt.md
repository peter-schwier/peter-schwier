---
title: Starship: Cross-Shell Prompt
pageTitle: Starship: Cross-Shell Prompt
length: 2589
author: 
timestamp: 2023-04-16T15:03:04-05:00
markdownload-tags: []
markdownload-source: https://starship.rs/
markdownload-hostname: starship.rs
---

# Starship: Cross-Shell Prompt

## Excerpt
> Starship is the minimal, blazing fast, and extremely customizable prompt for any shell! Shows the information you need, while staying sleek and minimal. Quick installation available for Bash, Fish, ZSH, Ion, Tcsh, Elvish, Nu, Xonsh, Cmd, and Powershell.

---
## Compatibility First

Works on the most common shells on the most common operating systems. Use it everywhere!

## Rust-Powered

Brings the best-in-class speed and safety of Rust, to make your prompt as quick and reliable as possible.

## Customizable

Every little detail is customizable to your liking, to make this prompt as minimal or feature-rich as you'd like it to be.

### [#](https://starship.rs/#prerequisites) Prerequisites

-   A [Nerd Font (opens new window)](https://www.nerdfonts.com/) installed and enabled in your terminal.

### [#](https://starship.rs/#quick-install) Quick Install

1.  Install the **starship** binary:
    
    #### [#](https://starship.rs/#install-latest-version) Install Latest Version
    
    With Shell:
    
    ```
    curl -sS https://starship.rs/install.sh | sh
    ```
    
    To update the Starship itself, rerun the above script. It will replace the current version without touching Starship's configuration.
    
    #### [#](https://starship.rs/#install-via-package-manager) Install via Package Manager
    
    With [Homebrew (opens new window)](https://brew.sh/):
    
    With [Winget (opens new window)](https://github.com/microsoft/winget-cli):
    
2.  Add the init script to your shell's config file:
    
    #### [#](https://starship.rs/#bash) Bash
    
    Add the following to the end of `~/.bashrc`:
    
    ```
    # ~/.bashrc
    
    eval "$(starship init bash)"
    ```
    
    #### [#](https://starship.rs/#fish) Fish
    
    Add the following to the end of `~/.config/fish/config.fish`:
    
    ```
    # ~/.config/fish/config.fish
    
    starship init fish | source
    ```
    
    #### [#](https://starship.rs/#zsh) Zsh
    
    Add the following to the end of `~/.zshrc`:
    
    ```
    # ~/.zshrc
    
    eval "$(starship init zsh)"
    ```
    
    #### [#](https://starship.rs/#powershell) Powershell
    
    Add the following to the end of `Microsoft.PowerShell_profile.ps1`. You can check the location of this file by querying the `$PROFILE` variable in PowerShell. Typically the path is `~\Documents\PowerShell\Microsoft.PowerShell_profile.ps1` or `~/.config/powershell/Microsoft.PowerShell_profile.ps1` on -Nix.
    
    ```
    Invoke-Expression (&starship init powershell)
    ```
    
    #### [#](https://starship.rs/#ion) Ion
    
    Add the following to the end of `~/.config/ion/initrc`:
    
    ```
    # ~/.config/ion/initrc
    
    eval $(starship init ion)
    ```
    
    #### [#](https://starship.rs/#elvish) Elvish
    
    WARNING
    
    Only elvish v0.18 or higher is supported.
    
    Add the following to the end of `~/.elvish/rc.elv`:
    
    ```
    # ~/.elvish/rc.elv
    
    eval (starship init elvish)
    ```
    
    #### [#](https://starship.rs/#tcsh) Tcsh
    
    Add the following to the end of `~/.tcshrc`:
    
    ```
    # ~/.tcshrc
    
    eval `starship init tcsh`
    ```
    
    #### [#](https://starship.rs/#nushell) Nushell
    
    WARNING
    
    This will change in the future. Only Nushell v0.73+ is supported.
    
    Add the following to the end of your Nushell env file (find it by running `$nu.env-path` in Nushell):
    
    ```
    mkdir ~/.cache/starship
    starship init nu | save -f ~/.cache/starship/init.nu
    ```
    
    And add the following to the end of your Nushell configuration (find it by running `$nu.config-path`):
    
    ```
    source ~/.cache/starship/init.nu
    ```
    
    #### [#](https://starship.rs/#xonsh) Xonsh
    
    Add the following to the end of `~/.xonshrc`:
    
    ```
    # ~/.xonshrc
    
    execx($(starship init xonsh))
    ```
    
    #### [#](https://starship.rs/#cmd) Cmd
    
    You need to use [Clink (opens new window)](https://chrisant996.github.io/clink/clink.html) (v1.2.30+) with Cmd. Add the following to a file `starship.lua` and place this file in Clink scripts directory:
    
    ```
    -- starship.lua
    
    load(io.popen('starship init cmd'):read("*a"))()
    ```

> Saved from https://starship.rs/ on 2023-04-16T15:03:04-05:00