---
layout:     post
title:      Negative Padding&#58; Unboxing the Box Model
date:       2015-10-19 20:00:00
summary:    This post is inspired with a discussion we had recently at our workplace. It started with a question, asked by one of the backend developers&#58; "Why do we have negative margin in CSS, while there's no possibility to set negative padding?". I like such "out of the box" questions, so I'll try to break down a problem and take a closer look of the box model.
categories: CSS
image: /img/pack.jpg
---

This post is inspired with a discussion we had recently at our workplace. It started with a question, asked by one of the backend developers:

> Why do we have negative margin in CSS, while there's no possibility to set negative padding?

The first thing which comes to mind is probably: "Because negative padding makes no sense!". It's not a complete answer though. I like such "out of the box" (pun intended) questions, so I'll try to break down a problem and take a closer look of the box model. The spec defines it as it goes:

> The CSS [box model](http://www.w3.org/TR/CSS21/box.html) describes the rectangular boxes that are generated for elements in the document tree and laid out according to the [visual formatting model](http://www.w3.org/TR/CSS21/visuren.html).
>
> -- <cite>[W3C](http://www.w3.org)</cite>

Here is the box model's visual representation:

Every box has its content area, which optionally can be surrounded by padding, border and margin area. Each of them has different purposes. For this article purposes, let's focus on the padding and the margin:

- Padding pushes away the border from the content. It makes the space around the content. When it's set to zero, the padding edge is the same as the content edge.
- Margin pushes away the content from any other existing boxes. It is used to make the horizontal and vertical space between elements. When margin is set to zero, it means that the margin edge is the same as the border edge.

There are [several differences](http://www.impressivewebs.com/difference-block-inline-css/) between setting the box model properties for block level and inline level elements.

Box model properties are essential for the browser to determine (during layout/reflow mechanism) the coordinates of the element and the space that it occupies on the page.

##Understanding negative margin

Despite its hackish reputation, negative margin is allowed by the specification.

Like I mentioned above, margins are used to create spacing between elements. To add any whitespace then, the margin value must be more than zero (zero margin value means no whitespace between elements). So, what would happen if we set the value to less than zero? Probably it would cause elements overlapping. Let's see how the elements with negative margin set behave.

Add image with visual representation:

For every static positioned element (with no float or absolute positioning applied):

- When **top or left** margin is set to negative value, it pulls the element _and_ the following elements up.
- When **bottom or right** margin is set to negative, it pulls the following elements up

### Differences between negative margin and relative positioning

Applying `position: relative` makes an element to shift its position relatively to its original position. Having that in mind, one may assume that this technique does exactly the same as negative margin. There are, though, two main differences:

- The element's original position remains in the flow of the document. So, if we apply for example `top: -20px`, the element will be shifted upwards, but its original space will remain and other elements will _not_ be shifted.
- For an element with `position: relative` the `z-index` property will work.

### Use cases

- Vertical centering of an element (this is one of [several techniques](https://css-tricks.com/centering-css-complete-guide/))
- Intentional elements overlapping, when you understand the consequences of negative margin

##Why we don't have negative padding then?

The word "padding" refers to something inner. That's because the idea of negative padding may appear counterintuitive. Element with negative padding would be like something with an inside-out lining.

These are mostly my thoughts and deductions about negative values in CSS box model. If you stumbled upon any reliable resource that explains the decision behind allowing negative margin and disallowing negative padding, feel free to share in the comments!


