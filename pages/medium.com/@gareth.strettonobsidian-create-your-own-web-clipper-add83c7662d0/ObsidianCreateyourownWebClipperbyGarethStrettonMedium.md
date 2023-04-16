---
title: Obsidian: Create your own Web Clipper - Gareth Stretton - Medium
pageTitle: Obsidian: Create your own Web Clipper | by Gareth Stretton | Medium
length: 13495
author: Gareth Stretton
timestamp: 2023-04-16T14:59:58-05:00
markdownload-tags: []
markdownload-source: https://medium.com/@gareth.stretton/obsidian-create-your-own-web-clipper-add83c7662d0
markdownload-hostname: medium.com
---

# Obsidian: Create your own Web Clipper | by Gareth Stretton | Medium

## Excerpt
> This article shares how to create your own Obsidian web clipper. It will allow you to send web content of your choosing to your Obsidian vault. Your clipper can be generic and work across all pages…

---
This article shares how to create your own Obsidian web clipper. It will allow you to send web content of your choosing to your Obsidian vault. Your clipper can be generic and work across all pages (e.g. send selected text to Obsidian), or it can be specialized and pull specific text from a specific page (e.g. actor names from IMDb).

This article is written so that anyone can follow along without getting lost. To that end, there are screenshots and detailed but easy-to-follow instructions.

Armed with this knowledge you’ll be able to capture content from the internet and send it to your vault in a layout of your preference!

## Sneak peek

Below are GIFs that show what we will create later in the article…

## How Does it Work?

