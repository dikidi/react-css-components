# React CSS components

[![Travis build status](https://img.shields.io/travis/andreypopp/react-css-components/master.svg)](https://travis-ci.org/andreypopp/react-css-components)
[![npm](https://img.shields.io/npm/v/react-css-components.svg)](https://www.npmjs.com/package/react-css-components)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Motivation](#motivation)
- [Installation & Usage](#installation-&-usage)
- [Base components](#base-components)
- [Custom composite components as bases](#custom-composite-components-as-bases)
- [Variants](#variants)
- [Prop variants](#prop-variants)
- [TODO](#todo)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Motivation

Define React presentational components with CSS.

The implementation is based on [CSS modules][]. In fact React CSS Components is
just a thin API on top of CSS modules.

**NOTE:** The current implementation is based on Webpack but everything is ready
to be ported onto other build systems (generic API is here just not yet
documented). Raise an issue or better submit a PR if you have some ideas.

## Installation & Usage

Install from npm:

    % npm install react-css-components

Configure in `webpack.config.js`:

```js
module.exports = {
  ...
  module: {
    loaders: [
      {
        test: /\.react.css$/,
        loader: 'babel-loader!react-css-components/webpack',
      }
    ]
  }
  ...
}
```
Now you can author React components in `Styles.react.css`:
```css
Label {
  color: red;
}

Label:hover {
  color: white;
}
```

And consume them like regular React components:
```js
import {Label} from './styles.react.css'

<Label /> // => <div class="<autogenerated classname>">...</div>
```

## Base components

### DOM components

By default React CSS Components produces styled `<div />` components. You can
change that by defining `base:` property:

```css
FancyButton {
  base: button;
  color: red;
}
```

Now `<FancyButton />` renders into `<button />`:

```js
import {FancyButton} from './styles.react.css'

<FancyButton /> // => <button class="<autogenerated classname>">...</button>
```

### Composite components

In fact any React components which accepts `className` props can be used as a
base. That's means that React CSS Components can be used as theming tool for any
UI library.

Example:

```css
DangerButton {
  base: react-ui-library/components/Button;
  color: red;
}
```

## Variants

Variants is a mechanism which allows to define styling variants for a component.

### Named variants

You can define additional styling variants for your components:

```css
Label {
  color: red;
}

Label:emphasis {
  font-weight: bold;
}
```

They are compiled as CSS classes which then can be controlled from JS via
`variant` prop:

```js
<Label variant={{emphasis: true}} /> // sets both classes with `color` and `font-weight`
```
### Variants controlled with JS expression

You can define variants which are conditionally applied if JS expression against
props evaluates to a truthy value.

Example:

```css
Label {
  color: red;
}

Label:prop(mode == "emphasis") {
  font-weight: bold;
}
```

Note that any free variable reference a member of `props`, thus in JS `mode`
becomes `props.mode` in the example above.

They are compiled as CSS classes as well. JS expressions within `prop(..)` are
used to determine if corresponding CSS classes should be applied to DOM:

```js
<Label mode="emphasis" /> // sets both classes with `color` and `font-weight`
```

## TODO

* [ ] Document how to add PostCSS transform to build pipeline (think autoprefixer).
* [ ] Document how to do CSS extraction.

[CSS modules]: https://github.com/css-modules/css-modules
