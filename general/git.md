# Git

## Table of Contents

1. [Commit Messages](#commit-messages)
   1. [Writing Message Titles](#writing-message-titles)
   1. [Writing Message Bodies](#writing-message-bodies)
   1. [Committing with Multiple Authors](#committing-with-multiple-authors)

## Commit Messages

Commit messages are an opportunity for us to convey a brief story or synopsis of
work completed. We should be answering questions like:

* What caused this original regression and why does this newly introduced code fix it?
* What dependency was removed from the application, and how does this affect performance?

When commit messages answer questions like these, our audience, colleagues and clients,
will have a better understanding of what our intention was.

The following is a list of items you should include in your commit message:

* Descriptive commit title
* What and Why
* Include supporting ticket number

### Writing Message Titles

Commit message titles should be descriptive and succinct. Imagine that you
have to scan a list of commit titles and messages. What characteristics would
make that task easier?

Let's take a look at some examples of commits for an Emacs `init` file.
Here is a set of commit titles that are inconsistent with varying
degrees of information.

#### Not Recommended

```bash

$ git log --oneline -5

7411bd8 Here's `auto-revert`
f4d4e32 Add pandoc for markdown since it has more features than the `markdown` library. I can now use `pandoc` to convert differrent file formats. It's the "swiss army knife of markup libraries!"
dde37c9 update .tmux.conf and smartparens
cea6ea8 fixes
7012c74 specify spell-fu files

```

Now, here is a set of commit titles that are consistent, yet concise.

#### Recommended

```bash

$ git log --oneline -5

7411bd8 Add `auto-revert` to `dired` mode
f4d4e32 Replace `markdown` library and use `pandoc`
dde37c9 Update `.tmux.conf` with new prefix and add `smartparens`
cea6ea8 Fix file formatting and include number lines
7012c74 Specify `spell-fu` file types and only apply to markdown and org-mode

```

By having clear commit titles, we can search through commits for keywords as well.
For example (a very contrived example), let's use the aforementioned bad and good examples
and let's try to search for the word `dired`.

#### Not Recommended

```bash

$ git log --oneline -1

7411bd8 Here's `auto-revert`

# Let's search for `dired`
$ git log --oneline -1 --grep='dired'

# No commits appear

```

Now using the set of well written commits, let's try searching for the keyword `dired`
again.

#### Recommended

```bash

$ git log --oneline -1

7411bd8 Add `auto-revert` to `dired` mode

# Let's search for `dired`
$ git log --oneline -1 --grep='dired'

7411bd8 Add `auto-revert` to `dired` mode
# We see the desired commit!

```

### Writing Message Bodies

Just like descriptive commit titles, the content of the commit can provide
useful information to our audience.

The message body of the commit is *optional*, however, it is beneficial to take
the time to convey the *why* and *how* in the commit body.

Let's take a look at Tim Pope's [example of a model Git commit message.][pope-git-commit]

```bash

Capitalized, short (50 chars or less) summary

More detailed explanatory text, if necessary.  Wrap it to about 72
characters or so.  In some contexts, the first line is treated as the
subject of an email and the rest of the text as the body.  The blank
line separating the summary from the body is critical (unless you omit
the body entirely); tools like rebase can get confused if you run the
two together.

Write your commit message in the imperative: "Fix bug" and not "Fixed bug"
or "Fixes bug."  This convention matches up with commit messages generated
by commands like git merge and git revert.

Further paragraphs come after blank lines.

- Bullet points are okay, too

- Typically a hyphen or asterisk is used for the bullet, followed by a
  single space, with blank lines in between, but conventions vary here

- Use a hanging indent

```

In addition, we should include any keywords or references to ticket numbers. This
will help future team members find relevant information. Here's another contrived
example that has a descriptive title and body.

```bash

Add note about how to close tickets with a commit

This commit will help users understand which ticket I want to close.
For example, GitHub provides a list of keywords that will close GitHub
issues automatically when a commit is merged in. You and your team should
decide on how on a consistent way to reference other project management tools.
Furthermore, determine a consistent place in the commit where you should
reference the ticket number. Suggested areas are the top or bottom of the commit
message body. Thanks all!

Close #1234

```

[pope-git-commit]: https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html

### Committing with Multiple Authors

Often times, we will work with colleagues and pair program together. When we are
ready to commit, there is a benefit to committing together. By adding co-authors
to our commits, we inform future team members that there were colleagues with
immediate context of the code. Adding co-authors is fairly straightforward.

```bash

Add a commit with co-authors

Here's the content of the commit. Talk with your team on where you'd like
to add co-authors. Try the top or below of the commit.

Co-authored-by: Narwin Narwhal <narwin@example.com>
Co-authored-by: Nanny Narwhal <nanny@example.com>

```
