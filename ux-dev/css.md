# CSS
## General info
We are currently using [PostCSS](https://github.com/postcss/postcss/) with [narwin-pack](https://github.com/dockyard/narwin-pack). However, some of our older projects are still written in Sass/SCSS.

## Syntax

* Use soft tabs with two spaces — they're the only way to guarantee code renders the same in any environment.
* See our [onboarding editors information](https://github.com/dockyard/styleguides/blob/master/ux-dev/ux-developer-onboarding.md#editors)
* When grouping selectors, each selector goes on a new line
* Include one space before the opening brace of declaration blocks for legibility.
* Place closing braces of declaration blocks on a new line.
* Include one space after `:` for each declaration.
* Each declaration should appear on its own line for more accurate error reporting.
  * There are a few usecases that we intentionally break this rule. A good examples would be the `grid-template-areas` property. 
* End all declarations with a semi-colon.
* Comma-separated property values should include a space after each comma (e.g., `box-shadow`).
* Don't include spaces after commas within `rgb()`, `rgba()`, `hsl()`, `hsla()`, or `rect()` values. This helps differentiate multiple color values (comma, no space) from multiple property values (comma with space).
* Don't prefix property values or color parameters with a leading zero (e.g., `.5` instead of `0.5` and `-.5px` instead of `-0.5px`).
* Lowercase all hex values, e.g., `#fff`. Lowercase letters are much easier to discern when scanning a document as they tend to have more unique shapes.
* Use shorthand hex values where available, e.g., `#fff` instead of `#ffffff`.
* Quote attribute values in selectors, e.g., `input[type="text"]`. They’re only optional in some cases, and it’s a good practice for consistency.
* Always use double quotes in selectors
* Always specify units unless that value is `0` e.g., `margin: 12px;` instead of `margin: 12;` and `margin: 0;` instead of `margin: 0px;`.
* We try and use `px` for general sizing, `em` or `rem` for type and related spacing depending on context, and all other values when they are appropriate.
* Pseudo element should always have a double semicolon, e.g., `.element::after`
* Nested selectors should have a space between each block:
```
.element {
  .element__child {
    ...
  }

  @media (...) {
    ...
  }
}
```

## Browser Support
Though specific projects may have different requirements, DockYard code is expected to be compatible with the last two releases of all major browsers:
* Chrome
* Firefox
* Safari
* IE/Edge

As a result, on most projects we can use Flexbox and/or Grid.

Always be sure to double check with an individual client to confirm browser support.

## General File Structure
Though some projects may have different requirements, DockYard generally has set of starter files such as:
* `app.css`
	* Holds `@import`s of all other css files
* `base.css`
	* Holds your CSS Reset
* `typography.css`
	* Holds all `t-...` styles for typography
* `variables.css`
	* Holds all global variables for a project
* `fonts.css`
	* Holds all `@font-face` declarations
* `layout.css`
	* Holds all global `l-...` styles
* `modules/`
  * Holds all app specific module files:
    * `modules/search-bar.css`, `modules/cards.css`

Learn more here: [beginning-a-project](https://github.com/dockyard/styleguides/blob/master/ux-dev/beginning-a-project.md#example-file-structure)

## Order of Declaration
Most projects should have our [style linter](https://github.com/DockYard/stylelint-config-narwin) installed. The linter helps us to define and maintain an [order of declaration](https://github.com/DockYard/stylelint-config-narwin/blob/master/index.js#L1).

You can also check out the [Linter Styleguide](https://github.com/dockyard/styleguides/blob/master/ux-dev/stylelint-config-narwin.md).

## Variables
Narwin-pack uses [postcss-custom-properties](https://github.com/postcss/postcss-custom-properties).

Most projects will have a variables file that holds all your global variables. Though there may be times when it makes sense to add variables to individual files, as a general rule variables should live in the `variables.css` file.

## Media queries
Unless we have a good reason to break the rule, we write all our media queries only specifying screen size.

All our `min-width` values should be on "whole number" e.g., `960px`, `576px`. All of our `max-width` values should end 1 below our `min-width` values.

```
@media (max-width: 959px) {
  ...
}

@media (min-width: 960px) {
  ...
}
```

## Resources
* [MDN Web Docs](https://developer.mozilla.org/en-US/)
* [Scalable and Modular Architecture for CSS by Jonathan Snook](http://smacss.com/)
* [CSS Secrets](http://shop.oreilly.com/product/0636920031123.do)
* [Code Smells in CSS](http://csswizardry.com/2012/11/code-smells-in-css/)
* [Writing DRYer vanilla CSS](http://csswizardry.com/2013/07/writing-dryer-vanilla-css/)
* [Shoot to kill; CSS selector intent](http://csswizardry.com/2012/07/shoot-to-kill-css-selector-intent/)

### Extras
* Read [Designing for Performance by Lara Callender Hogan](http://designingforperformance.com/index.html)
* Read [Web Form Design by Luke Wroblewski](http://www.lukew.com/resources/web_form_design.asp)