The technique is a combination of the [Obsidian URI protocol](https://help.obsidian.md/Advanced+topics/Using+obsidian+URI) and [UserScripts](https://openuserjs.org/about/Userscript-Beginners-HOWTO).

**Obsidian URI Protocol**

The Obsidian URI protocol looks similar to a website address, however it instructs the browser to work with Obsidian. For example, the URI below will open Obsidian…

```
obsidian:
```

Try typing it in your address bar and press enter. Obsidian should open! (This capability was demonstrated in a [previous article](https://medium.com/@gareth.stretton/obsidian-control-from-chrome-1ba27c5ef84b).)

The protocol can be used to:

-   open your vault, or
-   perform a vault search, or
-   to create, overwrite, or append content to a note.

**UserScripts**

UserScripts allow you to make changes to pages that you visit. These changes are only visible to you. You can change the behavior by adding JavaScript, change the style by adding CSS, or change the content by adding HTML.

Most popular websites have custom UserScripts to improve the experience of visitors. For example, there is a UserScript to click ‘skip’ immediately on YouTube ads.

> WARNING!  
> UserScripts are written by people. Some people are evil and they write code that does naughty things. Before running a UserScript make sure you understand what it does! Otherwise, an evil person may do naughty things to you.

Here are a few websites where you can find UserScripts…

-   [https://userscripts-mirror.org/](https://userscripts-mirror.org/)
-   [https://greasyfork.org/](https://greasyfork.org/)

**JQuery**

JQuery is also utilized as it makes identifying, extracting, and modifying components of a web page very easy. It’s a JavaScript library that will help us out.

## Making a Clipper

Now that we have a basic understanding, we can get started. We will…

-   Install an extension that allows us to manage UserScripts.
-   Test that it works by running a simple example.
-   Build a generic clipper to send selected text to Obsidian.
-   Make a site-specific clipper that can extract key details from a page.
-   Explore different ways to activate the clipper.
-   And finally explore how to overcome a few pitfalls.

Let’s go…

**Install a UserScript extension to your browser**

The very first step is to install an extension to your browser that will let you manage UserScripts. I manage my UserScripts with [Tampermonkey](https://www.tampermonkey.net/). It is a free extension that works with Chrome, Firefox, and Edge. I would encourage you to support the developer who made it as this is an excellent tool! (Thanks Jan Biniok). If you use a different browser, [this website](https://openuserjs.org/about/Userscript-Beginners-HOWTO) lists other extensions you can use.

Here is a direct link to the [Chrome extension for Tampermonkey](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo?hl=en). The steps below show how to install it.

![][fig1]

Add to Chrome

![][fig2]

Confirm adding extension

![][fig3]

Pin for ease of access

**Test it works**

Let’s test that TamperMonkey works by creating a UserScript that will run when we visit Obsidian.md. Head on over to [https://obsidian.md](https://www.obsidian.md/) and open the Tampermonkey Dashboard.

![][fig4]

Visit Obsidian.md and open the Tampermonkey Dashboard

Next create a new script and paste the code below…

![][fig5]

Create new script and paste code

```
(function() {    'use strict';    alert("Hello from Tampermonkey!")})();
```

What does it do? Since you were already at the Obsidian.md website it used the web address for the `@match` configuration. This instructs Tampermonkey to only run this script on that website. You can ignore most of the other `@` configuration but for your own documentation you’ll typically want to put in a meaningful `@name` and `@description`. The code `alert("Hello from Tampermonkey!")` will display a popup dialog box when the script is run. This will happen each time you visit Obsidian.md.

Try it out, revisit [https://obsidian.md](https://www.obsidian.md/). You should see the popup.

![][fig6]

Success: Tampermonkey works!

If you got an alert, then Tampermonkey is working. If it isn’t working, try recreating the script — there may be a typo somewhere.

One thing to note in the image above is the red square with white text. This just says how many scripts were run. Nothing to worry about.

## **Generic Clipper: Send Selected Text to Your Vault**

The clipper script below will send selected text from (almost) any website and save it as a new file in your vault. (Be sure to change `my_vault` to the name of your vault!) If the file already exists, it will append the content to the end.

The script is activated when you press `CMD + I` on Mac, or `CTRL + I` on Linux or Windows. The filename will be a combination of the website domain and the title of the page (e.g. “medium.com - Digitial Felinology: A Beginner’s Guide”). Some characters can not be included in a file name, so those are removed.

```
function shortcutPressed(e) {    let operatingSystem = navigator.userAgent.toLowerCase().search("mac") !== -1 ? "mac" : "pc"    let activationKey = 'KeyI'    if (operatingSystem === "mac") {                return e.code === activationKey && e.metaKey && !e.altKey && !e.shiftKey && !e.ctrlKey;    } else {                return e.code === activationKey && e.ctrlKey && !e.altKey && !e.shiftKey && !e.metaKey;    }}function createFilename() {    let domainName = location.href.split("://")[1].split("/")[0]    let pageTitle = document.title    let filename = domainName + " - " + pageTitle    let cleanName = filename.replace(/[\(\)\[\]\\/?%*:'|"<>!]/g, '-');    return cleanName}document.addEventListener('keydown', e => {    if (shortcutPressed(e)) {        let vault = "my_vault"        let filename = createFilename()        let selectedText = document.getSelection().toString()        let encodedText = encodeURIComponent("\n" + selectedText)        let uri = `obsidian://new?vault=${vault}&name=${filename}&content=${encodedText}&append=true`        location.href = uri    }})
```

The animated GIF below demonstrates using the clipper on a Wikipedia article.

![][fig7]

Organizing Cat Research with the Custom Clipper

> Note: If you’re using Firefox…
> 
> You will want to choose another key (e.g. `let activationKey = ‘KeyE’`) as the letter ‘I’ is already in use (Thanks for letting me know, Koos Looijesteijn).

In the next section, we will make a custom clipper that will pull specific text from a specific page and save it to a new file. We’ll use JQuery to make it easier to locate the text to extract. I’ll also include some re-usable code for working with the wider Obsidian URI protocol.

## **Specific Clipper: Capturing Key Information from a Specific Site**

The code below will add an Obsidian logo to the page. When this logo is clicked then it will save the title, year, guidance, duration, and description to a new note under the “Movie” folder in your vault.

```
function visitObsidianURI(action, options) {    let uri = createObsidianURI(action, options)    location.href = uri.toString()}function createObsidianURI(action, options) {    let uri = `obsidian://${action}`    let firstProperty = true    for (const property in options) {        if (typeof options[property] === "undefined") {            continue        }        let delimiter = (firstProperty ? "?" : "&")        firstProperty = false        let propertyValue = encodeURIComponent(options[property])        uri += `${delimiter}${property}=${propertyValue}`    }    return uri;}let obsidianOpen = (options) => visitObsidianURI("open", options)let openVault = (vault) => obsidianOpen({vault})let openFile = (vault, file) => obsidianOpen({vault, file})let openPath = (path) => obsidianOpen({path})let obsidianSearch = (options) => visitObsidianURI("search", options)let search = (vault, query) => obsidianSearch({vault, query})let obsidianNew = (options) => visitObsidianURI("new", options)let createNote = (vault, name, content, silent = true) => obsidianNew({vault, name, content, silent: (silent ? silent : undefined)})let overwriteNote = (vault, name, content, silent = true) => obsidianNew({vault, name, content, overwrite: true, silent: (silent ? silent : undefined)})let appendNote = (vault, name, content, silent = true, onNewLine = true) => {    content = onNewLine ? "\n" + content : content    obsidianNew({vault, name, content, append: true, silent: (silent ? silent : undefined)})}function createFilename() {    let domainName = location.href.split("://")[1].split("/")[0]    let pageTitle = document.title    let filename = domainName + " - " + pageTitle    let cleanName = filename.replace(/[\(\)\[\]\\/?%*:'|"<>!]/g, '-');    return cleanName}(function() {    'use strict';    const vault = "my_vault"    const obsidianIcon = "https://www.google.com/s2/favicons?sz=64&amp;domain=obsidian.md"    let heroTitle = '[data-testid="hero-title-block__title"]';    let title =   $(heroTitle).text();    let baseSelector = heroTitle + ' + div > ul > li'    let year = $(baseSelector + ' > span:eq(0)').text()    let guidance = $(baseSelector + ' > span:eq(1)').text()    let duration = $(baseSelector + ':eq(2)').text()    let description = $('[data-testid="plot"]').text();    let content = `---tags: [ "movie" ]---# ${title}Year: ${year}Guidance: ${guidance}Duration: ${duration}${description}`    let filename = "Movies/" + createFilename()    let obsidianLink = createObsidianURI("new", {vault, file: filename, content})    let newElement = `<a href="${obsidianLink}"><img src="${obsidianIcon}" width="32" height="32"></a>`    $('[data-testid="hero-title-block__title"]').append(newElement)})();
```

The GIF below shows our custom clipper in action.

![][fig8]

(The above IMDb page is: [https://www.imdb.com/title/tt0062622](https://www.imdb.com/title/tt0062622))

**JQuery “Selectors”**

In the above code we identify elements of the page using ‘selectors’. For example “`[data-testid="here-title-block__title]'`”. We can then get the text inside this element by calling the `.text()` function.

A selector describes a part of the webpage. JQuery attempts to find that element using that description. The description could be a sequence of elements, e.g. 3 paragraphs in a row, or perhaps a single element, e.g. a paragraph with the ID of title.

See [JQuery’s documentation](https://api.jquery.com/) for good examples. Also, [w3school’s JQuery documentation](https://www.w3schools.com/jquery/jquery_ref_selectors.asp) is very easy to follow.

Some websites make it easy to pin-point the element you want (e.g. they give it a unique id). Other sites force you to be creative, for example, you may need to describe the relative path from a known location. It takes practice to master JQuery. I’d recommend skimming the documentation so you know what’s possible. Keep some notes on what worked for you too.

**How to Write Selectors**

Luckily it is not too difficult to write a selector. In fact, you can ask Chrome to show you the element by clicking on it. If you’re lucky, it will have an ID. And your code will be as simple as `let title = $(“#title”).text()`. If you’re not lucky, you’ll need to test out a few until you make one that works.

The process to create selectors is:

1\. Open the Chrome console …  
(Press `F12`, or `CMD + SHIFT + I` on Mac, or `CTRL + SHIFT + I` on Windows)

2\. Ask Chrome to show the element of interest in the ‘elements’ tab …  
Click on the icon that looks like a ‘mouse cursor pointing at a box’, then click on the text you want to extract. The HTML element will be highlighted in the ‘elements’ tab.

![][fig9]

3\. Ask Chrome to load JQuery …  
Open the ‘console’ tab, copy-and-paste this code and press enter.

```
var jq = document.createElement('script');jq.src = "https://ajax.googleapis.com/ajax/libs/jquery/3.6.1/jquery.min.js";document.getElementsByTagName('head')[0].appendChild(jq);jQuery.noConflict();
```

4\. Prototype your selectors in the Chrome console.

```
$('[data-testid="hero-title-block__title"]').text()
```

![][fig10]

When you have a selector that works, add it to your Tampermonkey script. It’s good practice to add one at a time, that way if your script breaks you can identify what change was responsible. Programming is finicky. Good luck!

## Other ways to Activate a UserScript

We’ve looked at activating a script by pressing a shortcut (i.e. `CMD + I`). We’ve also triggered a script by adding the Obsidian logo and clicking on it. Another way is to add a context menu item that will appear when you right click on the page. Let’s look at how to add this…

The new `@` configuration will allow you to create a context menu and run your script when the context menu is opened.

```

```

The code below demonstrates adding a new context menu item.

```
function demoOpenVault() {  location.href = 'obsidian://open'}GM_registerMenuCommand("Open Vault", demoOpenVault, "o");
```

Here’s what it looks like:

![][fig11]

## Pitfalls to Avoid

We’ve covered a lot, but I want to leave you with two final tips….

1.  Your browser may ask you to confirm before opening Obsidian. This confirmation check can be disabled. The steps are described in a [previous post](https://medium.com/@gareth.stretton/obsidian-control-from-chrome-1ba27c5ef84b) (see section: ‘Always allow Chrome to open Obsidian’)
2.  Some web pages use iframes. This makes identifying the selected text harder. Below is some code that will help you identify selected text from within an iframe…

```
let text = ""let iframe = document.getElementById("IFRAME_ID")let idoc = iframe.contentDocument || iframe.contentWindow.documentif (idoc.getSelection) {  text = idoc.getSelection().toString()}return text
```

## Wrapping up

I believe that I’m the first to combine UserScripts with Obsidian URI Protocol to make tailor-made web clippers! It’s exciting to help the Obsidian community gain a useful capability.

[fig1]: https://miro.medium.com/v2/resize:fit:700/1*xqc3zF0Gz5Ha2nQnr2L04g.png
[fig2]: https://miro.medium.com/v2/resize:fit:474/1*kxTKWyLvZDOvx9Jy9BFM7A.png
[fig3]: https://miro.medium.com/v2/resize:fit:350/1*zFCnM1vViJ1dec7_O0WLUg.png
[fig4]: https://miro.medium.com/v2/resize:fit:696/1*OVLcC-frX6uNE-R4YNv6CA.png
[fig5]: https://miro.medium.com/v2/resize:fit:700/1*c_gF5EGNXJEexsZrsRR5hg.png
[fig6]: https://miro.medium.com/v2/resize:fit:700/1*rycx4rdTKGCLnYNg8YAPpA.png
[fig7]: https://miro.medium.com/v2/resize:fit:700/1*QdEKD_vm0bb80Tvo056faw.gif
[fig8]: https://miro.medium.com/v2/resize:fit:700/1*yvtKLDZaZcV_zRTTNcW32A.gif
[fig9]: https://miro.medium.com/v2/resize:fit:700/1*DicDuiNn9mm9ruQOGasvqA.png
[fig10]: https://miro.medium.com/v2/resize:fit:700/1*sgJLN13yKYRBQQGj5c7XgA.png
[fig11]: https://miro.medium.com/v2/resize:fit:681/1*-WgIRQI0V_Py5SLbYi6BYg.png

> Saved from https://medium.com/@gareth.stretton/obsidian-create-your-own-web-clipper-add83c7662d0 on 2023-04-16T14:59:58-05:00