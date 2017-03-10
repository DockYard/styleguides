# Handlebars Style Guide

## Table Of Contents

* [Quotes](#quotes)
* [Multiline expressions](#multiline-expressions)

## Quotes
Prefer double quotes to single quotes
```hbs
{{! good }}
<button {{action "doThing"}}>Do Thing</button>

{{! bad }}
<button {{action 'doThing'}}>Do Thing</button>
```

## Multiline expressions

Multi-line expressions should specify attributes starting on the second
line, and should be indented one deeper than the start of the component
or helper name.
```hbs
{{! good }}
{{x-thing
    value=blah
    options=options
    label="thing"
}}

{{! bad }}
{{x-thing
  value=blah
  options=options
  label="thing"}}

{{! this will be annoying to re-indent if you rename your component }}
{{! this is also annoying to add a new line you'd like at the end of the expression, or to remove the last line }}
{{x-thing value=blah
          options=options
          label="thing"}}

```
