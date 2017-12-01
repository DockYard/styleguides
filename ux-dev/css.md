# CSS

Read and follow the
[CSS Syntax section of Mark Otto’s (@mdo) code guide](http://codeguide.co/#http://codeguide.co/#css-syntax).
We don’t follow much of what comes after the Syntax section, so the rest
can be skipped.

We are currently using
[PostCSS](https://github.com/postcss/postcss/)
with
[narwin-pack](https://github.com/dockyard/narwin-pack). However, some of our older
projects are still written in Sass/SCSS.

## Required Reading

* Read
  [Scalable and Modular Architecture for CSS by Jonathan Snook](https://smacss.com/)
* Read
  [CSS Secrets](http://shop.oreilly.com/product/0636920031123.do)
* Read the
  [Code Smells in CSS](http://csswizardry.com/2012/11/code-smells-in-css/)
  article
* Read the
  [Writing DRYer vanilla CSS](http://csswizardry.com/2013/07/writing-dryer-vanilla-css/)
  article
* Read the
  [Shoot to kill; CSS selector intent](http://csswizardry.com/2012/07/shoot-to-kill-css-selector-intent/)
  article

## Extra Reading

When you have free time
* Read
  [Designing for Performance by Lara Callender Hogan](http://designingforperformance.com/index.html)
* Read
  [Web Form Design by Luke Wroblewski](http://www.lukew.com/resources/web_form_design.asp)

## Selector Order

Order things in HTML order unless it makes more sense to do it
alphabetically.

## Declaration Order

Declarations should be consistent throughout the application.

If there is a shorthand version for that set of styles, follow in the order of the shorthand. For margin that would be `margin-top`, `margin-right`, `margin-bottom` then `margin-left`. For border that would be `border-width`, `border-style` then `border-color`.

Example with grouping comments:
```
.block {
  /* BOX-MODEL */
    margin: 0;
    padding: 0;
    width: 0;
    height: 0;
    display: block;

  /* POSITIONING */
    position: absolute;
    z-index: 1;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;

  /* TYPOGRAPHY */
    font-size: 18px;
    color: var(--green);

  /* VISUAL */
    border: 1px solid black;
    background-color: var(--green);

  /* MISC */
    opacity: 1;
}
```