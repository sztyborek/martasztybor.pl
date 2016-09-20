---
layout:     post
title:      Useful CSS Features You May Have Not Known About
date:       2016-09-19 14:00:00
summary:    At work I'm sometimes surprised when I discover a CSS feature that enables me to use some clever trick. Some of these features are brand new and still not widely supported (hello, polyfills!), others are pretty old but not commonly known. Also, if you're anything like me and prefer digging for pure CSS solutions rather than coding in JavaScript, you may find here a couple of hints that will make your life (as a developer) easier.
categories: CSS
image: /img/skate.jpg
---

At work I'm sometimes surprised when I discover a CSS feature that enables me to use some clever trick. Some of these features are brand new and still not widely supported (hello, polyfills!), others are pretty old but not commonly known.

Also, if you're anything like me and prefer digging for "pure CSS" solutions rather than coding in JavaScript, you may find here a couple of hints that will make your life (as a developer) easier.

- [Dynamically generating counters using only CSS](./#css-counters)
- [Internal links and pure CSS lightboxes](./#pure-css-lightbox)
- [JavaScript-less sticky menus](./#pure-css-sticky-menus)
- [Accessing the HTML attributes' values using CSS](./#attr-function)
- [The `currentColor` property](./#currentcolor-property)
- [CSS feature queries](./#css-feature-queries)

<h2 id="css-counters">Dynamically generating counters using only CSS</h2>

Assume we have a collection of items represented by HTML elements. We want them to be ordered and displayed with proper numbers. First thing that comes to mind is to mark them up as ordered list (`ol`), and the browser will do the job for us.

{% highlight html %}
<ol>
  <li>Tidy the room</li>
  <li>Buy groceries</li>
  <li>Write blogpost</li>
</ol>
{% endhighlight %}

<div class="example">
  <ol>
    <li>Tidy the room</li>
    <li>Buy groceries</li>
    <li>Write blogpost</li>
  </ol>
</div>

So far, so good. We have semantic HTML and proper presentation. But what if we want to display the list with nested numbers? Or want to change the default look of numbers to fit into our designs? Oh yes, it's possible to do it by hand, but

- it's tedious,
- won't work if the content is generated dynamically.

It's possible to manipulate our HTML with JavaScript, but we are lazy developers, aren't we? We want to use some pure CSS solution, which will be simpler and more performant. Say "hello" to CSS counters, which allow you to dynamically add order numbers to your lists and generated content.

> CSS counters are, in essence, variables maintained by CSS whose values may be incremented by CSS rules to track how many times they're used. This lets you adjust the appearance of content based on its placement in the document. 
>
> -- <cite>[MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Lists_and_Counters/Using_CSS_counters)</cite>

To start generating numbers with CSS counters we need to know how to do the following:

- initialize the counter,
- increment the counter.

We initialize the counter for a parent element, which will contain our list of elements. It can be done with a `counter-reset` property. 

{% highlight css %}
ol {
  counter-reset: counter-name;
}
{% endhighlight %}

We need to provide a name for our counter to identify it. It works like a variable name, which we set as a `counter-reset` property value.

Then we can increment our counter using `counter-increment` property:

{% highlight css %}
ol > li {
  counter-increment: counter-name;
}
{% endhighlight %}

Alright, so we're all set. Now we need to replace the default numbers of the ordered list and make them more stylish. It's possible with the use of pseudoelements and `content` property.

{% highlight css %}
ol {
  list-style-type: none;  
}

ol > li::before {
  counter-increment: counter-name;
  content: counter(counter-name);
}
{% endhighlight %}

We can successfuly use counters not only for lists, but also for document sections (marking chapters with numbers, like in the codepen below) and other types of elements.

<figure>
  <pre class="codepen" data-height="470" data-type="result" data-href="RRxRbQ" data-user="sztyborek" data-safe="true"><code></code><a href="http://codepen.io/sztyborek/pen/RRxRbQ">Auto numbering chapters with CSS counters: check it out on Codepen</a></pre>
  <script async src="http://codepen.io/assets/embed/ei.js"></script>
  <figcaption>Auto numbering chapters with CSS counters</figcaption>
</figure>

### Browser support

In fact CSS counters exist for a pretty long time in the spec, so the browser support is awesome.

| Chrome | Firefox | Safari | Internet Explorer/Edge | Opera |
| ------ | ------- | ------ | ---------------------- | ----- |
| Any    | Any     | Any    | 8+                     | Any   |

### Resources and further reading

- [Pure CSS games with `counter-increment`](https://una.im/css-games/) by Una Kravets
- [MDN article about using CSS counters](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Lists_and_Counters/Using_CSS_counters)
- About [CSS counters and generated content](https://www.smashingmagazine.com/2013/04/css-generated-content-counters/) on Smashing Magazine
- [More practical examples of CSS counters usage](https://scotch.io/tutorials/getting-started-with-css-counters)

<h2 id="pure-css-lightbox">Internal links and pure CSS lightboxes</h2>

With HTML, it's possible to link not only to external resources, but also to certain sections of a document. Such links are called the <i>anchor links</i>.

{% highlight html %}
<a href="#useful-css-properties">Useful CSS properties</a>

<h1 id="useful-css-properties">Useful CSS properties</h1>
{% endhighlight %}

They are defined by a reference to an element's `id` attribute. We can enhance the experience of such kind of navigation using only CSS. There is a `:target` pseudoclass, which can be used to select and style internal links in the document. 

<figure>
  <pre class="codepen" data-height="470" data-type="result" data-href="xEVxAK" data-user="sztyborek" data-safe="true"><code></code><a href="http://codepen.io/sztyborek/pen/xEVxAK">Yellow fade technique demo: check it out on Codepen</a></pre>
  <script async src="http://codepen.io/assets/embed/ei.js"></script>
  <figcaption>The :target pseudoclass in use</figcaption>
</figure>

The example above is very simple and not very surprising. If you are waiting for something that will blow your mind or just make you think "oh, why I've been doing that using JavaScript for all these years" – there is indeed one clever trick involving `:target` pseudoclass.

We can create simple lightboxes without a single line of JavaScript. Here is the markup for our elements:

{% highlight html %}
<a href="#about-me">About me</a>

<div id="about-me" class="lightbox">
  <a href="#" class="close-me"></a>
  <p>
    Lightbox content
  </p>
</div>
{% endhighlight %}

We need to create a link to an existing section in our document. Clicking on this link should cause the lightbox to appear and apart from that we should also be able to close it. The magic here belongs to CSS.

Our lightbox should be initially hidden. We can apply `display: none` to the `.lightbox` element, but to make the effect more visually appealing we can enhance it with CSS transforms.

{% highlight css %}
.lightbox {
  position: fixed;
  transform: scale(0);
  transition: transform .3s ease-in;
}
{% endhighlight %}

It's also very important to apply `position: fixed` to remove our lightbox from the document flow to make it look and behave exactly like a lightbox.

The lightbox's `id` value should be the same as the `href` attribute value of the link. Then the following lines of CSS are responsible for opening the lightbox:

{% highlight css %}
.lightbox:target {
  transform: scale(1);
}
{% endhighlight %}

<figure>
  <pre class="codepen" data-height="470" data-type="result" data-href="ORNPRo" data-user="sztyborek" data-safe="true"><code></code><a href="http://codepen.io/sztyborek/pen/ORNPRo">Pure CSS lightbox: check it out on Codepen</a></pre>
  <script async src="http://codepen.io/assets/embed/ei.js"></script>
  <figcaption>Pure CSS lightbox example</figcaption>
</figure>

### Browser support

| Chrome | Firefox | Safari | Internet Explorer/Edge | Opera |
| ------ | ------- | ------ | ---------------------- | ----- |
| Any    | 3.5+    | 3.2+   | 7+                     | 10.1+ |

### Resources and further reading

- [Article on MDN documenting the `:target` pseudoclass](https://developer.mozilla.org/en-US/docs/Web/CSS/:target)
- [Using The CSS `:target` Selector To Create JavaScript-less UI Effects](http://blogs.adobe.com/dreamweaver/2015/01/using-the-css-target-selector-to-create-javascript-less-ui-effects.html) by Sara Soueidan
- [A simple image gallery using only CSS and the `:target` selector](https://hacks.mozilla.org/2012/02/a-simple-image-gallery-using-only-css-and-the-target-selector/) by Christian Heilmann

<h2 id="pure-css-sticky-menus">JavaScript-less sticky menus</h2>

Sticky navigation menus are quite popular in contemporary web design. Creating a fixed menu and changing its `position` when scrolling the document is feasible with a couple of lines of JavaScript. But if there is such demand for a certain design pattern implementation, shouldn't it land in CSS specification soon?

In fact, there is a possibility to create a sticky menu with just a single line of CSS. Unfortunately, for now it only exists as an experimental feature (you can find the [draft](https://drafts.csswg.org/css-position/#sticky-positioning) in the spec).

{% highlight css %}
.menu {
  position: sticky;
}
{% endhighlight %}

Turn the "experimental" flag in your browser and fasten your seatbelts!

<figure>
  <pre class="codepen" data-height="470" data-type="result" data-href="WGwQbk" data-user="sztyborek" data-safe="true"><code></code><a href="http://codepen.io/sztyborek/pen/WGwQbk">Pure CSS sticky menu: check it out on Codepen</a></pre>
  <script async src="http://codepen.io/assets/embed/ei.js"></script>
  <figcaption>Pure CSS sticky menu</figcaption>
</figure>

### Browser support

In Chrome and Firefox the feature works only after turning on the "experimental" flag.

| Chrome         | Firefox | Safari   | Internet Explorer/Edge | Opera      |
| -------------- | ------- | -------- | ---------------------- | ---------- |
| No support     | 32+     | 6.1+     | No support             | No support |

### Resources and further reading

- [CSS `position: sticky` – Introduction and Polyfills](https://www.sitepoint.com/css-position-sticky-introduction-polyfills/)

<h2 id="attr-function">Accessing the HTML attributes' values using CSS</h2>

Apart from CSS counters there are various ways to fill the generated content. One of them is accessing the value of HTML attributes using the `attr()` function.

We can use this feature for creating links accessible in print media.

{% highlight html %}
<p>For more information see 
  <a href="http://martasztybor.pl/css/useful-css-features/">
    the article about useful CSS features
  </a>
</p>
{% endhighlight %}

{% highlight css %}
@media print {
  a::after {
    content: " (" attr(href) ")"
  }
}
{% endhighlight %}

The result then should look like below:

<div class="example">
  For more information see the article about useful CSS features (http://martasztybor.pl/css/useful-css-features/)
</div>

Ire Aderinokun on her blog [bitsofco.de](https://bitsofco.de) described a clever trick with the use of the `attr()` feature. It proves that [styling broken images](https://bitsofco.de/styling-broken-images/) is possible.

### Browser support

| Chrome  | Firefox | Safari   | Internet Explorer/Edge | Opera |
| ------- | ------- | -------- | ---------------------- | ----- |
| 2+      | Any     | 3.1+     | 8+                     | 9+    |


<h2 id="currentcolor-property">The <code>currentColor</code> property</h2>

Before CSS variables (custom properties) will become widely supported, we can alredy use a value with a variable-like feel. It's called `currentColor` and its usage is limited to properties that accept color as a value.

{% highlight css %}
.simple-module {
  color: hotpink;
  border: 2px solid currentColor;
  box-shadow: 3px 3px 0 currentColor;
}
{% endhighlight %}

In the example above the element's font color will be applied to both to `border` and `box-shadow`. 

I prepared a small example of menu with a couple of items to show how `currentColor` can help us write more reusable code. In the example the `menu-item`'s left border and icon should change the color on hover and the color of each item should be different.

{% highlight html %}
  <nav>
  <ul>
    <li class="menu-item menu-item--shop">
      <svg viewBox="0 0 32 32">
        <use xlink:href="#icon-cart"></use>
      </svg>
      <a href="#">Shop</a>
    </li>
    <li class="menu-item menu-item--videos">
      <svg viewBox="0 0 32 32">
        <use xlink:href="#icon-play"></use>
      </svg>
      <a href="#">Videos</a>
    </li>
    <li class="menu-item menu-item--gallery">
      <svg viewBox="0 0 32 32">
        <use xlink:href="#icon-image"></use>
      </svg>
      <a href="#">Gallery</a>
    </li>
    <li class="menu-item menu-item--contact">
      <svg viewBox="0 0 32 32">
        <use xlink:href="#icon-phone"></use>
      </svg>
      <a href="#">Contact</a>
    </li>
  </ul>
</nav>
{% endhighlight %}

{% highlight scss %}
// Normal state 

.menu-item {
  color: white;
  border-bottom: 1px solid white;

  &::before {
    content: '';
    border-left: 5px solid #b5b5b5;
  }

  // Items' colors definitions
  svg {
    fill: white;
  }

  &--shop {
    color: #24C7B7;
  }
  
  &--videos {
    color: #D45542;
  }
  
  &--gallery {
    color: #DEB63E;
  }
  
  &--contact {
    color: #161652;
  }
}

// Hovered state
.menu-item:hover {
  &::before {
    border-left: currentColor;
  }

  svg {
    fill: currentColor;
  }
}
{% endhighlight %}

As we can see, there is no need to separately define colors for the icon and the border for each of the menu items.

<figure>
  <pre class="codepen" data-height="470" data-type="result" data-href="EgKkmz" data-user="sztyborek" data-safe="true"><code></code><a href="http://codepen.io/sztyborek/pen/EgKkmz">currentColor menu example: check it out on Codepen</a></pre>
  <script async src="http://codepen.io/assets/embed/ei.js"></script>
  <figcaption>More modular menu items coloring</figcaption>
</figure>

### Resources and further reading

- [Short explanation on CSS-Tricks](https://css-tricks.com/currentcolor/)
- [Cascading SVG Fill Color](https://css-tricks.com/cascading-svg-fill-color/) on CSS-Tricks
- [Extending the Color Cascade with the CSS `currentColor` Variable](http://blogs.adobe.com/dreamweaver/2015/02/extending-the-color-cascade-with-the-css-currentcolor-variable.html) by Sara Soueidan
- [Keeping CSS short with `currentColor`](https://osvaldas.info/keeping-css-short-with-currentcolor)
 
### Browser support

| Chrome  | Firefox | Safari | Internet Explorer | Opera |
| ------- | ------- | ------ | ----------------- | ----- |
| Any     | Any     | 4+     | 9+                | Any   |

<h2 id="css-feature-queries">CSS Feature queries</h2>

A [Modernizr](https://modernizr.com/) for CSS features? An automated ["Can I Use?"](http://caniuse.com/) in your stylesheets? You're welcome! 

Apart from querying for a certain type of media using `@media` [at-rule](https://css-tricks.com/the-at-rules-of-css/) it's also possible to  query for CSS features. The rule `@supports` can be used for testing if browser supports a certain feature and then apply nested CSS rules if the condition is met. For example:

{% highlight css %}
@supports (display: flex) {
  
  .my-element {
    display: flex;
    justify-content: center;
  }
}
{% endhighlight %}

Feature queries can help us to tackle uneven browser support and progressively enhance our websites.

### Resources and further reading

- [An Introduction to CSS’s `@supports` Rule (Feature Queries)](https://www.sitepoint.com/an-introduction-to-css-supports-rule-feature-queries/)
- [How to use the `@supports` rule in your CSS](http://www.creativebloq.com/css3/how-use-supports-rule-your-css-11410545)

### Browser support

Unfortunately, the browser support isn't too good for feature queries (they still aren't supported in MS Edge).

| Chrome  | Firefox | Safari | Internet Explorer/Edge | Opera |
| ------- | ------- | ------ | ---------------------- | ----- |
| 28+     | 22+     | 9+     | No support             | 12.1+ |

