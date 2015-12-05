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

<figure>
  <img src="{{ "/img/negative-padding/boxdim.png" | prepend: site.baseurl }}" alt="CSS box model" />
	<figcaption>CSS Box Model</figcaption>
</figure>

Every box has its content area which optionally can be surrounded by padding, border and margin area. Each of them has different purposes. Let's focus on the padding and the margin:

- Padding pushes away the border from the content, so it makes the space around the content. When it's set to zero, the padding edge is the same as the content edge.
- Margin pushes away the content from any other existing boxes. It is used to make the horizontal and vertical space between elements. When margin is set to zero, it means that the margin edge is the same as the border edge.

It's possible to set box model values by setting corresponding CSS properties (for example, to apply element's dimensions, use `width` and `height`, and `padding` to set padding width). You must keep in mind that there are [several differences](http://www.impressivewebs.com/difference-block-inline-css/) between setting the box model properties for block level and inline level elements.

Box model properties are essential for the browser to determine the coordinates of the element and the space that it occupies on the page. Browser uses a coordinate system with `(0,0)` point placed in the top left corner of the document (which pretty much corresponds with the `html` element). Elements' positions are measured in pixels with positive `x` direction going to the right and positive `y` direction to the bottom.

<figure>
  <img src="{{ "/img/negative-padding/coordinates.png" | prepend: site.baseurl }}" alt="Coordinate system of the browser" />
	<figcaption>Coordinate system of the browser</figcaption>
</figure>

##Understanding negative margin

Despite its "hackish" reputation negative margin [is allowed by the specification](http://www.w3.org/TR/CSS21/box.html#margin-properties). If applied with care and deep understanding, they produce valid code.

Like I mentioned above margins are used to create spacing between elements. To add any whitespace the margin value must be more than zero. Therefore, if we set negative margin value, it will cause elements to overlap. Let's see how the elements with negative margin set behave.

<pre class="codepen" data-height="470" data-type="result" data-href="GoROMY" data-user="sztyborek" data-safe="true"><code></code><a href="http://codepen.io/sztyborek/pen/GoROMY">Check out this Pen!</a></pre>
<script async src="http://codepen.io/assets/embed/ei.js"></script>

<figure>
	<figcaption>Visualization: adjacent elements with positive top margin value applied to the second one (left) and negative top margin value (right) - the second element overlaps the first one</figcaption>
</figure>

For every element with `position: static` (which is the default `position` property value) and no `float` applied:

- When **top or left** margin is set to negative value, it pulls the element **and** the following elements up or left.
- When **bottom or right** margin is set to negative, it pulls the following elements up or left.

Every elment with `position` value other than `static` (which means: `absolute`, `relative` or `fixed`) [creates new stacking context](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/Stacking_without_z-index). Absolute positioning removes the element from the document flow. In this case the main difference will be **not pulling** the following elements in the direction of negative margin. The same goes for elements with fixed positioning.

Case becomes a little more complex for floated elements. However [the thing can be a little quirky](http://robin.medvedi.eu/morning-headache-float-negative-margin-and-ie8/) and doesn't really fall within the scope of this article, so I refer the interested to further reading.

### Differences between negative margin and relative positioning

Applying `position: relative` makes an element to shift its position relatively to its original position. Having that in mind one may assume that this technique does exactly the same as negative margin. There are two main differences though:

- The element's original position remains in the flow of the document. It means that if we apply, for example `top: -20px`, the element will be shifted upwards but its original space will remain and other elements will **not** be shifted.
- For an element with `position: relative` the `z-index` property will work. This difference concerns only elements with `position: static`. You must keep in mind that [`z-index` works only for positioned elements](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/Adding_z-index).

<pre class="codepen" data-height="470" data-type="result" data-href="zrYLpN" data-user="sztyborek" data-safe="true"><code></code><a href="http://codepen.io/sztyborek/pen/zrYLpN">Check out this Pen!</a></pre>
<script async src="http://codepen.io/assets/embed/ei.js"></script>
<figure>
	<figcaption>Visualization of the main difference between relative positioning and negative margin</figcaption>
</figure>

### Use cases of negative margin

Nowadays there are several use cases when negative margin can come in handy:

- Vertical centering of a block element of known height (source: [CSS-Tricks guide for centering](https://css-tricks.com/centering-css-complete-guide/)):
{% highlight css %}
.parent {
  position: relative;
}

/*
  Element that should be centered
*/
.child {
  position: absolute;
  top: 50%;
  height: 200px;
  margin-top: -100px; /* Half of the element's height */
}
{% endhighlight %}

<pre class="codepen" data-height="470" data-type="result" data-href="VvRbpN" data-user="sztyborek" data-safe="true"><code></code><a href="http://codepen.io/sztyborek/pen/VvRbpN">Check out this Pen!</a></pre>
<script async src="http://codepen.io/assets/embed/ei.js"></script>

- Implementing design with overlapping elements. This is quite popular use case and negative margin allows you to achieve this effect without breaking the document flow.

<pre class="codepen" data-height="470" data-type="result" data-href="VvRbpN" data-user="sztyborek" data-safe="true"><code></code><a href="http://codepen.io/sztyborek/pen/VvRbpN">Check out this Pen!</a></pre>
<script async src="http://codepen.io/assets/embed/ei.js"></script>
<figure>
	<figcaption>Layout with overlapping elements</figcaption>
</figure>

- Creating full-width elements within padded containers:

{% highlight css %}
.parent {
  padding: 1.5rem;
}

/*
  Element that should have full width
*/
.child {
  width: 100%;
  margin-left: -1.5rem;
  margin-right: -1.5rem;
}
{% endhighlight %}

<pre class="codepen" data-height="470" data-type="result" data-href="VvRbpN" data-user="sztyborek" data-safe="true"><code></code><a href="http://codepen.io/sztyborek/pen/VvRbpN">Check out this Pen!</a></pre>
<script async src="http://codepen.io/assets/embed/ei.js"></script>
<figure>
	<figcaption>Making element fill up whole width of a container with padding applied</figcaption>
</figure>

- "Clearing" the leftmost and rightmost gutter padding in grid systems. Take a look at an example from Bootstrap:
{% highlight html %}
<div class="row">
  <div class="col-sm-6"></div>
  <div class="col-sm-6"></div>
</div>
{% endhighlight %}

{% highlight css %}
.row {
  margin-right: -15px;
  margin-left: -15px;
}

.col-sm-6 {
  padding-right: 15px;
  padding-left: 15px;
}
{% endhighlight %}

I think there are just a few examples of "non-hacky" uses of negative margin. If you encountered any other, share it in comments!

##Why we don't have negative padding then?

The word "padding" refers to something inner. That's because the idea of negative padding may appear counterintuitive. Element with negative padding would be like something with an inside-out lining.

Imagine that setting negative padding values is possible. When padding value is set to zero, the border edge is the same as the content edge. When we set it negative, then border will overlap the content. Maybe that would be useful but, hey, content extended beyond its boundaries? That would make calculations of the element dimensions and position on the page definitely troublesome.

###Hypothetical behavior of negative margin

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

<pre class="codepen" data-height="470" data-type="result" data-href="VvRbpN" data-user="sztyborek" data-safe="true"><code></code><a href="http://codepen.io/sztyborek/pen/VvRbpN">Check out this Pen!</a></pre>
<script async src="http://codepen.io/assets/embed/ei.js"></script>

For this example purposes, let's assume that it's possible to set the negative padding for an element:

{% highlight css %}
.example {
  padding: -2em 0 2em 0;
  border: 1px solid blue;
}
{% endhighlight %}

Then the element would probably render like that:


The element's bottom would probably be "cut out" by the bottom border. Let's go further with our thought experiment and set to negative value also the top padding:

{% highlight css %}
.example {
  padding: 0 -2em;
  border: 1px solid blue;
}
{% endhighlight %}

How the element would be rendered then?


These are mostly my thoughts and deductions about negative values in CSS box model. If you stumbled upon any reliable resource that explains the decision behind allowing negative margin and disallowing negative padding, feel free to share in the comments!

For deeper understanding issues mentioned in this article I recommend reading:
MDN, W3C recommendation, other resources...

Thank you for reading!


