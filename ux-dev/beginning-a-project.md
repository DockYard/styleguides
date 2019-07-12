# Beginning a Project

## Questions to Ask Client

* Is this project going to be responsive? If yes, what are the min and
  max px we should test for?
* What browsers should we support? Latest Chrome, Firefox and
  Safari as well as IE10+ unless otherwise specified.
* Are there specific devices we should support? Do we have access to
  those devices to test on them?
* Do we have to prepare for translation to other languages?
* How do you want us to prepare the deliverables? (Zip, GitHub, etc.)
* If the project has already begun, can we use
  [BEM](https://github.com/dockyard/styleguides/blob/master/ux-dev/class-naming-conventions.md#bem-naming-conventions)
  naming conventions?

## Questions to Ask Designer

* __Review all mockups and ask questions up front__ to prevent blocks
  during development. This includes clarifications or missing items.
* Do we have rights to all the images being used in the mockup?
* Does the client have licenses to all the fonts being used?

## Get narwin-pack
At [DockYard](https://dockyard.com/), we use PostCSS in our projects. [`narwin-pack`](https://github.com/DockYard/narwin-pack) is our curated collection of PostCSS plugins including:

- [`postcss-partial-import`](https://github.com/jonathantneal/postcss-partial-import),
- [`postcss-nested`](https://github.com/postcss/postcss-nested),
- [`postcss-custom-properties`](https://github.com/postcss/postcss-custom-properties),
- [`postcss-inline-svg`](https://github.com/TrySound/postcss-inline-svg),
- [`postcss-svgo`](https://github.com/ben-eb/postcss-svgo),
- [`autoprefixer`](https://github.com/postcss/autoprefixer),
- [`postcss-hexrgba`](https://github.com/seaneking/postcss-hexrgba)
- [`postcss-discard-comments`](https://github.com/ben-eb/postcss-discard-comments)

## SVG Jar Setup
We use [SVG Jar](https://github.com/ivanvotti/ember-svg-jar) to handle embedding of our SVG images in our Ember projects. To install SVG Jar, run:

`ember install ember-svg-jar`

Place SVG files in your project's `public/svgs` directory. Using Chrome, visit [http://localhost:4200/ember-svg-jar/index.html](http://localhost:4200/ember-svg-jar/index.html) to select any SVG and add to any template.

For more information about our SVG best practices, [check out our SVG styleguide](https://github.com/DockYard/styleguides/blob/master/ux-dev/svg.md).

## `stylelint-config-narwin` Setup
We use [`stylelint-config-narwin`](https://github.com/DockYard/stylelint-config-narwin#installation) to push the UXD team in the same direction for CSS best practices. To install `stylelint-config-narwin`

```
npm install stylelint-config-standard --save-dev
npm install stylelint-config-narwin --save-dev
```

### For Ember-specific installations

```
npm install stylelint-config-narwin --save-dev
ember install ember-cli-stylelint
```

In your project root, you'll have a newly created file, `stylelint.js`. Add the following:

```js
{
  "extends": "stylelint-config-narwin"
}
```

Additionally, in your `ember-cli-build.js` we'll need to update the CSS linter for CSS instead of SCSS:
```js
....
module.exports = function(defaults) {
  var app = new EmberApp(defaults, {
    // Add options here
    stylelint: {
      linterConfig: {
        syntax: 'css'
      }
    }
  });
....
```
Additional configuration options can be found in the [`stylelint-config-narwin` documentation](https://github.com/DockYard/stylelint-config-narwin#extending-the-config).


## Example File Structure

```
stylesheets
  modules/
    buttons.css
    footer.css
    header.css
    sidebar.css
  app.css
  base.css
  layout.css
  typography.css
  variables.css
```

## APP.CSS

```css
@import "variables.css";
@import "base.css";
@import "layout.css";
@import "typography.css";

@import "modules/buttons.css";
@import "modules/header.css";
@import "modules/footer.css";
@import "modules/sidebar.css";
```

Order should be alphabetical inside `modules` directory.

## BASE.CSS

Follows
[SMACSS’ Base Rules](http://smacss.com/book/type-base). Defines major default styling.
Does not include class or ID selectors.

We use modified versions of the
[Meyer reset](http://meyerweb.com/eric/tools/css/reset/).
If certain elements will not be used in the project, they should be removed.

```css
/* http://meyerweb.com/eric/tools/css/reset/
   v2.0 | 20110126
   License: none (public domain)
*/

html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed,
figure, figcaption, footer, header, hgroup,
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
  font: inherit;
  vertical-align: baseline;
}

/* HTML5 display-role reset for older browsers */
article, aside, details, figcaption, figure,
footer, header, hgroup, menu, nav, section {
  display: block;
}

body {
  line-height: 1;
}

ol,
ul {
  list-style: none;
}

blockquote,
q {
  quotes: none;
}

blockquote:before,
blockquote:after,
q:before,
q:after {
  content: '';
  content: none;
}

table {
  border-collapse: collapse;
  border-spacing: 0;
}

textarea,
input,
select,
button {
  border: 0;
  border-radius: 0;
  outline: none;
  appearance: none;
  background: transparent;
}

textarea {
  min-height: 8.5em; /* height of 3 lines */
}

select,
button {
  line-height: 1em;
  cursor: pointer;
}

/* HTML box-sizing: border-box; reset */
html {
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: inherit;
}

/* REMOVE FIREFOX INVALID BOX-SHADOW */
select:-moz-ui-invalid,
input:-moz-ui-invalid,
textarea:-moz-ui-invalid {
  box-shadow: none;
}
```

## LAYOUT.CSS

This does NOT closely follow
[SMACSS' Layout Rules](http://smacss.com/book/type-layout).
We use `layout.css` for wraps, grids and columns. Here is a
[JS Bin](https://jsbin.com/hijavakimu/edit?html,css,output) that illustrates some
common patterns.

If there are recurring shared wrap sizes, something like `.l-wrap--small`
and `.l-wrap--big` would make sense. If not recurring, `.header` would
be better and would not belong in `layout.css`, but in `header.css`. We should
should not specify component classes in `layout.css`, only global classes.

`layout.css`
```scss
.l-body {
  display: grid;

  @media (max-width: 799px) {
    grid-template-areas:
      "header"
      "main"
      "sidebar"
      "footer";
    grid-template-columns: 1fr;
  }

  @media (min-width: 800px) {
    grid-template-areas:
      "header header"
      "main sidebar"
      "footer footer";
    grid-template-columns: 3fr 1fr;
  }
}

.l-body--small {
  max-width: 680px;
}

.l-body--big {
  max-width: 1020px;
}

.l-main {
  grid-area: main;
}
```

`header.css`
```css
.header {
  grid-area: header;
}
```
`footer.css`
```css
.footer {
  grid-area: footer;
}
```
`sidebar.css`
```css
.sidebar {
  grid-area: sidebar;
}
```


## VARIABLES.CSS

Modularize the variables with a source of truth in a color book that is provided from the designer. We group like variables for quick reference. Here is a sample `variables.css` file.

`variables.css`
```css
:root {
  /* BASE COLORS */
  --color-black: #000;
  --color-white: #fff;

  /* BRAND COLORS */
  --color-brand-primary: #58a4b0;
  --color-brand-primary-alt: #417881;
  --color-brand-secondary: #daa49a;

  /* GRAYS */
  --color-gray-1: #fcfcfc;
  --color-gray-2: #adadad;
  --color-gray-3: #999;
  --color-gray-4: #4d4d4d;
  --color-gray-5: #1b1b1b;

  /* FONT FAMILY */
  --font-source: "Source Sans", sans-serif;
  --font-libre: "Libre Baskerville", serif;

  /* FONT WEIGHT */
  --weight-light: 300;
  --weight-regular: 400;
  --weight-bold: 700;
  --weight-black: 900;

  /* GLOBAL COMPONENT COLORS */
  --color-g-component-shadow: 0 2px 14px 0 rgba(var(--color-gray-3), .27);
  --color-g-component-shadow-focus: 0 4px 8px -6px rgba(0,0,0,0.08), 0 0 11px 0 rgba(0,0,0,0.06), 0 8px 12px -6px rgba(0,0,0,0.10)

  /* GLOBAL MEASUREMENTS */
  --global-radius: 6px;
  --global-radius-small: 2px;

  /* GLOBAL TRANSITION */
  --global-transition: .2s ease;
}

```
`button.css`
``` scss
.button {
  color: var(--color-white);
  background-color: var(--color-black);
  …
}

.button-inverted {
  color: var(--color-black);
  background-color: var(--color-white);
  …
}

```

## TYPOGRAPHY.CSS

Styles in `typography.css` should be specific to typographic properties. You should apply layout
 through BEM and match the typography class, such as `t-link` with a BEM class (i.e.- `<a class="nav__link t-link">`). Any box model or visual style declarations should exist within component files.

Typography properties include all styles that effect typography. They don't touch the box-model and try to avoid colors. Some examples include: `font-family`, `font-style`, `font-weight`. Some examples of properties that don't belong are: `padding`, `display`, `z-index`.

In the `:root` define your type scale used throughout the project.

```scss
:root {
  --type-scale-12: .75rem;
  --type-scale-16: 1rem;
  --type-scale-20: 1.25rem;
  --type-scale-24: 1.5rem;
  --type-scale-32: 2rem;
}
```

`.t-display` is used for the shared display font style. Most likely a
page heading.

```scss
.t-display {
  font-weight: var(--weight-bold);

  @media (max-width: 799px) {
    font-size: var(--type-scale-24);
  }

  @media (min-width: 800px) {
    font-size: var(--type-scale-32);
  }
}
```

`.t-body` is used for the shared text font style. Most likely the styles
used for blocks of text on a blog or static pages.

```scss
.t-body {
  font-size: var(--type-scale-16);
  font-weight: var(--weight-book);
  line-height: 1.556;
}
```

`.t-link` is used for shared text link styles. Most likely links that
appear within a `.t-text`.

```scss
.t-link {
  border-bottom: 1px dotted var(--color-g-border-1);
  transition: .2s ease border-bottom, .2s ease color;

  &:hover,
  &:active,
  &:focus {
    color: var(--color-black);
    border-bottom-style: solid;
    border-bottom-color: var(--color-black);
  }

  &:focus {
    box-shadow: var(--color-g-component-shadow-focus);
  }
}
```