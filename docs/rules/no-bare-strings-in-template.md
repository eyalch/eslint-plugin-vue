---
pageClass: rule-details
sidebarDepth: 0
title: vue/no-bare-strings-in-template
description: disallow the use of bare strings in `<template>`
since: v7.0.0
---
# vue/no-bare-strings-in-template

> disallow the use of bare strings in `<template>`

## :book: Rule Details

This rule disallows the use of bare strings in `<template>`.  
In order to be able to internationalize your application, you will need to avoid using plain strings in your templates. Instead, you would need to use a template helper specializing in translation.  

This rule was inspired by [no-bare-strings rule in ember-template-lint](https://github.com/ember-template-lint/ember-template-lint/blob/master/docs/rule/no-bare-strings.md).


<eslint-code-block :rules="{'vue/no-bare-strings-in-template': ['error']}">

```vue
<template>
  <!-- ✓ GOOD -->
  <h1>{{ $t('foo.bar') }}</h1>
  <h1>{{ foo }}</h1>
  <h1 v-t="'foo.bar'"></h1>

  <!-- ✗ BAD -->
  <h1>Lorem ipsum</h1>
  <div
    title="Lorem ipsum"
    aria-label="Lorem ipsum"
    aria-placeholder="Lorem ipsum"
    aria-roledescription="Lorem ipsum"
    aria-valuetext="Lorem ipsum"
  />
  <img alt="Lorem ipsum">
  <input placeholder="Lorem ipsum">
  <h1 v-text="'Lorem ipsum'" />

  <!-- Does not check -->
  <h1>{{ 'Lorem ipsum' }}</h1>
  <div
    v-bind:title="'Lorem ipsum'"
  />
</template>
```

</eslint-code-block>

:::tip
This rule does not check for string literals, in bindings and mustaches interpolation. This is because it looks like a conscious decision.  
If you want to report these string literals, enable the [vue/no-useless-v-bind] and [vue/no-useless-mustaches] rules and fix the useless string literals.
:::

## :wrench: Options

```js
{
  "vue/no-bare-strings-in-template": ["error", {
    "allowlist": [
      "(", ")", ",", ".", "&", "+", "-", "=", "*", "/", "#", "%", "!", "?", ":", "[", "]", "{", "}", "<", ">", "\u00b7", "\u2022", "\u2010", "\u2013", "\u2014", "\u2212", "|"
    ],
    "attributes": {
      "/.+/": ["title", "aria-label", "aria-placeholder", "aria-roledescription", "aria-valuetext"],
      "input": ["placeholder"],
      "img": ["alt"]
    },
    "directives": ["v-text"],
    "ignoreComponents": ["h1", "custom-component", "CustomComponent"]
  }]
}
```

- `allowlist` ... An array of allowed strings.
- `attributes` ... An object whose keys are tag name or patterns and value is an array of attributes to check for that tag name.
- `directives` ... An array of directive names to check literal value.
- `ignores` ... An array of element names to ignore. RegExp is allowed.

### `{ "ignoreComponents": ["h1", "custom-component", "CustomComponent"] }`

<eslint-code-block fix :rules="{'vue/no-bare-strings-in-template': ['error', { 'ignoreComponents': ['h1', 'custom-component', 'CustomComponent'] }]}">

```vue
<template>
  <!-- ✓ GOOD -->
  <h1>Heading</h1>
  <custom-component>Some text</custom-component>
  <CustomComponent>Some more text</CustomComponent>

  <!-- ✗ BAD -->
  <h2>Lorem ipsum</h2>
  <another-component>Lorem ipsum</another-component>
  <AnotherComponent>Lorem ipsum</AnotherComponent>
</template>
```

</eslint-code-block>

## :couple: Related Rules

- [vue/no-useless-v-bind]
- [vue/no-useless-mustaches]

[vue/no-useless-v-bind]: ./no-useless-v-bind.md
[vue/no-useless-mustaches]: ./no-useless-mustaches.md

## :rocket: Version

This rule was introduced in eslint-plugin-vue v7.0.0

## :mag: Implementation

- [Rule source](https://github.com/vuejs/eslint-plugin-vue/blob/master/lib/rules/no-bare-strings-in-template.js)
- [Test source](https://github.com/vuejs/eslint-plugin-vue/blob/master/tests/lib/rules/no-bare-strings-in-template.js)
