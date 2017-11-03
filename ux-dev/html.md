# HTML

Good reads:
* [HTML section of Mark Otto’s (@mdo) code guide](http://codeguide.co/#html)
and
* [Avoiding common HTML5 Mistakes](http://html5doctor.com/avoiding-common-html5-mistakes/).
* [HTML: A good basis for accessibility](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/HTML)

## Indentation and Line Breaks

Break to a new line if the tag contains another element.

Rather than this:

```html
<p>This is a <a href="#">link</a>.</p>
```

We do this:

```html
<p>
  This is a
  <a href="#">link</a>.
</p>
```

The period has to be right after the anchor tag in this case because we don’t want an
[extra space](http://stackoverflow.com/questions/588356/why-does-the-browser-renders-a-newline-as-space)
when the browser renders it. This makes it easier to see all elements
even in narrower Vim windows.

But sometimes we can’t avoid it:

```html
<h2>June 16<sup>th</sup></h2>
```

because there shouldn’t be a space or possibility of line break
between the date and its
[ordinal indicator](http://en.wikipedia.org/wiki/Ordinal_indicator).

## Comments

* [Clear communication through HTML and GitHub](https://dockyard.com/blog/2015/09/02/clear-communication-through-html)
* [The Art of Comments](https://css-tricks.com/the-art-of-comments/)
* Add TODO’s for incomplete placeholder links, images and copy. Examples
  of this happening are when we don’t have final copy or when dev needs
  to make it dynamic.
* Explain anything that may be confusing like strange calculations or
  weird rules like `-webkit-text-size-adjust: 100%;` for solving mobile
  webkit zooming as told by
  http://stackoverflow.com/questions/5303263/fix-font-size-issue-on-mobile-safari-iphone-where-text-is-rendered-inconsisten

Example:
```html
{{! TODO: Include final copy when it is delivered}}
<p>Hi! This is fake copy that should never see a production environment</p>
```

## Escaping characters

* [Use escapes for `&lt;`(<), `&gt;`(>), `&amp;`(&).](http://www.w3.org/International/questions/qa-escapes#use)
* No need to escape for
  [smartquotes](http://smartquotesforsmartpeople.com/) or en/em dashes. Makes it
  easier to read.

## QA

* Check HTML in
  [WAVE’s outliner plugin](https://chrome.google.com/webstore/detail/wave-evaluation-tool/jbbplnpkjmmeebjpijfedlgcdilocofh?hl=en-US)
  or your favorite outliner. Does how we broke it down makes sense?
* Test with screenreader (VoiceOver and
  [NVDA](http://www.nvaccess.org/)).
* Make sure
  [forms are semantic and accessible](http://www.uxbooth.com/articles/styling-forms-accessibly/).
* Are there any extra elements that are unnecessary?

## Common Patterns

* [Cites and blockquotes](http://html5doctor.com/cite-and-blockquote-reloaded/)
* [Figures and figcaptions](http://html5doctor.com/the-figure-figcaption-elements/)
* The less elements the better. We try to avoid elements that are only
  there for styling purposes. It’s simpler to have less elements unless
  they are needed for semantic reasons.
```html
<nav class="nav">
  <a class="nav__link" href="/about">About</a>
  <a class="nav__link" href="/team">Team</a>
  <a class="nav__link" href="/contact">Contact</a>
</nav>
```
