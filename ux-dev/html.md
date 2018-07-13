# HTML
At DockYard, we use semantic HTML to ensure content is accessible and conveys meaning and relationships. We don’t add elements to our HTML solely for stylistic purposes. 

## Attribute Order
We follow the order below for listing attributes on an element:
* class
* id, name
* src, for, type, href, value
* title, alt
* role, aria-*
* data-*

## Escaping characters
* [Use escapes for `&lt;`(<), `&gt;`(>), `&amp;`(&).](https://www.w3.org/International/questions/qa-escapes#use)
* No need to escape for smart quotes or en/em dashes. 

## Line Breaks and Whitespace
In Handlebars, you can add a tilde `~` character inside tags to remove whitespace before or after a component or output. This is useful for removing line breaks that make code more readable, but negatively impact styles because of browser-rendered whitespace. You don't need to do this with every instance of handlebars and should use this as needed.

You could do this:
```hbs
<section>
  {{~content~}}
</section>
```

Instead of this:
```hbs
<section>{{content}}</section>
```

Another example:
```hbs
{{~#each item in model~}}
   {{item}}
{{~/each~}}
```
...removes whitespace before and after the list, and between items. While...

```hbs
{{#each item in model}}
   {{~item~}}
{{/each}}
```
...leaves spaces before and after the list, but removes between items.

## Comments
The most common HTML comment at DockYard is a `TODO`. `TODO`s are used to track items that need to be completed at a later date (such as incomplete placeholder links, images, and copy) or when engineering is needed to complete the work. 

When writing a `TODO`, specify if it is for UXD or engineering. If it’s for engineering, make sure to communicate that `TODO` with an engineer via a tag in the PR, a ping in the project Slack, or both. 

When you finish a `TODO`, delete the comment.

Example:
```html
{{! TODO ENG: Toggle class `is-active` on the button below when a user clicks on it }}
<button>Add Event</button>
```

## Common Patterns
* [Cites and Blockquotes](http://html5doctor.com/cite-and-blockquote-reloaded/)
* [Figures and Figcaptions](http://html5doctor.com/the-figure-figcaption-elements/)
* [Responsive Images](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images)

## QA
* Check HTML in [WAVE’s outliner plugin](https://chrome.google.com/webstore/detail/wave-evaluation-tool/jbbplnpkjmmeebjpijfedlgcdilocofh?hl=en-US) or your favorite outliner to ensure markup is accessible. 
* Test with a screen reader (VoiceOver and/or [NVDA](https://www.nvaccess.org/)).
* Make sure [forms are semantic and accessible](http://www.uxbooth.com/articles/styling-forms-accessibly/).
* Remove any elements that are used only for styling.
* Check to make sure you’re using the most accurate, semantic elements.
* Ensure your content uses smart quotes and en/em dashes when necessary.

## Resources
* [HTML section of Mark Otto’s Code Guide](http://codeguide.co/#html) (Note that our attribute order is slightly different)
* [Avoiding Common HTML5 Mistakes](http://html5doctor.com/avoiding-common-html5-mistakes/)
* [MDN’s HTML: A good basis for accessibility](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/HTML)
* [The Art of Comments](https://css-tricks.com/the-art-of-comments/)
* [Clear communication through HTML and GitHub](https://dockyard.com/blog/2015/09/02/clear-communication-through-html)
* [Quotes and Accents](http://quotesandaccents.com/)
* [Smart Quotes For Smart People](http://smartquotesforsmartpeople.com/) 
* [Handlebars Whitespace Control](https://handlebarsjs.com/expressions.html#whitespace-control)
* [Ember White Space Playground](https://emberjs.jsbin.com/nubup/1/edit?html,css,js,output)
* [MDN’s Element Reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)
* [VS Code TODO Highlighter](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight)
