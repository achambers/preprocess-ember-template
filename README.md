ember-template-imports
==============================================================================

This addon provides plugins for the `<template>`-tag way of writing components within Ember! 

```js
import MyComponent from './my-component';

<template>
  <MyComponent/>
</template>
```

Template imports are an upcoming feature in Ember. Like Glimmer components, the
primitive APIs for supporting imports were built before we decided on a final
format for their high level usage. There are a number of different ideas for how
we can integrate imports with templates, and the idea behind this addon is that
it can be a test bed for them all. This way, we can share common tooling between
solutions, and work together as a community as we explore the design space.

So far, this addon supports two different potential formats:

- Template tags, embedded in `.gjs` files

  ```js
  import MyComponent from './my-component';

  <template>
    <MyComponent/>
  </template>
  ```

For the previous version of this addon, see [this repository](https://github.com/patricklx/ember-template-imports).
And huge thanks to @patricklx for his contributions here!

## Using Template Tags and `.gjs` Files

Template tags are a new syntax introduced in `.gjs`, a new file format that is
a superset of standard JavaScript. In this syntax, templates are defined in
JavaScript files directly.

```js
// components/hello.gjs
<template>
  <span>Hello, {{@name}}!</span>
</template>
```

This example defines a template-only component, which is the default export of
`hello.js`. You would be able to use this component in another component
like so:

```js
// components/hello-world.gjs
import Hello from './hello';

<template>
  <Hello @name="world" />
</template>
```

You can also export the component explicitly:

```js
// components/hello.gjs
export default <template>
  <span>Hello, {{@name}}!</span>
</template>
```

Omitting the `export default` is just syntactic sugar. In addition, you can
define template-only components and assign them to variables, allowing you to
export components with named exports:

```js
export const First = <template>First</template>

export const Second = <template>Second</template>

export const Third = <template>Third</template>
```

This also allows you to create components that are only used locally, in the
same file:

```js
const Option = <template>
  <option selected={{@selected}} value={{@value}}>
    {{or @title @value}}
  </option>
</template>

export const CustomSelect = <template>
  <select>
    {{#each @options as |option|}}
      <Option
        @value={{option.value}}
        @selected={{eq option @selectedOption}}
      />
    {{/each}}
  </select>
</template>
```

Helpers and modifiers can also be defined in the same file as your components,
making them very flexible:

```js
import { helper } from '@ember/component/helper';
import { modifier } from 'ember-modifier';

const plusOne = helper(([num]) => num + 1);

const setScrollPosition = modifier((element, [position]) => {
  element.scrollTop = position
});

<template>
  <div class="scroll-container" {{setScrollPosition @scrollPos}}>
    {{#each @items as |item index|}}
      Item #{{plusOne index}}: {{item}}
    {{/each}}
  </div>
</template>
```

Finally, to associate a template with a class-based component, you can use the
template syntax directly in the class body:

```js
// components/hello.gjs
export default class Hello {
  name = 'world';

  <template>
    <span>Hello, {{this.name}}!</span>
  </template>
}
```

## Reference: built-in helpers, modifiers, components

As implemented as part of the [Strict Mode Templates RFC][rfc-496], the built in
helpers, modifiers and components are available for import:

* `array` (`import { array } from '@ember/helper';`)
* `concat` (`import { concat } from '@ember/helper';`)
* `fn` (`import { fn } from '@ember/helper';`)
* `get` (`import { get } from '@ember/helper';`)
* `hash` (`import { hash } from '@ember/helper';`)
* `on` (`import { on } from '@ember/modifier';`)
* `Input` (`import { Input } from '@ember/component';`)
* `LinkTo` (`import { LinkTo } from '@ember/routing';`)
* `Textarea` (`import { Textarea } from '@ember/component';`)

[rfc-496]: https://github.com/emberjs/rfcs/pull/496

Compatibility
------------------------------------------------------------------------------

* Ember.js v3.27 or above
* Ember CLI v2.13 or above
* `ember-cli-htmlbars` 6.0 or above
* Node.js v12 or above


Installation
------------------------------------------------------------------------------

```
ember install ember-template-imports
```

Editor Integrations
------------------------------------------------------------------------------

To get syntax highlighting inside embedded templates and support for the GJS
file extension, you may need to configure your editor.

### Visual Studio Code
The [vscode-glimmer](https://marketplace.visualstudio.com/items?itemName=chiragpat.vscode-glimmer) plugin handles syntax highlighting for both proposed formats.

### Neovim

[Example Neovim Config](https://github.com/NullVoxPopuli/dotfiles/blob/main/home/.config/nvim/lua/plugins/syntax.lua#L15) with support for good highlighting of embedded templates in JS and TS, using:

- https://github.com/nvim-treesitter/nvim-treesitter
- https://github.com/alexlafroscia/tree-sitter-glimmer



Contributing
------------------------------------------------------------------------------

See the [Contributing](CONTRIBUTING.md) guide for details.


License
------------------------------------------------------------------------------

This project is licensed under the [MIT License](LICENSE.md).
