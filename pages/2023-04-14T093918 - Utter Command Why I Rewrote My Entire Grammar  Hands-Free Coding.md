# Utter Command: Why I Rewrote My Entire Grammar | Hands-Free Coding

markdownload-timestamp:: 2023-04-14T09:39:18 (UTC -05:00)
markdownload-source:: https://handsfreecoding.org/2018/09/04/utter-command-why-i-rewrote-my-entire-grammar/
markdownload-hostname:: handsfreecoding.org
author:: 
tags:: Mouseless, Keyboardless, Accessibility



## Excerpt
> For years, I’ve been approaching speech recognition like a backend engineer: I have a flexible coding style for managing my grammars, I’ve implemented a lot of functionality, and I&#821…

---
For years, I’ve been approaching speech recognition like a backend engineer: I have a flexible coding style for managing my grammars, I’ve implemented a lot of functionality, and I’ve added some helpful integrations. But embarrassingly, until recently, I hadn’t put much thought into the User Experience. This all changed after I received an email from Kim Patch, the author of [Utter Command](http://redstartsystems.com/uttercommand "Utter Command"), a set of extensions to Dragon that has been around for decades.

Kim offered to talk over the phone, and we ended up chatting for over an hour. Kim was full of insightful observations how to design grammars. I quickly realized that my own grammars were a mess: a mishmash of scraps I picked up from Tavis Rudd’s video, commands I found in GitHub repositories, and plenty of stuff I had made up on my own. This mess was costing me in multiple ways. Adding a command to my grammar was always an ordeal, because I had to come up with some unique identifier (frequently a non-English word), check to make sure it didn’t conflict with anything, and then do my best to remember that word or phrase. In practice, this often meant recognition errors, or pauses as I tried to remember what I had used for my commands. All this discouraged me from adding more commands.

Kim’s work showed that there is a Better Way. I’m happy to say that she’s done a phenomenal job writing her ideas down, too, so you don’t have to all go find Kim’s phone number. I encourage you to explore her [website](http://redstartsystems.com/uttercommand "website"), but make sure you don’t miss [Human-Machine Grammar – The Rules](http://redstartsystems.com/human-machine-grammar-the-rules "Human-Machine Grammar – The Rules"). These 16 rules contain her biggest ideas — although it’s also very helpful to see exactly how she instantiates those ideas in Utter Command. In her own words:

> The 16 Human-Machine Grammar rules are aimed at keeping the speech interface vocabulary small and easy to remember and predict. These guidelines cut out alternate wordings and establish consistent patterns across the entire set of commands, making it much easier to remember or guess how a command should be worded.

This is well worth reading yourself, but I’ll provide my own notes and compare and contrast this with other grammar styles.

## The principles behind Utter Command

One of the striking aspects of Kim’s grammar is that it manages to be concise while still using English words. She takes inspiration from human-human communication systems, such as what you might hear at McDonald’s. You won’t hear “place an order for two cartons of fries”, nor will you hear something that sounds alien. Instead you are likely to hear “two fry”. It’s concise, easy to remember, and easy to extend (guess how you would request three hamburgers). Kim pays close attention to _cognitive load_ in the design of her grammar: how much thought you have to put into speaking commands. If you are using speech recognition to write code, you already have plenty to think about in the code that you are writing. If you also have to think about remembering an alien grammar on top of that, it’s going to detract from your focus on the code. Kim’s grammar minimizes that distraction. There are a couple principles I’ve found particularly useful:

1.  Keep the total number of words in your grammar as small as possible. Look for where you are using two or more slightly different words to mean basically the same thing, and pick one. To avoid reinventing the wheel, you can use the [Utter Command dictionary](http://redstartsystems.com/human-machine-grammar-dictionary "Utter Command dictionary").
2.  Pick a consistent word ordering. Many commands have you manipulating some object on the screen: closing a window, for example. In standard English, you would use the imperative “Close Window”, which has the order verb-object. Kim’s grammar standardizes on the order object-verb instead (“Window Close”), because it requires slightly less cognitive load once you get used to it (by the time you think of “close”, you’ve surely already had to think of the object you want to close, so you might as well say that first). This also means that your commands align better with the natural sequence in time, which is great for speaking commands that do multiple things in sequence. As an added bonus, because this word order doesn’t exactly match the standard English imperative style, it also means that the command is less likely to conflict with your prose dictation (otherwise a concern when using an English-based grammar).

## An alternative: ShortTalk

Kim’s grammar is not the only carefully thought-through grammar for speech recognition out there. The other major one that I’m aware of is [ShortTalk](http://shorttalk-emacs.sourceforge.net/ShortTalk/index.html "ShortTalk") by Nils Klarlund. This is the style that popularized alien-like words such as “spooce” and “nairx”, and certainly influenced my own early grammar design, as channeled through Tavis Rudd’s [famous video](https://www.youtube.com/watch?v=8SkdfdXWYaI "famous video"). I recommend reading about the grammar to form your own conclusions, but here are the reasons I ultimately decided to switch to Utter Command-style:

-   Your mileage may vary, but I found it took a long time to remember these commands, except for the ones that I used on a daily basis. My hesitation before speaking commands eliminated any speed advantage from having shorter commands.
-   While ShortTalk is a fairly extensive grammar, it doesn’t handle _all_ my needs, which left me needing to design my own commands for additional functionality — often requiring trial and error along with memorization.
-   Recently I’ve been experimenting with integrating Dragon alternatives with Dragonfly. The two options I’m most interested in are Google Speech API, because it’s production-ready and platform-agnostic, and Mozilla DeepSpeech, because it’s open-source. Both of these are unlike Dragon in that they don’t operate on a known grammar, so they aren’t going to recognize any of the ShortTalk words by default. The Google Speech API offers a very limited way to provide your own vocabulary in the form of a list of up to 500 unranked words and phrases, without any way to actually train these words. Hence, it is much easier to use these with a grammar that is based on English words.

## Installing Utter Command

The quickest way to get going with Utter Command is currently to [buy it from Kim’s website](http://redstartsystems.com/cgi-bin/appmain "buy it from Kim’s website"). Kim has [open-sourced](https://github.com/KimPatch/SpeechInput "open-sourced") some of the code, but it depends on DLLs that are not yet available.

If you choose to go this route, be aware that you need Dragon Professional (Dragon 15 Professional Individual will do fine). Interestingly, Utter Command is not implemented with NatLink, but instead uses a native language for specifying commands known as DVC. DVC commands can be simply added to files within `C:\ProgramData\Nuance\NaturallySpeaking15\Data\enx\dvcu\general` and automatically picked up (after a small tweak is made in `C:\ProgramData\Nuance\NaturallySpeaking15\nssystem.ini` to change `enx Base DVC Directory=dvce` to `enx Base DVC Directory=dvcu`).

I will note that some of the integration points with Windows and browsers have broken, so expect a bit of roughness around the edges. Kim is looking for help posting and improving the rest of her code, updating documentation, and making Utter Command open source. She is also open to porting the system to use something other than DVC, such as Dragonfly. If this is something you are interested in, please post in the comments and I can connect you.

## Implementing Utter Command style in Dragonfly

The upside of DVC is that it is very fast, but it’s also very complicated and verbose, so I’m not about to switch my Dragonfly grammar all over to DVC. Additionally, while there is some support for successive commands without pausing in Utter Command, it is all done in a one-off basis, typically for editing commands. I like to design my Dragonfly grammar so that I can make a whole subset of commands chainable. Since discovering Utter Command, I’ve been redesigning my grammar to try to get the best of both worlds: use the principles behind Utter Command where possible, but allow commands to flow from one to the other without pauses. Relearning my entire grammar hasn’t exactly been easy, but due to the simplicity of Utter Command style, it also hasn’t been nearly as painful as I would have expected. I’m already seeing dividends, where structuring my grammar more clearly has indicated gaps that I’ve filled, and I can already sense the decrease in cognitive load even though I’m still internalizing the commands. Here are some pieces of advice if you choose to go this route:

-   You don’t need a perfectly unambiguous grammar; you just need it to be unambiguous for the cases you actually use, which is a much weaker requirement. This is to say: don’t obsess over every possible way your grammar could theoretically go wrong or lead to an ambiguous recognition, other than following the advice in the next bullet.
-   Avoid one-word commands except for the most heavily-use cases. The problem is that once you’ve used a word in a one-word command, it’s very easy to create ambiguous commands if you ever reuse that word again. For example, I initially had “tab” simulate a press of the tab key, and “left” press the left key. When I was adding commands for my browser, I had “tab” as a prefix for several commands, and I kept bumping into misrecognition errors, e.g. I had a “tab left” command that was triggering “tab” and “left” instead. I could have changed that one command, but I ran into enough trouble with similar cases that I decided to change “tab” to “tab-key” (but I kept “left” as is).
-   Utter Command puts the number of repetitions for command first, e.g. “2 Down” instead of “Down 2”. I think this makes a lot of sense from a cognitive load standpoint, because counting requires some thought, so you want to speak the number as early as possible after that thought to release your mind to think about the rest of the command. The downside is that setting up a bunch of Dragonfly commands with numeric prefixes can lead to a massive slowdown if you do it wrong. Dragonfly’s IntegerRef expands into a fairly complicated mini-grammar and Dragon (apparently) resolves commands in a left-to-right fashion, leading to slowness when the two are combined. To solve this, I had to restructure my grammar so that I declared “<n>” one time instead of per-each-command — and then had that followed by an Alternative of all the commands to which it might apply. In general, it helps with grammar performance if you de-duplicate as much as possible, and this is a good example of that.

Utter Command also updates the natural text dictation commands to follow its style. You can easily do this yourself using the same method as Utter Command without pulling in any dependencies. Simply add the following to `C:\ProgramData\Nuance\NaturallySpeaking15\Users\<your_profile>\current\options.ini` under `[Options]`:

<table><tbody><tr><td><p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p><p>8</p><p>9</p><p>10</p><p>11</p><p>12</p><p>13</p><p>14</p><p>15</p><p>16</p><p>17</p><p>18</p><p>19</p><p>20</p><p>21</p></td><td><div><p><code>enx Correct XYZ Command=Nope %1</code></p><p><code>enx Correct That Command=Nope</code></p><p><code>enx Select XYZ Command=Words %1</code></p><p><code>enx Copy XYZ Command=Words Copy %1</code></p><p><code>enx Cut XYZ Command=Words Cut %1</code></p><p><code>enx Delete XYZ Command=Words Delete %1</code></p><p><code>enx Bold XYZ Command=Words Bold %1</code></p><p><code>enx Italicize XYZ Command=Words Italic %1</code></p><p><code>enx Underline XYZ Command=Words Underline %1</code></p><p><code>enx Select XYZ Through XYZ Command=Words %1 Through %1</code></p><p><code>enx Cut XYZ Through XYZ Command=Words Cut %1 Through %1</code></p><p><code>enx Copy XYZ Through XYZ Command=Words Copy %1 Through %1</code></p><p><code>enx Delete XYZ Through XYZ Command=Words Delete %1 Through %1</code></p><p><code>enx Bold XYZ Through XYZ Command=Words Bold %1 Through %1</code></p><p><code>enx Italicize XYZ Through XYZ Command=Words italic %1 Through %1</code></p><p><code>enx Underline XYZ Through XYZ Command=Words Underline %1 Through %1</code></p><p><code>enx Insert Before XYZ Command=Go Before %1</code></p><p><code>enx Insert After XYZ Command=Go After %1</code></p><p><code>enx Cap That Command=Add Caps</code></p><p><code>enx No Caps That Command=Add No-Caps</code></p><p><code>enx All Caps That Command=Add All-Caps</code></p></div></td></tr></tbody></table>

## Final thoughts

Redoing your grammar may seem like a daunting prospect, but if I did it after years of internalizing my old grammar, you can too! Please tell us about your experiences in the comments.

Finally, I want to give a huge thanks to Kim Patch for her incredible contributions in this area. Utter Command is a brilliant work and deserves more attention than it gets. It is very well-documented and I highly recommend that you explore [her website](http://redstartsystems.com/uttercommand "her website") and [list of Utter Command resources](https://docs.google.com/document/d/1CZNvIjI7vxTFdx0IrsKJLaFDKpJODCsMWD2Zh3JC5Rg/edit#heading=h.1pzhtagmesuo "list of Utter Command resources") to learn more.