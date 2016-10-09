---
layout:     post
title:      Basic Responsive Typography Solution I Often Use
date:       2016-10-09 14:00:00
summary:    Typography for Web and, specifically, responsive typography is a very broad topic. To be honest, I haven't mastered yet any bulletproof responsive typography technique. I usually work with web applications, which mostly don't include huge chunks of text. In such cases the solution described in this blogpost can be sufficient.
categories: CSS Typography Responsive
image: /img/responsive.jpg
---

Typography for Web and, specifically, responsive typography is a very broad topic. To be honest, I haven't mastered yet any bulletproof responsive typography technique. I usually work with web applications, which mostly don't include huge chunks of text. In such cases the solution described in this blogpost can be sufficient.

But if you are looking for a verbose, extensive article presenting various aspects of (responsive) typography, jump straight to [further reading](./#further-reading). I recommended a list of articles which can help you to learn the aspects of web typography.

## Basic responsive typography with `rem`s

Solution described below can be a good starting point to learn more sophisticated techniques. Here we will use a combination of media queries and using `rem`s as our sizing units for fonts and measure.

The first step is to set a base font size on the document root element. We will be using `rem`s, so [to make maths simpler](https://snook.ca/archives/html_and_css/font-size-with-rem) we're going to set it as `10px`.

{% highlight css %}
html {
  font-size: 10px; /* 1rem = 10px */
}
{% endhighlight %}


We will be also implementing a mobile first approach, so the font size above will be applied for smallest screens. The next step is to set root font sizes for a couple of breakpoints for larger screens:


{% highlight css %}
html {
  font-size: 10px;
}

@media screen and (min-width: 48em) {
  html {
    font-size: 12px;
  }
}

@media screen and (min-width: 62em) {
  html {
    font-size: 14px;
  }
}

@media screen and (min-width: 75em) {
  html {
    font-size: 16px;
  }
}
{% endhighlight %}

The breakpoints are set in `em`s for [proper scaling when user zooms in](https://cloudfour.com/thinks/the-ems-have-it-proportional-media-queries-ftw/) the page.

The base `font-size: 10px` is too small even for the smallest screens, so we need to set a font size for body, which will cascade for all elements. I picked `1.4rem`, which equals `14px` for small screens and will scale up according to breakpoints shown above.

{% highlight css %}
body {
  font: normal 1.4rem/1.45 Arial, sans-serif;
}
{% endhighlight %}

And the last step of our setup is to globally set the font sizes in `rem`s for basic typography elements like paragraphs and headings.

{% highlight css %}
p {
  margin-top: 1rem;
  margin-bottom: 1rem;
}

h1, h2, h3, h4 {
  line-height: 1.2;
  margin: 1.5rem 0;
}

h1 {
  font-size: 2.25rem;
}

h2 {
  font-size: 2rem;
}

h3 {
  font-size: 1.8rem;
}

h4 {
  font-size: 1.6rem;
}
{% endhighlight %}


<figure>
  <pre class="codepen" data-height="470" data-type="result" data-href="QKmgOy" data-user="sztyborek" data-safe="true"><code></code><a href="http://codepen.io/sztyborek/pen/QKmgOy">Responsive typography setup demo: check it out on Codepen</a></pre>
  <script async src="http://codepen.io/assets/embed/ei.js"></script>
  <figcaption>Responsive typography setup demo</figcaption>
</figure>

<h2 id="further-reading">Further reading</h2>

Solution described above doesn't take into account things like vertical rhythm or responsive headlines. Here is a list of articles which can help you understand such aspects and learn how to implement them into your projects.

- [Interactive guide to web typography](http://www.kaikkonendesign.fi/typography/)
- [4 simple steps to vertical rhythm](http://typecast.com/blog/4-simple-steps-to-vertical-rhythm)
- [Modular scale calculator](http://www.modularscale.com/)
- [Techniques for responsive typography](http://tympanus.net/codrops/2013/11/19/techniques-for-responsive-typography/)
- [Fluid typography with `vw` and `vh`](https://www.smashingmagazine.com/2016/05/fluid-typography/)
- [Responsive typography with vertical rhythm using Sass](https://zellwk.com/blog/responsive-typography/)
- [Responsive typography with Sass maps](https://www.smashingmagazine.com/2015/06/responsive-typography-with-sass-maps/)
