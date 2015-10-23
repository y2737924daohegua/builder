Builder
=======

Builder is a task runner.

Builder is an enhancement to `npm run TASK`.

Builder is a meta-tool for all your commond build, quality, test tasks.

## Overview

At a high level `builder` is a tool for consuming `package.json` `scripts`
commands, providing sensible / flexible defaults, and support various scenarios
("archetypes") for your common use cases across multiple projects.

## Archetypes

Archetypes deal with common scenarios for your projects. Like:

* [builder-react-component][]: A React component
* A React application server
* A Chai / jQuery / VanillaJS widget

Archetypes typically provide:

* A `package.json` with `builder`-friendly `script` tasks.
* Dependencies and dev dependencies to build, test, etc.
* Configuration files for all `script` tasks.

In most cases, you won't need to override anything. But, if you do, pick the
most granular `scripts` command in the archetype you need to override and
define _just that_ in your project's `package.json` `script` section. Copy
any configuration files that you need to tweak and re-define the command.

### Special Archetype Tasks

Archetypes use conventional `scripts` task names, except for the following
special cases:

* `"npm:postinstall"
* `"npm:preversion"
* `"npm:version"
* `"npm:test"`

These tasks are specifically actionable during the `npm` lifecycle, and
consequently, the archetype mostly ignores those for installation by default,
offering them up for actual use in _your_ project.

As an **additional restriction**, non-`npm:FOO`-prefixed tasks with the same
name (e.g., `FOO`) _may_ call then `npm:`-prefixed task, but _not_ the other
way around. So

```js
// Good / OK
"npm:test": "builder run test-frontend",
"test": "builder run npm:test",

// Bad
"npm:test": "builder run test",
"test": "builder run test-frontend",
````

## Package Script Commands

The underyling concept here is that `builder` `script` commands simple _are_
NPM-friendly `package.json` `script` commands. Pretty much anything that you
can execute with `npm run FOO` can be executed with `builder run FOO`.

### Tips & Tricks

#### Terminal Color

Builder uses `exec` under the hood with piped `stdout` and `stderr`. Programs
typically interpret the piped environment as "doesn't support color" and
disable color. Consequently, you typically need to set a "**force color**"
option on your executables in `scripts` commands if they exist.

#### `require.resolve()`

Builder uses some magic to enhance `NODE_PATH` to look in the root of your
project (normal) and in the installed modules of builder archetypes. This
latter path enhancement sometimes throws tools / libraries for a loop. We
recommend using `require.resolve("LIBRARY_OR_REQUIRE_PATH")` to get the
appropriate installed file path to a dependency.

This comes up in situations including:

* Webpack loaders
* Karma included files


**TODO: Document more.**

**TODO: Give actual override _example_**

**TODO: How to install / configure an archetype**

[builder-react-component]: https://github.com/FormidableLabs/builder-react-component
