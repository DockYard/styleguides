# Ember.js Style Guide

## Table Of Contents

* [General](#general)
* [Organizing your modules](#organizing-your-modules)
* [Controllers](#controllers)
* [Templates](#templates)
* [Routes](#routes)
* [Ember data](#ember-data)


## General

### Create local version of Ember.\* and DS.\*

Future versions of Ember will be released as ES2015 modules, so we'll be
able to import `Ember.computed` directly as `computed`. This includes
`computed.alias` or `computed.bool`, should be set to `alias` and
`bool`, respectively. Do not use extend prototype syntax

```javascript
// Good

import Ember from 'ember';
import DS from 'ember-data';

const {
  Model,
  attr
} = DS;

const { computed } = Ember;
const { alias } = computed;

export default Model.extend({
  firstName: attr('string'),
  lastName: attr('string'),

  surname: alias('lastName'),

  fullName: computed('firstName', 'lastName', function() {
    // Code
  })
});

// Bad

export default DS.Model.extend({
  firstName: DS.attr('string'),
  lastName: DS.attr('string'),

  fullName: Ember.computed('firstName', 'lastName', {
    get() {
      // Code
    },

    set() {
      // Code
    }
  }),

  fullNameBad: function() {
    // Code
  }.property('firstName', 'lastName')
});
```

## Organizing your modules

```js
export default Component.extend({
  // Defaults
  tagName: 'span',

  // Single line CP
  post: alias('myPost'),

  // Multiline CP
  authorName: computed('author.firstName', 'author.lastName', {
    get() {
      // Code
    },

    set() {
      // Code
    }
  })

  actions: {
    someAction: function() {
      // Code
    }
  }
});
```

### Define default values first

Define your object's default values first.

### Define single line computed properties second

Define single line computed properties (`thing: alias('myThing')`)
after default values.

### Define multi-line computed properties third

Multi-line computed properties should follow your single line CPs.
Please follow the [new computed syntax](http://emberjs.com/blog/2015/05/13/ember-1-12-released.html#toc_new-computed-syntax).

### Define actions last

Define your route/component/controller's action last, to provide a
common place that actions can be found.

### Avoid overwriting init

Unless you want to change an object's `init` function, perform actions
by hooking into the object's `init` hook via `on`. This prevents you
from forgetting to call `_super`. [Here is why you shouldn't override
init](http://reefpoints.dockyard.com/2014/04/28/dont-override-init.html).

### Use Pods structure

Store local components within their pod, global components in the
`components` structure.

```
app
  application/
    template.hbs
    route.js
  blog/
    index/
      blog-listing/ - component only used on the index template
        template.hbs
      route.js
      template.hbs
    route.js
    comment-details/ - used within blog templates
      component.js
      template.hbs

  components/
    tag-listing/ - used throughout the app
      template.hbs

  post/
    adapter.js
    model.js
    serializer.js
```

## Controllers

### Define query params first

For consistancy and ease of discover, list your query params first in
your contoller. These should be listed above default values.

### Do not use ObjectController or ArrayController

ObjectController is presently deprecated, and ArrayController will be as
well. Controllers are not going to be used in Ember 2.0, so by using
`Controller`, you will make it easier to migrate to 2.0.

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

### Use block syntax

Use block syntax instead of `in` syntax with block helpers

```hbs
{{! Good }}
{{#each posts as |post|}}

{{! Bad }}
{{#each post in posts}}
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

## Routes

### Perform all async actions required for the page to load in `model` hooks

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

### Organize your models

Group attributes, relations, then computed properties. Organize each
subgroup alphabetically.


## Tests

### Unit
Definition: Testing algorithimic complexity one collaborator at a time, no additional collaborators.
Example: Asserting the return value of a function or computed given certain inputs.

### Integration
Definition: Testing the interaction between multiple units (but still at a level that understands the implementation).
Example: Asserting that a component or group of components renders correctly given they are instantiated with certain arguments

### Acceptance
Definition: Black box testing of the application, often tied directly to high level business use-cases. Simulate the outside world.
Example: Asserting that when the user visits the blog, a list of posts is shown.

### Page Objects
Use Page Objects for modeling interactions with the application during tests.
Page Objects are responsible for knowing about the HTML and CSS
implementation details of the application and providing an API for
interacting with page elements. This isolates our tests from
presentation details and provides a single place to fix breakages due to
changes in presentation.

```js
// Good

// tests/page-objects/base.js
export default class BaseObject {
  clickWithText(selector, text) {
    click(selector, `:contains("${title}")`);
    return this;
  }
}

// tests/page-objects/posts-index.js
import BaseObject from './base';

export default class PostsIndexObject extends BaseObject {
  clickPostTitle(title) {
    click('.post-item', `:contains("${title}")`);
    return this;
  }
}

// Test
test('clicking post title navigates to the post show page', function(assert) {
  visit('/posts');

  new PostsIndexObject({ assert })
    .clickPostTitle('Best Post Ever!');

  // more test code
});
```

```js
// Bad

// Test
test('clicking post title navigates to the post show page', function(assert) {
  visit('/posts');
  click('.post-item:contains("Best Post Evar!")');

  // more test code
});
```

### Data Attributes
When using data attributes to target page elements, use attribute values
that are unique, even between elements of the same 'type'.

```hbs
{{! Good }}

<div data-auto-id="posts-list">
  {{#each posts as |post|}}
    {{post-item post=post data-auto-id=(dasherize-and-concat "post-list-item" post.title)}}
  {{/each}}
</div>
```

```hbs
{{! Bad }}

<div data-auto-id="posts-list">
  {{#each posts as |post|}}
    {{post-item post=post data-auto-id="posts-list-item"}}
  {{/each}}
</div>
```

### Server Mocking
Ideally, tests that require interaction with a backend should be
run against a test instance of the backend as described in [this blog
post](https://dockyard.com/blog/2015/03/25/testing-when-your-frontend-and-backend-are-separated).
When this approach is not possible, use
[Pretender](https://github.com/pretenderjs/pretender) to create a mock
server and stub out endpoint responses.

Stub endpoints on a per-test basis rather than sharing static fixture responses
between tests. Sharing static fixture responses between tests leads to
duplication between fixtures and/or test failures due to changes in shared fixtures.

```js
// Good
import Ember from 'ember';
import { module, test } from 'qunit';
import startApp from 'my-app/tests/helpers/start-app';
import Pretender from 'pretender';
import prepareResponse from '../helpers/prepare-response';
import PostFactory from '../factories/post';

let application, server;

module('Acceptance: Posts Index', {
  beforeEach() {
    application = startApp();
    server = new Pretender();
  },

  afterEach() {
    Ember.run(application, 'destroy');
  }
});

test('`More Posts` button is disabled when there are no more posts', function(assert) {
  assert.expect(2);

  prepareResponse(server, '/posts', {
    method: 'GET',
    response: { data: PostFactory.build(10) }
  });

  new PostsIndexObject({ assert })
    .visit()
    .assertPostsCount(10)
    .assertMorePostsEnabled(false);
});

test('`More Posts` button is enabled when there are more posts', function(assert) {
  assert.expect(2);

  prepareResponse(server, '/posts', {
    method: 'GET',
    response: { data: PostFactory.build(11) }
  });

  new PostsIndexObject({ assert })
    .visit()
    .assertPostsCount(10)
    .assertMorePostsEnabled(true);
});
```

```js
// Bad
import Ember from 'ember';
import { module, test } from 'qunit';
import startApp from 'my-app/tests/helpers/start-app';
import startServer from '../helpers/start-server';

let application, server;

module('Acceptance: Posts Index', {
  beforeEach() {
    application = startApp();
    server = startServer(); // This function returns a pretender instance with static responses that are shared between tests.
  },

  afterEach() {
    Ember.run(application, 'destroy');
    server = undefined;
  }
});

test('`More Posts` button is disabled when there are no more posts', function(assert) {
  assert.expect(2);

  new PostsIndexObject({ assert })
    .visit()
    .assertPostsCount(10)
    .assertMorePostsEnabled(false);
});
```
