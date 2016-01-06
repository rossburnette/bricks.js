# Bricks.js

[![Bricks.js on NPM](https://img.shields.io/npm/v/bricks.js.svg)](https://www.npmjs.com/package/bricks.js)

> Momma said, "Stay patient." - Bricks, DJ Carnage

But you don't need to, because Bricks is **a blazing fast masonry layout generator for fixed width elements**.

* [Demo](http://callmecavs.github.io/bricks.js/)

## Quick Start | [Skip](#usage)

```es6
// import Bricks
import bricks from 'bricks.js'

// create an instance
const instance = bricks({
  container: '.container',
  packed: 'data-packed',

  // define your grid at different breakpoints
  sizes: [
    { columns: 2, gutter: 10 },
    { mq: '768px', columns: 3, gutter: 25 },
    { mq: '1024px', columns: 4, gutter: 50 }
  ]
})

// bind callbacks
instance
  .on('pack', () => console.log('All grid items have been packed.'))
  .on('update', () => console.log('New grid items have been packed.'))
  .on('resize', (size) => console.log('The grid has be re-packed to accommodate a new breakpoint.'))

// start it up
instance
  .resize()     // bind resize handler
  .pack()       // pack initial items

// add new items via AJAX
fetch('path/to.html')
  .then(response => response.text())
  .then(html => {
    document.querySelector('.container').appendChild(html)

    // position them within the existing grid
    instance.update()
  })
```

## Usage

Bricks was developed with a modern JavaScript workflow in mind. To use it, it's recommended you have a build system in place that can transpile ES6, and bundle modules. For a minimal boilerplate that does so, check out [outset](https://github.com/callmecavs/outset).

Follow these steps to get started:

* [Install](#install)
* [Instantiate](#instantiate)
  * [Parameters](#parameters)
* [API / Events](#api--events)

### Install

Using NPM, install Bricks.js, and add it to your `package.json` dependencies.

```
$ npm install bricks.js --save
```

### Instantiate

Simply import Bricks, then instantiate it.

It's recommended that you **assign your Bricks instance to a variable**. Using your instance, you can enable the resize handler, bind callback handlers, and accommodate dynamically added elements.

Parameters passed to the constructor are detailed below.

```es6
// import Bricks
import bricks from 'bricks.js'

// create an instance
const instance = bricks({
  container: '.container',
  packed: 'data-packed',
  sizes: [
    { columns: 2, gutter: 10 },
    { mq: '768px', columns: 3, gutter: 25 },
    { mq: '1024px', columns: 4, gutter: 50 }
  ]
})
```

#### Parameters

Note that **all parameters are required**:

* A [container](#container) selector
* A [packed](#packed) attribute
* A [sizes](#sizes) array

##### container

A CSS selector that matches the grid wrapper.

Note that the _direct children of this element must be the grid items_.

```es6
const instance = bricks({
  container: '.selector'
})
```

##### packed

An attribute added to items positioned within the grid.

Note that if the attribute is not prefixed with `data-`, it will be added.

```es6
const instance = bricks({
  packed: 'data-packed'
})
```

##### sizes

An array of objects describing the grid's properties at different breakpoints.

When defining your sizes, note the following:

* Sizes must use _`min-width` media queries_
* Sizes must be listed _smallest to largest_

The size object without the `mq` property is assumed to be your smallest breakpoint, and must appear first.

```es6
// columns - the number of vertical columns
// gutter  - the space (in px) between the columns and grid items

const instance = bricks({
  sizes: [
    { columns: 2, gutter: 10 },
    { mq: '768px', columns: 3, gutter: 25 },
    { mq: '1024px', columns: 4, gutter: 50 }
  ]
})
```

### API / Events

Note that **all methods, including those from the event emitter, are chainable**.

Bricks instances are extended with [Knot.js](https://github.com/callmecavs/knot.js), a browser-based event emitter. Use the event emitter syntax to add and remove callbacks associated with the API methods.

API methods, and their corresponding events, are described below:

#### .pack()

Used to pack all elements within the container.

Note that it needs to be called after creating your instance, to pack the initial items.

```es6
// pack all items, using an existing instance
instance.pack()

// fired when all items have been packed
instance.on('pack', () => {
  // ...
})
```

#### .update()

Used to handle dynamically added elements.

Note that `update` is the preferred method for positioning new items within the grid, _assuming the media query hasn't changed_, because it will only operate on items that have not yet been packed (i.e. don't have the `packed` attribute).

```es6
// call the update method, using an existing instance
instance.update()

// fired when newly added elements have been packed
instance.on('update', () => {
  // ...
})
```

#### .resize()

Used to bind the `resize` handler to the `window` resize event. It should be called _only once_, when creating your instance.

Note that the resize handler fires the `pack` method _if the resulting screen size matches a size parameter other than the current one_.

```es6
// bind the resize handler, using an existing instance
instance.resize()

// fired when resizing has caused the items to be re-packed
// the pack event is also fired at this time - use the resize event ONLY for breakpoint specific code
instance.on('resize', (size) => {
  // 'size' is the newly matching size object
  // ...
})
```

## Browser Support

Bricks depends on the following browser APIs:

* ES5 array methods: [forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach), [map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map), [indexOf](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)
* [requestAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame)
* [matchMedia](https://developer.mozilla.org/en-US/docs/Web/API/Window/matchMedia)

Consequently, it supports the following natively:

* Chrome 24+
* Firefox 23+
* Safari 6.1+
* Opera 15+
* IE 10+
* iOS Safari 7.1+
* Android Browser 4.4+

To support older browsers, consider including [polyfills/shims](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills) for the APIs listed above. There are **no plans to include any in the library**, in the interest of file size.

## License

MIT. © 2016 Michael Cavalea

[![Built With Love](http://forthebadge.com/images/badges/built-with-love.svg)](http://forthebadge.com)
