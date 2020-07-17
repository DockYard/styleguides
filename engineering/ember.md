# Ember.js Style Guide

## Table Of Contents

* [General](#general)
* [Organizing your modules](#organizing-your-modules)
* [Models](#models)
* [Controllers](#controllers)
* [Templates](#templates)
* [Routing](#routing)
* [Ember data](#ember-data)


## General

### Import what you use, do not use globals

For Ember Data, we should import `Model`, `attr` and other ember-data modules
from `ember-data`, and then destructure our desired modules.
For Ember, we should only import those modules that will be used.
Ember's imports can be found in the [JavaScript Module API RFC](https://github.com/emberjs/rfcs/blob/master/text/0176-javascript-module-api.md) and in the [Ember API docs](https://emberjs.com/api/ember/).

```javascript
// Good
import Model from 'ember-data/model';
import attr from 'ember-data/attr';
import { computed } from '@ember/object';
import { alias } from '@ember/object/computed';

export default Model.extend({
  firstName: attr('string'),
  lastName: attr('string'),

  surname: alias('lastName'),

  fullName: computed('firstName', 'lastName', function() {
    // Code
  })
});

// Bad
import Ember from 'ember';
import DS from 'ember-data';

export default DS.Model.extend({
  firstName: DS.attr('string'),
  lastName: DS.attr('string'),

  surname: Ember.computed.alias('lastName'),

  fullName: Ember.computed('firstName', 'lastName', {
    get() {
      // Code
    },

    set() {
      // Code
    }
  })
});
```

### Don't use Ember's prototype extensions

Avoid Ember's `Date`, `Function` and `String` prototype extensions. Prefer the
corresponding functions from the `Ember` object.

Preferably turn the prototype extensions off by updating the
`EmberENV.EXTEND_PROTOTYPES` setting in your `config/environment` file.

```javascript
module.exports = function(environment) {
  var ENV = {
    EmberENV: {
      EXTEND_PROTOTYPES: {
        Date: false,
        Function: false,
        String: false
      }
    }
```

```javascript
// Good

export default Model.extend({
  hobbies: w('surfing skateboarding skydiving'),
  fullName: computed('firstName', 'lastName', function() { ... }),
  didError: on('error', function() { ... })
});

// Bad

export default Model.extend({
  hobbies: 'surfing skateboarding skydiving'.w(),
  fullName: function() { ... }.property('firstName', 'lastName'),
  didError: function() { ... }.on('error')
});
```

### Usage of `get` and `set`

Since ember v3.1 release, we are able to use native getters for majority
of the cases where computed property access is needed. Ember `get` helper is
still useful and required when:

- the lookup is nested such as `a.b.c` because Ember get helper can prevent uncaught null-property exceptions.
- dealing with ember data promise proxy objects or async relationships.
Ember proxy objects, including promise proxies, still require that you call `.get()` to read values.

`object.get(attr)` and `object.set(attr, value)` should be prefered over
`get(object, attr)` and `set(object, attr)` when Ember `get` helper is needed. This makes code more
readable and consistent with JS native property access and community guidelines.

Ember guides always use `this.get(property)` instead of `get(this, property)`,
this practice also remove the need for an extra
`import { get, set } from ember/object` line in certain files.

In addition, complex object computed properties and actions should be unit-tested and
it is highly suggested to track for any runtime errors with frontend error-tracking services.
Eliminate the need for mocking a complex object in unit tests whenever possible, however when
any data mocking is needed, correct types should be used:
- when mocked property is always expected to be an EmberObject,
EmberObject should be used instead of a native JS object.
- when mocked property is expected to be an EmberArray,
use EmberArray instead of native JS array.
- when mocked property is expected to be an ember data model,
please create or retrieve an instance of a target ember data model instead of a native JS object.
- when mocked property is a service, owner.lookup should be used,
for rare cases when true mocking is needed mocked object should extend from ember service.

For pre-3.1 ember applications use `get(context, attr)` and `set(context, attr, value)`
instead of `context.get(attr)` and `context.set(attr, value)`.

```js
// Bad
import { get, set } from '@ember/object';

set(this, 'isSelected', true);
get(this, 'isSelected');

// Good

this.set('isSelected', true);
this.isSelected;
this.get('model.comments.firstObject');
this.get('model.comments.firstObject.createdAt');
```

### Use brace expansion

This allows much less redundancy and is easier to read.

Note that **the dependent keys must be together (without space)** for the brace expansion to work.

```js
// Good
fullName: computed('user.{firstName,lastName}', {
  // Code
})

// Bad
fullName: computed('user.firstName', 'user.lastName', {
  // Code
})
```

## Organizing your modules

Ordering a module's properties in a predictable manner will make it easier to
scan.

1. __Plain properties__

   Start with properties that configure the module's behavior. Examples are
   `tagName` and `classNames` on components and `queryParams` on controllers and
   routes. Followed by any other simple properties, like default values for properties.

2. __Single line computed property macros__

   E.g. `alias`, `sort` and other macros. Start with service injections. If the
   module is a model, then `attr` properties should be first, followed by
   `belongsTo` and `hasMany`.

3. __Multi line computed property functions__

4. __Lifecycle hooks__

   The hooks should be chronologically ordered by the order they are invoked in.

5. __Functions__

   Public functions first, internal functions after.

6. __Actions__

```js
export default Component.extend({
  // Plain properties
  tagName: 'span',

  // Single line CP
  post: alias('myPost'),

  // Multiline CP
  authorName: computed('author.{firstName,lastName}', function() {
    // code
  }),

  // Lifecycle hooks
  didReceiveAttrs() {
    this._super(...arguments);
    // code
  },

  // Functions
  someFunction() {
    // code
  },

  actions: {
    someAction() {
      // Code
    }
  }
});
```

### Override init

Rather than using the object's `init` hook via `on`, override init and
call `_super` with `...arguments`. This allows you to control execution
order. [Don't Don't Override Init](https://dockyard.com/blog/2015/10/19/2015-dont-dont-override-init)

## Models

### Organization

Models should be grouped as follows:

* Attributes
* Associations
* Computed Properties

Within each section, the attributes should be ordered alphabetically.

```js
// Good
import Model from 'ember-data/model';
import attr from 'ember-data/attr';
import { hasMany } from 'ember-data/relationships';
import { computed } from '@ember/object';

export default Model.extend({
  // Attributes
  firstName: attr('string'),
  lastName: attr('string'),

  // Associations
  children: hasMany('child'),

  // Computed Properties
  fullName: computed('firstName', 'lastName', function() {
    // Code
  })
});

// Bad
import Model from 'ember-data/model';
import attr from 'ember-data/attr';
import { hasMany } from 'ember-data/relationships';
import { computed } from '@ember/object';

export default Model.extend({
  children: hasMany('child'),
  firstName: attr('string'),
  lastName: attr('string'),

  fullName: computed('firstName', 'lastName', function() {
    // Code
  })
});

```

## Controllers

### Define query params first

For consistency and ease of discover, list your query params first in
your controller. These should be listed above default values.

### Alias your model

It provides a cleaner code to name your model `user` if it is a user. It
is more maintainable, and will fall in line with future routable
components

```javascript
export default Controller.extend({
  user: alias('model')
});
```

## Templates

### Do not use partials

Always use components. Partials share scope with the parent view, use
components will provide a consistent scope.

### Don't yield `this`

Use the hash helper to yield what you need instead.

```hbs
{{! Good }}
{{yield (hash thing=thing action=(action "action"))}}

{{! Bad }}
{{yield this}}
```

### Use components in `{{#each}}` blocks

Contents of your each blocks should be a single line, use components
when more than one line is needed. This will allow you to test the
contents in isolation via unit tests, as your loop will likely contain
more complex logic in this case.

```hbs
{{! Good }}
{{#each posts as |post|}}
  {{post-summary post=post}}
{{/each}}

{{! Bad }}
{{#each posts as |post|}}
  <article>
    <img src={{post.image}} />
    <h1>{{post.title}}</h2>
    <p>{{post.summar}}</p>
  </article>
{{/each}}
```

### Always use the `action` keyword to pass actions.

Although it's not strictly needed to use the `action` keyword to pass on
actions that have already been passed with the `action` keyword once,
it's recommended to always use the `action` keyword when passing an action
to another component. This will prevent some potential bugs that can happen
and also make it more clear that you are passing an action.

```hbs
{{! Good }}
{{edit-post post=post deletePost=(action deletePost)}}

{{! Bad }}
{{edit-post post=post deletePost=deletePost}}
```

### Ordering static attributes, dynamic attributes, and action helpers for HTML elements

Ultimately, we should make it easier for other developers to read templates.
Ordering attributes and then action helpers will provide clarity.

```hbs
{{! Bad }}

<button disabled={{isDisabled}} data-auto-id="click-me" {{action (action click)}} name="wonderful-button" class="wonderful-button">Click me</button>
```

```hbs
{{! Good }}

<button class="wonderful-button"
  data-auto-id="click-me"
  name="wonderful-button"
  disabled={{isDisabled}}
  onclick={{action click}}>
    Click me
</button>
```

## Routing

### Route naming
Dynamic segments should be underscored. This will allow Ember to resolve
promises without extra serialization
work.

```js
// good

this.route('foo', { path: ':foo_id' });

// bad

this.route('foo', { path: ':fooId' });
```

[Example with broken
links](https://ember-twiddle.com/0fea52795863b88214cb?numColumns=3).

### Perform all async actions required for the page to load in route `model` hooks

The model hooks are async hooks, and will wait for any promises returned
to resolve. An example of this would be models needed to fill a drop
down in a form, you don't want to render this page without the options
in the dropdown. A counter example would be comments on a page. The
comments should be fetched along side the model, but should not block
your page from loading if the required model is there.

## Ember Data

### Be explicit with Ember Data attribute types

Even though Ember Data can be used without using explicit types in
`attr`, always supply an attribute type to ensure the right data
transform is used.

```javascript
// Good

export default Model.extend({
  firstName: attr('string'),
  jerseyNumber: attr('number')
});

// Bad

export default Model.extend({
  firstName: attr(),
  jerseyNumber: attr()
});
```
