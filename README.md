# WebGl 2D Scatterplot with Regl

[![npm version](https://img.shields.io/npm/v/regl-scatterplot.svg)](https://www.npmjs.com/package/regl-scatterplot)
[![node stability](https://img.shields.io/badge/stability-experimental-EC5314.svg)](https://nodejs.org/api/documentation.html#documentation_stability_index)
[![build status](https://travis-ci.org/flekschas/regl-scatterplot.svg?branch=master)](https://travis-ci.org/flekschas/regl-scatterplot)
[![code style prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg)](https://github.com/prettier/prettier)
[![regl-scatterplot demo](https://img.shields.io/badge/demo-online-6ae3c7.svg)](https://flekschas.github.io/regl-scatterplot/)

> A highly scalable scatterplot rendered with WebGL using [Regl](https://github.com/regl-project/regl)

## Install

```
npm -i regl-scatterplot
```

## Getting started

```javascript
import createScatterplot from "regl-scatterplot";

const canvas = document.querySelector("#canvas");

const { width, height } = canvas.getBoundingClientRect();

const scatterplot = createScatterplot({
  canvas,
  width,
  height,
  pointSize: 5
});

const points = new Array(10000)
  .fill()
  .map(() => [-1 + Math.random() * 2, -1 + Math.random() * 2, color]);

scatterplot.draw(points);
```

See a complete example at [example/index.js](example/index.js).

## API

### Constructors

#### `const scatterplot = createScatterplot(options)`

**Returns:** a new scatterplot instance.

**Options:** is an object that accepts the following properties:

- `regl` a Regl instance to be used for rendering.
- `background` background color of the scatterplot.
- `canvas` canvas element.
- `colors` colormap.
- `pointSize` size of the points.
- `pointSizeSelected` additional size of the points when being selected. I.e., if set to `0` selected and deselect points have the same size.
- `pointOutlineWidth` width of the point outline when a point is being selected.
- `width` width of the canvas element.
- `height` height of the canvas element.
- `target` location that the camera should look at.
- `distance` distance of the camera to the target.
- `rotation` rotation of the camera around the target.
- `view` view matrix defining `target`, `distance`, and `rotation`. When given `target`, `distance`, and `rotation` are ignored.

#### `const regl = createRegl(canvas)`

**Returns:** a new Regl instance with appropriate extensions being enabled.

**Canvas:** the canvas object on which the scatterplot will be rendered on.


#### `const texture = createTextureFromUrl(regl, url, isCrossOrigin)`

**Returns:** a new [Regl texture](https://github.com/regl-project/regl/blob/gh-pages/API.md#textures) from a remote image.

**regl:** the Regl instance associated to your scatterplot instance. Either use [`createRegl()`](#const-regl--createreglcanvas) or `scatterplot.regl`;

**url:** the URL to an image.

**isCrossOrigin:** if `url` is pointing to another origin `isCrossOrigin` must be set to `true`.


### Properties

#### `scatterplot.canvas`

The canvas element on which the scatterplot is rendered.

#### `scatterplot.regl`

The Regl instance which renderes the scatterplot.

#### `scatterplot.version`

The version number of the scatterplot.


### Methods

#### `scatterplot.draw(points)`

Sets and draws `points`, which must be an array of points where a point is interpreted as a quadruple of form `[x, y, category, value]`.

**Examples:**

```javascript

const points = [
  [
    // The X position
    2,
    // The Y position
    1,
    // The category, which defaults to `0` if `undefined`
    0,
    // Some value, which defaults to `0` if `undefined`
    0.5
  ]
];

scatterplot.draw(points);

// You can now do something else like changing the point size etc.

// Lets redraw the scatterplot. Since `draw` is caching the points you don't
// have to specify the points here again if they didn't change.
scatterplot.draw();


// Lets actively unset the points. Since `draw()` assumes that you want to
// redraw existing points you have to actively pass in an empty array
scatterplot.draw([]);
```

#### `scatterplot.attr(arg)`

Setting or getting attributes. If `arg` is a string then one of the attributes listed below will be returned. If `arg` is a object of key-value pairs then those attributes will be set.

**Attributes:**

| Name        | Type   | Example | Nullifiable |
|-------------|--------|---------|-------------|
| width       | number | 300     | false       |
| height      | number | 200     | false       |
| aspectRatio | number | 1.0     | false       |

**Examples:**

```javascript
// Set width and height
scatterplot.attr({ width: 300, height: 200 });

// get width
const width = scatterplot.attr('width');

// Set the aspect ratio of the scatterplot. This aspect ratio is referring to
// your data source and **not** the aspect ratio of the canvas element! By
// default it is assumed that your data us following a 1:1 ratio and this ratio
// is preserved even if your canvas element has some other aspect ratio. But if
// you wanted you could provide data that's going from [0,2] in x and [0,1] in y
// in which case you'd have to set the aspect ratio as follows to `2`.
scatterplot.attr({ aspectRatio: 2.0 });
```

Nullifiable: an attribute is considered _nullifiable_ if you can unset it. Attributes that are not _nullifiable_ can be used, i.e., if you call `scatterplot.attr({ width: 0 });` will not change the width as `0` is interpreted as a falsey value.

#### `scatterplot.style()`

Setting or getting styles. If `arg` is a string then one of the attributes listed below will be returned. If `arg` is a object of key-value pairs then those attributes will be set.

**Attributes:**

| Name              | Type            | Default          | Constraints            | Nullifiable |
|-------------------|-----------------|------------------|------------------------|-------------|
| background        | string or array | rgba(0, 0, 0, 1) | hex, rgb, rgba         | `false`     |
| backgroundImage   | function        | `undefined`      | Regl texture           | `true`      |
| colorBy           | string          | `undefined`      | `category` or `value`  | `true`      |
| colors            | array           | _see below_      | list of hex, rgb, rgba | `false`     |
| opacity           | number          | `1`              | > 0                    | `false`     |
| pointOutlineWidth | number          | `2`              | >= 0                   | `false`     |
| pointSize         | number          | `6`              | > 0                    | `false`     |
| pointSizeSelected | number          | `2`              | >= 0                   | `false`     |

**Notes:**

- The background of the scatterplot is transparent, i.e., you have to control
  the background with CSS! `background` is used when drawing the
  outline of selected points to simulate the padded border only.

- The background image must be a Regl texture function. To easily set a remote
  image as the background please use [`createTextureFromUrl`](#const-texture--createTextureFromUrlregl-url-isCrossOrigin).

- The scatterplot stores 4 colors per color representing 4 states, representing:

  - normal: used by default [default: `[0.66, 0.66, 0.66, 1]`]
  - active: used when selecting a point [default: `[0, 0.55, 1, 1]`]
  - hover: used when mousing over a point _not implemented yet_ [default: `[1, 1, 1, 1]`]
  - background: used as the background color [default: `[0, 0, 0, 1]`]

  When defining colors you can either pass in a flat array of _normal_ colors
  and regl-scatterplot will fill in the rest or you provide a list of quadruples
  where each quadruple defines the colors for all 4 states.

- Points can currently by colored by _category_ and _value_.

- The size of selected points is given by `pointSize + pointSizeSelected`

**Examples:**

```javascript
// Set background color red
scatterplot.style({ background: '#00ff00' });
scatterplot.style({ background: [255, 0, 0] });
scatterplot.style({ background: [255, 0, 0, 1.0] });
scatterplot.style({ background: [1, 0, 0, 1.0] }); // normalized rgba

// Set background image to an image with the same origin
scatterplot.style({ backgroundImage: 'my-image.png' });
// Set background image to an image with a different origin
scatterplot.style({ backgroundImage: { src: 'https://server.com/my-image.png', crossOrigin: true } });
// Set background image to some regl texture
const image = new Image();
image.src = 'my-image.png';
image.onload = () => { scatterplot.style({ backgroundImage: regl.texture(image) });

// Color by
scatterplot.style({ colorBy: 'category' });

// Set color map
scatterplot.style({ colors: ['#ff0000', '#00ff00', '#0000ff'] });

// Set base opacity
scatterplot.style({ opacity: 0.5 });

// Set the width of the outline of selected points
scatterplot.style({ pointOutlineWidth: 2 });

// Set the base point size
scatterplot.style({ pointSize: 10 });

// Set the additional point size of selected points
scatterplot.style({ pointSizeSelected: 2 });
```


#### `scatterplot.select(points)`

Select some points, such that they get visually highlighted. This will trigger
a `select` event.

#### `scatterplot.deselect()`

Deselect all selected points. This will trigger a `deselect` event.

#### `scatterplot.destroy()`

Destroys the scatterplot instance by disposing all event listeners, the pubSub
instance, regl, and the camera.

#### `scatterplot.refresh()`

Refreshes the viewport of the scatterplot's regl instance.

#### `scatterplot.reset()`

Sets the view back to the initially defined view.

#### `scatterplot.subscribe(eventName, eventHandler)`

Subscribe to an event.

**eventName** needs to be one of the following events:

- `select` [payload: `{ points }`]: broadcasted when points are selected
- `deselect` [payload: `undefined`]: broadcasted when points are deselected
- `view` [payload: camera view matrix]: broadcasted when the view changes

**eventHandler** needs to be a callback function that can receive the payload.

#### `scatterplot.unsubscribe(eventName, eventHandler)`

Unsubscribe from an event. See [`scatterplot.subscribe()`](#scatterplot.subscribe) for a list of all
events.
