---
title: Talon 0.2.1.0 documentation
pageTitle: Talon 0.2.1.0 documentation
length: 11117
author: 
timestamp: 2023-04-16T15:06:44-05:00
markdownload-tags: []
markdownload-source: https://talonvoice.com/docs/index.html#
markdownload-hostname: talonvoice.com
---

# Talon 0.2.1.0 documentation

## Excerpt
> Talon aims to bring programming, realtime video gaming, command line, and full desktop computer proficiency to people who have limited or no use of their hands, and vastly improve productivity and wow-factor of anyone who can use a computer.

---
## Introduction[¶](https://talonvoice.com/docs/index.html#introduction "Permalink to this headline")

## Overview[¶](https://talonvoice.com/docs/index.html#overview "Permalink to this headline")

Talon aims to bring programming, realtime video gaming, command line, and full desktop computer proficiency to people who have limited or no use of their hands, and vastly improve productivity and wow-factor of anyone who can use a computer.

[Join the Slack](https://talonvoice.com/chat) to talk, get hyped, or for help with Talon.

NOTE: This Talon release is very new and is not fully documented yet! Please ask any questions in the #help channel on the Slack linked above.

-   System requirements:
    
    -   macOS High Sierra (10.13) or newer. Talon is a universal2 build with native Apple Silicon support.
        
    -   Linux / X11 (Ubuntu 18.04+, and most modern distros), Wayland support is currently limited to XWayland
        
    -   Windows 8 or newer
        
    
-   Powerful voice control - Talon comes with a free speech recognition engine, and it is also compatible with Dragon with no additional setup.
    
-   Multiple algorithms for eye tracking mouse control (depends on a single Tobii 4C, Tobii 5 or equivalent eye tracker)
    
-   Noise recognition system (pop and hiss). Many more noises coming soon.
    
-   Scriptable with Python 3 (via embedded CPython, no need to install or configure Python on your host system).
    
-   Talon is very modular and adaptable - you can use eye tracking without speech recognition, or vice versa.
    

## Getting Started[¶](https://talonvoice.com/docs/index.html#getting-started "Permalink to this headline")

1.  Download and install [Talon](https://talonvoice.com/) for your operating system.
    
2.  Run the Talon app.
    
3.  Open the Talon Home directory. This is `%APPDATA%\Talon` on Windows, and `~/.talon` on macOS/Linux. (Talon has a menu in your system tray near the clock, you can use `Scripting -> Open ~/.talon` as a shortcut open Talon Home).
    

3.  Add some scripts to `~/.talon/user` to add voice commands and other behaviour to Talon (see the [Getting Scripts](https://talonvoice.com/docs/index.html#getting-scripts) section below). Your user scripts control all of the voice commands in Talon, so Talon won’t recognize any commands until you add some scripts.
    
4.  Install Conformer using Talon’s Speech Recognition menu.
    
5.  Go to `Scripting -> View Log` in the menu for debug output, or `Scripting -> Open REPL` for a Python command line.
    

## Getting Scripts[¶](https://talonvoice.com/docs/index.html#getting-scripts "Permalink to this headline")

The best way to get started right now is to clone [knausj\_talon](https://github.com/knausj85/knausj_talon) into your `~/.talon/user` directory. Ask in #help on Slack if you run into any problems.

## Learning Talon[¶](https://talonvoice.com/docs/index.html#learning-talon "Permalink to this headline")

These are some resources to help you learn to use and customize Talon:

-   [Talon Practice (by chaosparrot)](https://chaosparrot.github.io/talon_practice/)
    
-   [knausj\_talon README](https://github.com/knausj85/knausj_talon/blob/master/README.md)
    
-   [Unofficial Community Wiki](https://talon.wiki/)
    
-   [Search User Repositories](https://search.talonvoice.com/)
    

## .talon Files[¶](https://talonvoice.com/docs/index.html#talon-files "Permalink to this headline")

## Overview[¶](https://talonvoice.com/docs/index.html#overview "Permalink to this headline")

Voice commands are defined in files with the `.talon` file extension, located in the `user` directory inside [Talon Home](https://talonvoice.com/docs/index.html#getting-started).

`.talon` files can:

-   match specific applications, window titles, or other criteria
    
-   define voice commands
    
-   define global hotkeys (Mac-only at the moment)
    
-   reimplement global actions (for example, to change the behavior of Talon in a specific application)
    
-   set settings
    
-   enable tags
    

Creating a file in `user` named `hello.talon` with these contents will declare a voice command:

```
hello talon: "hello world"

```

This means when you say `hello talon`, Talon will type `hello world`.

This is a more advanced example:

```
# activate this .talon file if the current app name is "Chrome"
# you can find app names by running ui.apps() in the REPL
app.name: Chrome
-
# key_wait increases the delay when pressing keys (milliseconds)
# this is useful if an app seems to jumble or drop keys
settings():
    key_wait = 4.0

# activate the global tag "browser"
tag(): browser

# define some voice commands
hello chrome: "hello world"
switch tab: key(ctrl-tab)
go to google:
    # note: use key(cmd-t) on Mac
    key(ctrl-t)
    insert("google.com")
    key(enter)

```

The `-` on the line after after `app.name` is _important_.

-   Lines above the `-` are used to set criteria for activating the file.
    
-   Lines below the `-` declare things or activate tags.
    

Any line beginning with `#` is considered to be a comment and ignored.

## API Reference[¶](https://talonvoice.com/docs/index.html#module-talon "Permalink to this headline")

_class_ talon.Context(_name: str \= None_, _\*_, _desc: str \= None_)[¶](https://talonvoice.com/docs/index.html#talon.Context "Permalink to this definition")

Creating a Context:

```
from talon import Context
ctx = Context()

```

action\_class(_path: str_) → Callable\[\[talon.scripting.types.Class\], talon.scripting.actions.ActionClassProxy\][¶](https://talonvoice.com/docs/index.html#talon.Context.action_class "Permalink to this definition")

```
@ctx.action_class('prefix')
class Actions:
    def action_name():
        print("Running actions.prefix.action_name()")

```

action(_path: str_)[¶](https://talonvoice.com/docs/index.html#talon.Context.action "Permalink to this definition")

```
@ctx.action('prefix.action_name')
def action_name():
    print("Running actions.prefix.action_name()")

```

capture(_path: Optional\[str\] \= None_, _\*_, _rule: Optional\[str\] \= None_) → Callable\[\[talon.scripting.types.DecoratedT\], talon.scripting.types.DecoratedT\][¶](https://talonvoice.com/docs/index.html#talon.Context.capture "Permalink to this definition")

```
@ctx.capture('number', rule='(one | two)')
def number(m) -> int:
    return 1 if m[0] == 'one' else 2

```

_property_ matches_: Union\[str, talon.scripting.match.Match\]_[¶](https://talonvoice.com/docs/index.html#talon.Context.matches "Permalink to this definition")

Describe when to activate this Context. If not specified, Context is always active.

```
ctx.matches = r"""
os: windows
app.name: Slack
"""

```

Type

str

_property_ apps[¶](https://talonvoice.com/docs/index.html#talon.Context.apps "Permalink to this definition")

apps:

```
ctx.apps["chrome"] = r"""
os: windows
app.name: Google Chrome
"""

```

Type

str

_property_ lists_: dict\[str, Mapping\[str, str\]\]_[¶](https://talonvoice.com/docs/index.html#talon.Context.lists "Permalink to this definition")

lists:

```
ctx.lists["user.listname"] = ["word", "word2"]
ctx.lists["user.listname"] = {
    "pronunciation": "word",
}

```

Type

dict\[str, Union\[list\[str\], dict\[str, str\]\]\]

_property_ settings[¶](https://talonvoice.com/docs/index.html#talon.Context.settings "Permalink to this definition")

settings:

```
ctx.settings = {
    "input_wait": 1.0,
}

```

Type

dict\[str, Any\]

_property_ tags[¶](https://talonvoice.com/docs/index.html#talon.Context.tags "Permalink to this definition")

tags:

```
ctx.tags = ["user.terminal"]

```

Type

frozenset\[str\]

_property_ commands_: Mapping\[str, talon.scripting.types.CommandImpl\]_[¶](https://talonvoice.com/docs/index.html#talon.Context.commands "Permalink to this definition")

Return the commands defined by this Context

Type

dict\[str, CommandImpl\]

_property_ hotkeys_: Mapping\[str, talon.scripting.types.ScriptImpl\]_[¶](https://talonvoice.com/docs/index.html#talon.Context.hotkeys "Permalink to this definition")

Return the hotkeys defined by this Context

Type

dict\[str, ScriptImpl\]

_class_ talon.Module(_name: str \= None_, _\*_, _desc: str \= None_)[¶](https://talonvoice.com/docs/index.html#talon.Module "Permalink to this definition")

Creating a Module:

```
from talon import Module
mod = Module()

```

action\_class(_cls: talon.scripting.types.Class_) → talon.scripting.actions.ActionClassProxy[¶](https://talonvoice.com/docs/index.html#talon.Module.action_class "Permalink to this definition")

```
@mod.action_class
class Actions:
    def action_name():
        "Description of the action"

    def second_action(arg1: int, arg2: str='') -> str:
        "Action with arguments, return type, and body"
        return 'test'

```

action(_func: talon.scripting.types.DecoratedT_) → talon.scripting.types.ActionDecl\[talon.scripting.types.DecoratedT\][¶](https://talonvoice.com/docs/index.html#talon.Module.action "Permalink to this definition")

```
@mod.action
def action_name():
    "Description of the action"

```

capture(_\*_, _rule: str_) → Callable\[\[talon.scripting.types.DecoratedT\], talon.scripting.types.DecoratedT\][¶](https://talonvoice.com/docs/index.html#talon.Module.capture "Permalink to this definition")

capture(_func: talon.scripting.types.DecoratedT_) → talon.scripting.types.DecoratedT

```
@mod.capture
def capture_name() -> str:
    "Description of the capture"

```

scope(_func: ScopeFunc_) → ScopeDecl[¶](https://talonvoice.com/docs/index.html#talon.Module.scope "Permalink to this definition")

```
@mod.scope
def scope():
    return {
        "key": "value",
    }

# call scope.update() at any time to force a scope update

```

setting(_name: str, type: typing.Type\[talon.scripting.types.T\], default: typing.Union\[talon.scripting.types.T, talon.scripting.types.SettingDecl.NoValueType\] \= <talon.scripting.types.SettingDecl.NoValueType object>, desc: typing.Optional\[str\] \= None_) → talon.scripting.types.SettingDecl\[talon.scripting.types.T\][¶](https://talonvoice.com/docs/index.html#talon.Module.setting "Permalink to this definition")

```
mod.setting("setting_name", int, default=0, desc="an example integer setting")
mod.setting("setting_name_2", str, default='', desc="an example string setting")

```

list(_name: str_, _desc: Optional\[str\] \= None_) → talon.scripting.types.NameDecl[¶](https://talonvoice.com/docs/index.html#talon.Module.list "Permalink to this definition")

```
mod.list("list_name", desc="list description")

```

mode(_name: str_, _desc: Optional\[str\] \= None_) → talon.scripting.types.NameDecl[¶](https://talonvoice.com/docs/index.html#talon.Module.mode "Permalink to this definition")

```
mod.mode("mode_name", desc="mode description")

```

tag(_name: str_, _desc: Optional\[str\] \= None_) → talon.scripting.types.NameDecl[¶](https://talonvoice.com/docs/index.html#talon.Module.tag "Permalink to this definition")

```
mod.tag("tag_name", desc="tag description")

```

talon.actions[¶](https://talonvoice.com/docs/index.html#talon.actions "Permalink to this definition")

…

talon.registry[¶](https://talonvoice.com/docs/index.html#talon.registry "Permalink to this definition")

…

talon.scope[¶](https://talonvoice.com/docs/index.html#talon.scope "Permalink to this definition")

…

talon.settings[¶](https://talonvoice.com/docs/index.html#talon.settings "Permalink to this definition")

…

talon.storage[¶](https://talonvoice.com/docs/index.html#talon.storage "Permalink to this definition")

…

## talon.app[¶](https://talonvoice.com/docs/index.html#talon-app "Permalink to this headline")

talon.app.register(_topic: str_, _cb: Callable_) → None[¶](https://talonvoice.com/docs/index.html#talon.app.register "Permalink to this definition")

Register for an application event.

-   `ready`: Talon is ready. Your callback will be called after Talon launch and during script reloads.
    
-   `launch`: Talon launched. Your callback will only be called immediately after Talon launch.
    
-   `startup`: Talon launched during system startup.
    

```
from talon import app

def app_ready():
    print("Talon is ready")
app.register("ready", app_ready)

```

talon.app.unregister(_topic: Any_, _cb: Callable_) → None[¶](https://talonvoice.com/docs/index.html#talon.app.unregister "Permalink to this definition")

Unregister a previously registered event:

```
app.unregister("ready", app_ready)

```

talon.app.notify(_title: Optional\[str\] \= None_, _subtitle: Optional\[str\] \= None_, _body: Optional\[str\] \= None_, _sound: bool \= False_) → None[¶](https://talonvoice.com/docs/index.html#talon.app.notify "Permalink to this definition")

Display a desktop notification, optionally playing a sound.

```
from talon import app

app.notify(body="Hello world")
app.notify(title="Hello world",
           subtitle="Welcome to Talon",
           body="Enjoy your stay.",
           sound=True)

```

## talon.clip[¶](https://talonvoice.com/docs/index.html#talon-clip "Permalink to this headline")

talon.clip.has\_mode(_mode: Optional\[str\] \= None_) → bool[¶](https://talonvoice.com/docs/index.html#talon.clip.has_mode "Permalink to this definition")

Check if a clipboard mode is supported.

Useful modes: “main”, “select”, “find”

talon.clip.text(_\*_, _mode: Optional\[str\] \= None_) → Optional\[str\][¶](https://talonvoice.com/docs/index.html#talon.clip.text "Permalink to this definition")

Get the text contents of the clipboard.

talon.clip.set\_text(_s: str_, _\*_, _mode: Optional\[str\] \= None_) → None[¶](https://talonvoice.com/docs/index.html#talon.clip.set_text "Permalink to this definition")

Set the text contents of the clipboard.

talon.clip.image(_\*_, _mode: Optional\[str\] \= None_) → Optional\[talon.skia.image.Image\][¶](https://talonvoice.com/docs/index.html#talon.clip.image "Permalink to this definition")

Get the image contents of the clipboard.

talon.clip.set\_image(_image: talon.skia.image.Image_, _\*_, _mode: Optional\[str\] \= None_) → None[¶](https://talonvoice.com/docs/index.html#talon.clip.set_image "Permalink to this definition")

Set the image contents of the clipboard.

talon.clip.clear(_\*_, _mode: Optional\[str\] \= None_) → None[¶](https://talonvoice.com/docs/index.html#talon.clip.clear "Permalink to this definition")

Clear the clipboard.

_exception_ talon.clip.NoChange[¶](https://talonvoice.com/docs/index.html#talon.clip.NoChange "Permalink to this definition")

talon.clip.revert(_\*_, _old: talon.clip.MimeData \= None_, _mode: str \= None_) → Generator\[None, None, None\][¶](https://talonvoice.com/docs/index.html#talon.clip.revert "Permalink to this definition")

Restore the old text of the clipboard after running a block:

```
from talon import clip

with clip.revert():
    clip.set_text("this will only be set temporarily")

```

talon.clip.capture(_timeout: float \= 0.5_, _\*_, _inc: int \= 0_, _mode: str \= None_) → Generator\[talon.clip.ChangePromise, None, None\][¶](https://talonvoice.com/docs/index.html#talon.clip.capture "Permalink to this definition")

Capture a change in the clipboard, then restore the old text contents:

```
from talon import actions, clip

with clip.capture() as s:
    actions.edit.copy()
print(s.get())

```

## talon.fs[¶](https://talonvoice.com/docs/index.html#talon-fs "Permalink to this headline")

```
from talon import fs

def on_change(path, flags):
    if flags.renamed:
        print("renamed", path)
    if flags.exists:
        print("changed", path)
    else:
        print("deleted", path)

fs.watch('/path/to/stuff', on_change)

```

_class_ talon.fs.FsEventFlags(_exists: bool_, _renamed: bool_)[¶](https://talonvoice.com/docs/index.html#talon.fs.FsEventFlags "Permalink to this definition")

talon.fs.watch(_path: str_, _cb: Callable\[\[str, [talon.fs.FsEventFlags](https://talonvoice.com/docs/index.html#talon.fs.FsEventFlags "talon.fs.FsEventFlags")\], None\]_) → None[¶](https://talonvoice.com/docs/index.html#talon.fs.watch "Permalink to this definition")

Watch _path_ for changes and call `cb(path: str, flags: FsEventFlags)` when changes occur.

talon.fs.unwatch(_path: str_, _cb: Callable\[\[str, [talon.fs.FsEventFlags](https://talonvoice.com/docs/index.html#talon.fs.FsEventFlags "talon.fs.FsEventFlags")\], None\]_) → None[¶](https://talonvoice.com/docs/index.html#talon.fs.unwatch "Permalink to this definition")

Remove _cb_ from the set of callbacks being watched for _path_.

## talon.noise[¶](https://talonvoice.com/docs/index.html#talon-noise "Permalink to this headline")

talon.noise.register(_topic: Any_, _cb: Callable_) → None[¶](https://talonvoice.com/docs/index.html#talon.noise.register "Permalink to this definition")

Register for a noise event.

-   `""` - an empty string registers the callback for all noises.
    
-   `"pop"`
    
-   `"hiss"`
    

```
from talon import noise

def on_pop(active):
    print("pop")
noise.register("pop", on_pop)

def on_hiss(active):
    print("hiss", active)
noise.register("hiss", on_hiss)

```

talon.noise.unregister(_topic: Any_, _cb: Callable_) → None[¶](https://talonvoice.com/docs/index.html#talon.noise.unregister "Permalink to this definition")

Unregister a previously registered event:

```
noise.unregister("pop", on_pop)

```

> Saved from https://talonvoice.com/docs/index.html# on 2023-04-16T15:06:44-05:00