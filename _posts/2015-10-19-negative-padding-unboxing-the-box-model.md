---
layout:     post
title:      Negative Padding&#58; Unboxing the Box Model
date:       2015-10-19 20:00:00
summary:    This post is inspired with a discussion we had recently at our workplace. It started with a question, asked by one of the backend developers&#58; "Why do we have negative margin in CSS, while there's no possibility to set negative padding?". I like such "out of the box" questions, so I'll try to break down a problem and take a closer look of the box model.
categories: CSS
image: /img/pack.jpg
---

This post is inspired with a discussion we had once at [our workplace](http://10clouds.com). It started with a question, asked by one of the backend developers:

> Why do we have negative margin in CSS, while there's no possibility to set negative padding?

The first thing which comes to mind is probably: "Because negative padding makes no sense!". It's not a complete answer though. I like such "out of the box" (pun intended) questions, so I'll try to break down a problem and take a closer look of the box model. The spec defines it as it goes:

> The CSS [box model](http://www.w3.org/TR/CSS21/box.html) describes the rectangular boxes that are generated for elements in the document tree and laid out according to the [visual formatting model](http://www.w3.org/TR/CSS21/visuren.html).
>
> -- <cite>[W3C](http://www.w3.org)</cite>

Here is the box model's visual representation:

<figure>
	<figcaption>CSS Box Model</figcaption>
</figure>

Every box has its content area, which optionally can be surrounded by padding, border and margin area. Each of them has different purposes. Let's focus on the padding and the margin:

- Padding pushes away the border from the content, so it makes the space around the content. When it's set to zero, the padding edge is the same as the content edge.
- Margin pushes away the content from any other existing boxes. It is used to make the horizontal and vertical space between elements. When margin is set to zero, it means that the margin edge is the same as the border edge.

It's possible to set box model values by setting corresponding CSS properties (for example, to apply element's dimensions, use `width` and `height`, and `padding` to set padding width). You must keep in mind that there are [several differences](http://www.impressivewebs.com/difference-block-inline-css/) between setting the box model properties for block level and inline level elements.

Box model properties are essential for the browser to determine the coordinates of the element and the space that it occupies on the page ([here is more detailed information about how browsers work](http://www.html5rocks.com/en/tutorials/internals/howbrowserswork)). Browser uses a coordinate system with `(0,0)` point placed in the top left corner of the viewport (the visible part of the browser window, the `html` element). Elements' positions are measured in pixels with positive `x` direction going to the right and positive `y` direction to the bottom.

<figure>
	<figcaption>Coordinate system of the browser</figcaption>
</figure>

##Understanding negative margin

Despite its "hackish" reputation, negative margin [is allowed by the specification](http://www.w3.org/TR/CSS21/box.html#margin-properties).

Like I mentioned above, margins are used to create spacing between elements. To add any whitespace then, the margin value must be more than zero. So, what would happen if we set the value to less than zero? Probably it would cause elements overlapping. Let's see how the elements with negative margin set behave.

<figure>
	<figcaption></figcaption>
</figure>

For every static positioned element (with no float or absolute positioning applied):

(Cases: element positioned statically, relatively, absolutely - or fixed, applied float)

- When **top or left** margin is set to negative value, it pulls the element _and_ the following elements up.
- When **bottom or right** margin is set to negative, it pulls the following elements up

### Differences between negative margin and relative positioning

Applying `position: relative` makes an element to shift its position relatively to its original position. Having that in mind, one may assume that this technique does exactly the same as negative margin. There are, though, two main differences:

- The element's original position remains in the flow of the document. So, if we apply for example `top: -20px`, the element will be shifted upwards, but its original space will remain and other elements will _not_ be shifted.
- For an element with `position: relative` the `z-index` property will work.

<figure>
	<figcaption>Visualization of the difference between relative positioning and negative margin</figcaption>
</figure>

### Use cases of negative margin

- Vertical centering of an element (this is one of [several techniques](https://css-tricks.com/centering-css-complete-guide/))
- Intentional elements overlapping, when you understand the consequences of negative margin
- In grid systems, for grid container. Grid container has negative left and right margin to override grid gutter paddings.

##Why we don't have negative padding then?

The word "padding" refers to something inner. That's because the idea of negative padding may appear counterintuitive. Element with negative padding would be like something with an inside-out lining.


These are my assumptions...

Let's have a `<p>` element with some text content, padding and border:

{% highlight html %}
<p class="example">Lorem ipsum dolor sit amet.</p>
{% endhighlight %}

{% highlight css %}
.example {
  padding: 0 2em;
  border: 1px solid blue;
}
{% endhighlight %}

This is how would it render in the browser:

// Illustration

For this example purposes, let's assume that it's possible to set the negative padding for an element:

{% highlight css %}
.example {
  padding: -2em 0 2em 0;
  border: 1px solid blue;
}
{% endhighlight %}

Then the element would probably render like that:

// Illustration

The element's bottom would probably be "cut out" by the bottom border. Let's go further with our thought experiment and set to negative value also the top padding:

{% highlight css %}
.example {
  padding: 0 -2em;
  border: 1px solid blue;
}
{% endhighlight %}

How the element would be rendered then? Well...

// Illustration

The main problem with the negative padding is for browser to determine the element's `width` property. How would the browser perform the calculations when negative padding values were allowed? The content would overflow outside the border making it even more hard [when `box-sizing: border-box` set](https://css-tricks.com/box-sizing/).

These are mostly my thoughts and deductions about negative values in CSS box model. If you stumbled upon any reliable resource that explains the decision behind allowing negative margin and disallowing negative padding, feel free to share in the comments!

Thank you for reading!


