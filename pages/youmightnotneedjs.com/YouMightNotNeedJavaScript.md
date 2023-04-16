---
title: You Might Not Need JavaScript
pageTitle: You Might Not Need JavaScript
length: 11117
author: 
timestamp: 2023-04-16T14:47:26-05:00
markdownload-tags: []
markdownload-source: http://youmightnotneedjs.com/
markdownload-hostname: youmightnotneedjs.com
---

# You Might Not Need JavaScript

## Excerpt
> Examples of common UI elements and interactions with HTML and CSS alone.

---
JavaScript is great, and by all means use it, while also being aware that you can build so many functional UI components without the additional dependancy.

Maybe you can include a few lines of utility code, or a mixin, and forgo the requirement. If you're only targeting more modern browsers, you might not need anything more than what the browser ships with.

This site is fully copied from [youmightnotneedjquery.com](http://youmightnotneedjquery.com/), an excellent resource for vanilla JavaScript created by [@adamfschwartz](http://twitter.com/adamfschwartz) and [@zackbloom](http://twitter.com/zachbloom). But this time, we take a look at the power of modern native HTML and CSS as well as some of the syntactic sugar of Sass. Because, you might not need scripts for that task at all!

**(Note: Some of these demos may not be accessible and work on all browsers. Please take a moment to review the usage notes and to test them in your project before using in production)**

Your search didn't match any comparisons.

## Components

### [Image Slider](http://youmightnotneedjs.com/#image_slider)

#### CODEPEN

**Usage: Presentational, Option to read or be skipped by screen reader, no user control over images.**

#### HTML

```
<div id="slider">
  <div id="slide-holder">
    <div class="slide">content1</div>
    <div class="slide">content2</div>
    <div class="slide">content3</div>
  </div>
</div>
```

#### SCSS

```
#slider {
width: $slide-width;
height: $slide-height;
overflow: hidden;
}

.slide {
width: $slide-width;
height: $slide-height;
float: left;
position: relative;
}

#slide-holder {
  // wide enough to fit all the slides
width: 400%;
position: relative;
left: 0;
will-change: transform;
animation: scroller 10s infinite;
}

// need a step for each slide
@keyframes scroller {
  0% { transform: translateX(0); }
  33% { transform: translateX(-$slide-width); }
  66% { transform: translateX(-$slide-width*2); }
  100% { transform: translateX(0); }
}
```

### [Modal](http://youmightnotneedjs.com/#modal)

#### CODEPEN

**Usage: Safe for production with caveat (see scss) — Shoutout to [Marcy Sutton](https://twitter.com/marcysutton)**

#### HTML

```
<a class="modalTrigger" id="modalTrigger"
   href="#modalDialog1">
Open dialog
</a>

<dialog class="modal" id="modalDialog1"
tabindex="-1" role="dialog">
  <div>
  <a href="#modalTrigger" aria-label="Close">
  &times;
  </a>
  <p>Hello Beautiful!</p>
  </div>
</dialog>
```

#### SCSS

```
// Modal hidden until it matches an anchor target 
// Note: you can't close it with the escape key 
// or trap focus inside of it.
.modal {
  opacity: 0;
  visibility: hidden;
}

// modal fades in when it matches
// an anchor link (i.e. clicked on)
#modalDialog:target {
  opacity: 1;
  visibility: hidden;
}
```

### [View Switcher](http://youmightnotneedjs.com/#view_switcher)

#### CODEPEN

**Usage: Presentational: keyboard navigable, not screen reader accessible.**

#### HTML

```
<div id="slider">

  <!-- Slide Images -->
<img id="slide-1" src="img1.jpg" alt="img description"/>
  <img id="slide-2" src="img2.jpg" alt="img description"/>
  <img id="slide-3" src="img3.jpg" alt="img description"/>
  <img id="slide-4" src="img4.jpg" alt="img description"/>

<!-- Navigation for the slides -->
<ul>
<li><a href="#slide-1" aria-label="Image 1">1</a></li>
<li><a href="#slide-2" aria-label="Image 2">2</a></li>
<li><a href="#slide-3" aria-label="Image 3">3</a></li>
<li><a href="#slide-4" aria-label="Image 4">4</a></li>
</ul>
</div>
```

#### SCSS

```
#slider {
  img {
    position: absolute;
    top: 0;
    left: 0;
    width: 300px;
    height: 200px;
    
    &:target {
      transition: all .5s ease-in-out;
    }
    
    &:not(:target), 
    &:target ~ img#slide-4  {
      position: relative;
      opacity: 0;
    }

    // set initially visible
    &#slide-4 {
      position: absolute;
      opacity: 1;
     }
  }
}
```

## Forms

### [Color Picker](http://youmightnotneedjs.com/#color_picker)

#### CODEPEN

**Usage: Safe for production.**

#### HTML

```
<form>
  <input type="color" aria-label="Select a color" />
</form>
```

### [File Upload](http://youmightnotneedjs.com/#file_upload)

#### CODEPEN

**Usage: Safe for production.**

#### HTML

```
<form>
  <input type="file" accept="image/*" aria-label="select file to upload">
  <input type="submit">
</form>
```

### [Form Validation](http://youmightnotneedjs.com/#form_validation)

#### CODEPEN

**Usage: Safe for production.**

#### HTML

```
<!-- A few examples from links above -->

<form>
  <!-- Case insensitive binary choice -->
  <label for="item1">Would you prefer a banana or a cherry?</label>
  <input id="item1" pattern="[Bb]anana|[Cc]herry">

  <!-- Email validation -->
  <label for="item2">What's your e-mail?</label>
  <input id="item2" type="email" name="email">

  <!-- Max length validation -->
  <label for="item3">Leave a short message</label>
  <textarea id="item3" name="msg" maxlength="140" rows="5"></textarea>

  <!-- Numeric + Symbol pattern as required field -->
  <label for="item4">Phone Number (format: xxxx-xxx-xxxx):</label><br/>
  <input id="item4" type="tel" pattern="^\d{4}-\d{3}-\d{4}$" required >

  <button>Submit</button>
</form>
```

## Interactions

## Navigation

### [Accordion](http://youmightnotneedjs.com/#accordion)

#### CODEPEN

**Usage: Not keyboard navigable, inability to focus.**

#### HTML

```
<div class="togglebox">
  <input id="toggle1" type="radio" name="toggle" />
  <label for="toggle1">Label 1</label>
  <section id="content1">
    <p>Content for the first accordion</p>
  </section>
  
  <input id="toggle2" type="radio" name="toggle" />
  <label for="toggle2">Label 2</label>
  <section id="content2">
    <p>Content for the second accordion</p>
  </section>
  
  <input id="toggle3" type="radio" name="toggle" />
  <label for="toggle3">Label 3</label>
  <section id="content3">
    <p>Content for the third accordion</p>
  </section>
</div>
```

#### SCSS

```
// Visually hide radio buttons
input[type="radio"] {
  position: absolute;
  opacity: 0;

  &:focus + label {
    color: black;
    background-color: wheat;
  }
}

// Style label/entry for accordion
label {
  position: relative;
  display: block;
  cursor: pointer;
}

// Style accordion content
section {
  height: 0;
  transition: .3s all;
  overflow: hidden;
}

// Open content when clicking label
#toggle1:checked ~ #content1,
#toggle2:checked ~ #content2,
#toggle3:checked ~ #content3,
#toggle4:checked ~ #content4 {
  // set height for transition duration to work
  // (also can set to auto without transition)
  height: 200px;
}
```

### [Lightbox](http://youmightnotneedjs.com/#lightbox)

#### CODEPEN

**Usage: Not keyboard navigable.**

#### HTML

```
<!-- thumbnail image wrapped in a link -->
<a href="#img1">
  <img src="img.jpg" class="thumbnail">
</a>

<!-- lightbox container hidden with CSS -->
<a href="#" class="lightbox" id="img1">
  <img src="img.jpg">
</a>
```

#### SCSS

```
$thumbnail-size: 40px;
$background-color: black;

.thumbnail {
  max-width: $thumbnail-size;
}

.lightbox {
  // Hide lightbox image
display: none;

// Position/style of lightbox
position: fixed;
z-index: 999;
width: 100%;
height: 100%;
text-align: center;
top: 0;
left: 0;
background: $background-color;
}

.lightbox img {
/// Pad the lightbox image
max-width: 90%;
max-height: 80%;
margin-top: 2%;
}

.lightbox:target {
// Remove default outline and unhide lightbox
outline: none;
display: block;
}
```

### [Tabs](http://youmightnotneedjs.com/#tabs)

#### CODEPEN

**Usage: Not keyboard navigable.**

#### HTML

```
<div class="tabs">    
   <div class="tab">
       <input type="radio" name="tabgroup" id="tab-1" checked>
       <label for="tab-1">Label One</label>
       <div class="content">
          <p>Tab One Content</p>
       </div> 
   </div>
    
   <div class="tab">
       <input type="radio" name="tabgroup" id="tab-2">
       <label for="tab-2">Label Two</label>
       <div class="content">
           <p>Tab Two Content</p>
       </div> 
   </div>
    
    <div class="tab">
       <input type="radio" name="tabgroup" id="tab-3">
       <label for="tab-3">Label Three</label>
       <div class="content">
           <p>Tab Three Content</p>
       </div> 
   </div>
</div>
```

#### SCSS

```
.tabs {
  position: relative;  
  // set height to be even for all tab groups
  min-height: 200px;
}

.tab {
  float: left;
}

.tab label {
  // set label height
  top: 1em;
}

// Visually hide radio buttons
.tab [type=radio] {
  position: absolute;
  height: 0;
  width: 0;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  
  &:focus + label {
    outline: 2px dotted black;
  }
}

.content {
  position: absolute;
  top: 1em; left: 0; right: 0; bottom: 0;
  opacity: 0;
}

[type=radio]:checked ~ label {
  z-index: 2;
}

[type=radio]:checked ~ label ~ .content {
  z-index: 1;
  opacity: 1;
}
```

> Saved from http://youmightnotneedjs.com/ on 2023-04-16T14:47:26-05:00