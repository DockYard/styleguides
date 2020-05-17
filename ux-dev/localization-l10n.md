# Localization/l10n

## `ember-intl`

<!-- Discuss how this is used for numbers, dates, time, and relative time -->

The `ember-intl` Ember addon is the tool we use for translating apps into other languages. This addon allows us to do the following:

* Locale-aware numbers
* Locale-aware dates and times
* Locale-aware display of relative time
* More

See the [documentation for ember-intl](https://ember-intl.github.io/ember-intl/docs) for more information.

## Keys/Strings

All strings should be represented as localization keys and follow a project structure. For example:

`app-name.template-name.component-name.string-name`

Each period separated name is tied to a place in the file structure or within the Handlebars template.

Let's break those down:

`app-name` is the name of our app or project, in this case "imatic".ss

`template-name` is the name of the template file, "contact-form"

`component-name` is the name of the individual page component, "contact-form-container"

`string-name` is the name of the content of the string, "submit-button"

*Before localization*

```hbs
  <button>Submit Contact Form</button>
```

*After localization*
```hbs
  <button>
    {{t "app-name.template-name.component-name.string-name"}}
  </button>
```

## Configuring Template Linting

To ensure localization keys are used and not strings, add the following code to your `template-lintrc.js` file:

```js
rules: {
  'no-bare-strings': true
}
```

## Pluralization

<!-- Discuss pluralization ICU string structure -->

## Dynamic content

<!-- Dealing with strings that contain dynamic content -->

## Generic strings to avoid duplicate strings

<!-- Dealing with strings that contain the same copy -->

## Concatenation

<!-- Dealing with strings that have HTML -->

## Resources

[ICU Project](http://demo.icu-project.org/icu-bin/locexp)
