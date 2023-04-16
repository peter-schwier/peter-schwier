---
title: ~geb/numen - Voice control for handsfree computing
pageTitle: ~geb/numen - Voice control for handsfree computing - sourcehut git
length: 2070
author: 
timestamp: 2023-04-16T14:35:49-05:00
markdownload-tags: []
markdownload-source: https://git.sr.ht/~geb/numen
markdownload-hostname: git.sr.ht
---

# ~geb/numen - Voice control for handsfree computing - sourcehut git

## Excerpt
> Numen is voice control for computing without a keyboard or mouse,
and works system-wide on your Linux machine.

---
Numen is voice control for computing without a keyboard or mouse, and works system-wide on your Linux machine.

There's a short demonstration on: [https://numenvoice.org](https://numenvoice.org/)

### [#](https://git.sr.ht/~geb/numen#install-from-source)Install From Source

`go` is required. (It's sometimes packaged as `golang`)

The [speech recognition library](https://alphacephei.com/vosk) and an English model (about 40MB) can be installed with:

```
sudo ./install-vosk.sh && sudo ./install-model.sh
```

The [dotool](https://sr.ht/~geb/dotool) command which simulates the input, can be installed with:

```
sudo ./install-dotool.sh
```

Finally, `numen` itself can be installed with:

```
sudo ./install-numen.sh
```

### [#](https://git.sr.ht/~geb/numen#permission)Permission

`dotool` requires permission to `/dev/uinput` to create the virtual input devices, and a udev rule grants this to users in group input.

You could try:

```
echo type hello | dotool
```

and if need be, you can run:

```
sudo groupadd -f input
sudo usermod -a -G input $USER
```

and re-login and trigger the udev rule or just reboot.

### [#](https://git.sr.ht/~geb/numen#getting-started)Getting Started

Once you've got a microphone, you can run it with:

```
numen
```

There shouldn't be any output but you should be able to type "hey" by saying "hoof eve yank" and transcribe a sentence after saying "scribe". You can terminate it by pressing Ctrl+c which is "troll cap".

If nothing happened, check it's using the right audio device with:

```
timeout 5 numen --verbose --audiolog=me.wav
aplay me.wav
```

and specify a `--mic` from `--list-mics` if not.

Now you can have a go in your text editor, the default phrases are in the `/etc/numen/phrases` directory.

### [#](https://git.sr.ht/~geb/numen#going-further)Going Further

I use numen for all my computing and stick to keyboard-based programs like [Neovim](https://neovim.io/) and [qutebrowser](https://qutebrowser.org/), my text editor and browser. I also use a minimal desktop environment I wrote called [Tiles](https://git.sr.ht/~geb/tiles) that doesn't require a pointer device.

### [#](https://git.sr.ht/~geb/numen#contact-and-matrix-chat)Contact and Matrix Chat

You can send questions, thoughts or patches by composing an email to [~geb/public-inbox@lists.sr.ht](https://lists.sr.ht/~geb/public-inbox).

You're also welcome to join our Matrix chat at [#numen:matrix.org](https://matrix.to/#/#numen:matrix.org).

### [#](https://git.sr.ht/~geb/numen#see-also)See Also

-   [Tiles](https://git.sr.ht/~geb/tiles) - A minimal desktop environment suitable for voice control.
-   [Hiccup](https://git.sr.ht/~geb/hiccup) - Noise input for playing games.

### [#](https://git.sr.ht/~geb/numen#support-me)Support Me

[Thank you!](https://liberapay.com/geb)

### [#](https://git.sr.ht/~geb/numen#license)License

GPLv3 only, see [LICENSE](https://git.sr.ht/~geb/numen/tree/master/LICENSE).

Copyright (c) 2022-2023 John Gebbie

> Saved from https://git.sr.ht/~geb/numen on 2023-04-16T14:35:49-05:00