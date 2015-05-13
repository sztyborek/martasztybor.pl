---
layout: post
title:  "Welcome to Jekyll!"
date:   2015-02-04 13:21:33
categories: jekyll update
---

<h2>Subcontent</h2>

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

There are lists, too!

<ul>
	<li>I am unordered list item</li>
	<li>I am unordered list item</li>
	<li>I am unordered list item</li>
</ul>

<ol>
	<li>I am first list item</li>
	<li>I am the second...</li>
	<li>And I am last, third element</li>
</ol>

<blockquote>
	<p>
    Perfection is achieved, not when there is nothing more to add, but when there is nothing left to take away.
  </p>
  <footer><cite title="Antoine de Saint-Exupéry">Antoine de Saint-Exupéry</cite></footer>
</blockquote>

There is also syntax highlighting!

{% highlight css %}
  .person {
    color: black;
    transition: all .5s ease-in-out;
  }

  .person__hand {}
  .person--female {}
  .person--female__hand {}
  .person__hand--left {}

  // Some comment

  /* Some block level comment */

  angular
    .module('ets')
    .directive('collapse', collapse);

  collapse.$inject = [];

  function collapse() {
    return {
      scope: {
        collapse: '='
      },
      link: ($scope, element) => {
        $scope.$watch('collapse', (shouldCollapse) => {
          if (shouldCollapse) {
            element.collapse('show');
          } else {
            element.collapse('hide');
          }
        });
      }
    };
  }

  <article aria-hidden="true">
    Lorem ipsum
  </article>

{% endhighlight %}

<h3>Heading H3</h3>

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

<h4>Heading H4</h4>

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

<h5>Heading H5</h5>

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.
