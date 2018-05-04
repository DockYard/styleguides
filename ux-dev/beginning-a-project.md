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

## Example File Structure

```
stylesheets
  modules/
    buttons.css
    footer.css
    header.css
  app.css
  base.css
  layout.css
  load.css
  type.css
```

## APP.CSS

```css
@import "load.css";
@import "base.css";
@import "layout.css";
@import "type.css";

@import "modules/buttons.css";
@import "modules/header.css";
@import "modules/footer.css";
```

Order should be alphabetical inside `modules` directory.

## BASE.CSS

Follows
[SMACSS’ Base Rules](https://smacss.com/book/type-base). Defines major default styling.
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
ol, ul {
  list-style: none;
}
blockquote, q {
  quotes: none;
}
blockquote:before, blockquote:after,
q:before, q:after {
    content: '';
    content: none;
  }
  table {
    border-collapse: collapse;
    border-spacing: 0;
  }
```

## LAYOUT.CSS

This does NOT closely follow
[SMACSS' Layout Rules](https://smacss.com/book/type-layout).
We use `layout.css` for wraps, grids and columns. Here is a
[JS Bin](http://jsbin.com/tiyome/3/edit?html,css,output) that illustrates some
common patterns.

If there are recurring shared wrap sizes, something like `.l-wrap--s`
and `.l-wrap--b` would make sense. If not recurring, `.l-header` would
be better and would not belong in `layout.css`, but in `header.css`.

```css
.l-wrap--s,
.l-wrap--b {
  margin-right: auto;
  margin-left: auto;
  width: 90%;
}

.l-wrap--s {
  max-width: 640px;
}

.l-wrap--b {
  max-width: 1020px;
}
```

## LOAD.CSS

Includes color and font variables. Here is a sample `load.css` file. So
far this only works in Firefox, but with PostCSS, we can use it across
browsers.

```css
:root {
  /* COLORS */
  --white: #fff;
  --black: #000;
  --red: #ff4040;

  /* FONT FAMILIES */
  --serif: "Libre Baskerville", serif;

  /* FONT WEIGHTS */
  --light: 300;
  --book: 400;
  --bold: 700;
}
```

## TYPE.CSS

`.t-display` is used for the shared display font style. Most likely a
page heading.

```css
.t-display {
  font-weight: var(--bold);
  @media (max-width: 1199px) {
    font-size: 24px;
  }
  @media (min-width: 1200px) {
    font-size: 36px;
  }
}
```

`.t-text` is used for the shared text font style. Most likely the styles
used for blocks of text on a blog or static pages.

```css
.t-text {
  font-weight: var(--light);
  @media (max-width: 1199px) {
    font-size: 16px;
  }
  @media (min-width: 1200px) {
    font-size: 18px;
  }
}
```

`.t-link` is used for shared text link styles. Most likely links that
appear within a `.t-text`.

```css
.t-link {
  border-bottom: 1px solid var(--white);
  text-decoration: none;
}
```
