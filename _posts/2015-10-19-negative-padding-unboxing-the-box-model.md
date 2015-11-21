---
layout:     post
title:      Negative Padding&#58; Unboxing the Box Model
date:       2015-10-19 20:00:00
summary:    Lorem ipsum
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

Despite its hackish reputation, negative margin is perfectly valid CSS (although it may be limited depending on the browser).

##Why we don't have negative padding then?

The word "padding" refers to something inner. That's because the idea of negative padding may appear counterintuitive. Element with negative padding would be like something with an inside-out lining.

These are mostly my thoughts and deductions about negative values in CSS box model. If you stumbled upon any reliable resource that explains the decision behind allowing negative margin and disallowing negative padding, feel free to share in the comments!


