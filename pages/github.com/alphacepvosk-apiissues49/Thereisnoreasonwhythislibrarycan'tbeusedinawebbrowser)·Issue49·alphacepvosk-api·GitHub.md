---
title: There is no reason why this library can't be used in a web browser :) · Issue #49 · alphacep/vosk-api
pageTitle: There is no reason why this library can't be used in a web browser :) · Issue #49 · alphacep/vosk-api · GitHub
length: 2148
author: alphacep
timestamp: 2023-04-16T15:09:28-05:00
markdownload-tags: []
markdownload-source: https://github.com/alphacep/vosk-api/issues/49
markdownload-hostname: github.com
---

# There is no reason why this library can't be used in a web browser :) · Issue #49 · alphacep/vosk-api · GitHub

## Excerpt
> I have a demo of the WebAssembly version available here: https://dtreskunov.github.io/tiny-kaldi/ Works pretty well, actually. Also, check out how small the binary is!

---
I've now packaged it as WebComponent that allows you to easily use it on your own site.

Try it out by pasting the following into Developer Console on a site that doesn't have a restrictive Content Security Policy (such as [https://developer.mozilla.org](https://developer.mozilla.org/)):

```js
(function() {

    const scriptElement = document.createElement('script')
    scriptElement.type = 'text/javascript'
    scriptElement.src = 'https://dtreskunov.github.io/tiny-kaldi/VoskJS.js'
    document.querySelector('head').appendChild(scriptElement)
    const recognizerElement = document.createElement('vosk-recognizer')
    recognizerElement.addEventListener('vosk-recognizer-result', ({detail}) => {
        if (!detail.text) return
        console.log(detail)
    })
    recognizerElement.setAttribute('model-url', 'model-en.tar.gz')
    recognizerElement.setAttribute('oneshot', true)
    recognizerElement.style['background-color'] = '#fff'
    recognizerElement.style['border-radius'] = '5px'
    recognizerElement.style['border'] = '1px solid #aaa'
    recognizerElement.style['top'] = '50%'
    recognizerElement.style['font-family'] = ' sans-serif'
    recognizerElement.style['left'] = '50%'
    recognizerElement.style['padding'] = '.5rem'
    recognizerElement.style['position'] = 'fixed'
    recognizerElement.style['transform'] = 'translate(-50%, -50%)'

    document.querySelector('body').appendChild(recognizerElement)
})()
```

```html
<!doctype html>
<html>
<style>
vosk-recognizer {
    --font-size: 2rem;
    background-color: #fff;
    border-radius: 5px;
    border: 1px solid #aaa;
    font-family: sans-serif;
    left: 50%;
    padding: .5rem;
    position: fixed;
    top: 50%;
    transform: translate(-50%, -50%);
}
</style>
<vosk-recognizer model-url="model-en.tar.gz" oneshot></vosk-recognizer>
<script src="https://dtreskunov.github.io/tiny-kaldi/VoskJS.js"></script>
</html>
```

You can even specify your own model by changing the `model-url` attribute. Download one of the models listed on [this page](https://github.com/alphacep/vosk-api/blob/master/doc/models.md) and serve it using a web server that sends `Access-Control-Allow-Origin: *` response header. For example, 'http-server --cors -p 8000' (install using 'npm install -g http-server').

> Saved from https://github.com/alphacep/vosk-api/issues/49 on 2023-04-16T15:09:28-05:00