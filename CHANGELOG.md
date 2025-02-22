## v1.6.3

- Fix an issue with repeated zooming to points
- Improve lasso on long press start

## v1.6.2

- Fix a build regression from updating to Rollup v3

## v1.6.1

- Update third party libraries
- Improve lasso long press indicator styling
- Fix an issue where a lasso with less than three control points

## v1.6.0

- Add the ability to filter down points via `scatterplot.filter(pointIdxs)`. This can be useful if you need to temporarily need to hide points without having to re-instantiate the regl-scatterplot instance. E.g., when calling `scatterplot.filter([0, 1, 2])`, only the first, second, and third point will remain visible. All other points (and their related point connections) will be visually and interactively hidden.

  To reset the filter call `scatterplot.unfilter()` or `scatterplot.filter([])`.

  https://user-images.githubusercontent.com/932103/222810324-3e048176-fd1d-4ede-a836-511c548f09ff.mp4

- Add the ability to retrieve selected and filtered point indices via `scatterplot.get('selectedPoints')` and `scatterplot.get('filteredPoints')` respectively.

## v1.5.1

- Refactor lasso manager to support SSR ([#101](https://github.com/flekschas/regl-scatterplot/issues/101))
- Fix a glitch where the scatterplot instance is destroyed after a `scatterplot.draw()` was called but before `scatterplot.draw()` finished. ([#101](https://github.com/flekschas/regl-scatterplot/issues/101))

## v1.5.0

- Add the ability to lasso select point upon long press via `scatterplot.set('lassoOnLongPress', true)`.
- Fix type issues ([#98](https://github.com/flekschas/regl-scatterplot/issues/98))

## v1.4.2

- Enforce the canvas width and height to be at least 1px to prevent view operations from breaking.

## v1.4.1

- Bump `dom-2d-camera` to publish view updates on camera tweens ([#95](https://github.com/flekschas/regl-scatterplot/issues/95))

## v1.4.0

- Add zooming via the following four methods. All four methods supported animated transition just like `draw()`.
  1. `scatterplot.zoomToLocation(target, distance)` for zooming to a specific point location
  2. `scatterplot.zoomToArea(rectangle)` for zooming to an area specified by a rectangular bounding box
  3. `scatterplot.zoomToPoints(pointIndices)` for zooming to a set of points
  4. `scatterplot.zoomToOrigin()` for zooming to the origin

## v1.3.2

- Add `scatterplot.isSupported` and `renderer.isSupported` as read-only properties to expose if all GL extensions are supported and enabled in the user's browser (#90)
- Add `checkSupport()` as a globally exported function for users to check if their browser supports and has enabled all required GL extensions (#90)

## v1.3.1

- Add a missing `select` event name to the type definition (#87)

## v1.3.0

- Add properties `opacityInactiveScale` and `opacityInactiveMax` to enable de-emphasizing unselected points and highlight selected points. The final point opacity is now set to `min(opacityInactiveMax, currentOpacity) * opacityInactiveScale` when at least one point is selected. By default `opacityInactiveScale` and `opacityInactiveMax` are set to `1`. I.e., the default behavior did not change.

## v1.2.3

- Properly initialize new x/y scales upon calling `set()`

## v1.2.2

- Fix a minor issue in `destroy()` that prevented disconnecting resize listeners

## v1.2.1

- Update dom-2d-camera to fix internal tests

## v1.2.0

- Outsource WebGL renderer to improve instancing: You can now use a single shared WebGL renderer (via `createRenderer()`) to power multiple scatter plot instances. See https://flekschas.github.io/regl-scatterplot/multiple-instances.html.
- Add `scatterplot.redraw()` to enforce a redrawing of the scene. This can be necessary when you manually updated the camera.
- Add `scatterplot.view(cameraView, { preventEvent })` as a shorthand for setting the scatter plot's camera view. The `preventEvent` option enables synchronizing multiple scatter plot instances. See https://flekschas.github.io/regl-scatterplot/multiple-instances.html.
- Allow passing typed arrays to `scatterplot.draw({ x, y, z, w })`.

**Breaking changes:**

- `scatterplot.export()` is now returning an [ImageData](https://developer.mozilla.org/en-US/docs/Web/API/ImageData) object for better utility.

## v1.1.1

- Fix incorrect opacity handling (#74)

## v1.1.0

- Add `scatterplot.get('pointsInView')` to retrieve the indices of the points currently visible within the view (#72). Shoutout to [@dulex123](https://github.com/dulex123) for his PR!
- Add `scatterplot.get('points')` to retrieve all currently drawn points.
- Add an example for demonstrating how labels can be rendered. The demo using `scatterplot.get('pointsInView')` to determine when to render text labels. See https://flekschas.github.io/regl-scatterplot/text-labels.html

## v1.0.0

Woohoo 🥳 It's time to release v1! Nothing dramatic changed in this release but I felt that the library/API is now stable enough. Also, a big shoutout to [@manzt](https://github.com/manzt) and [@japrescott](https://github.com/japrescott) for their PRs.

- Support drawing columnar data `scatterplot.draw({ x: [...], y: [...], ... })` (#59)
- Fix the rendering issue with with Safari 14 (#45)
- Replace webpack with vite (#64)

## v0.19.2

- Fix an issue with not properly publishing the pointout event (#58)

## v0.19.1

- Simplify drawing loop by using `regl.frame` and bailing out of when nothing needs to be redrawn
- Update regl-line to be compatible with regl v2

## v0.19.0

- Ensure inter buffers and viewports are properly updated on every draw call.

**Breaking changes:**

- Improve the handling of the connection order by switching from a structured 5th component to a 6th component.

  Previously to provide a specific order, the 5th component had to be a tuple of `[lineComponentId, orderIndex]`. With this update the 5th component will always just be `lineComponentId` and the 6th will be `orderIndex`.

## v0.18.7

- Avoid text selection when lassoing

## v0.18.6

- Optimize density-based opacity encoding

## v0.18.5

- Fix an issue with the lasso position when the page is scrollable (#50)

## v0.18.4

- Fix an issues when programmatically trying to `select()` or `hover()` non-existing points

## v0.18.3

- Harmonize `hover(pointIdx, { showReticleOnce, preventEvent })` with `select()` API by allowing it to prevent the `pointover` and `pointout` from being published.

## v0.18.2

- Add two events: `init` and `destroy`
- Add `lassoLineWidth` to allow adjusting the lasso line width
- Fix an issue with normalizing RGBA values
- Fix a camera issue when lassoing in non-`panZoom` mouse mode

## v0.18.1

- Rename `showRecticle` to `showReticle` and `recticleColor` to `reticleColor` (#47)
- Fix reticle glitches by updating `regl-line` (#46)

## v0.18.0

- Add density-based dynamic point opacity via `opacityBy: 'density'`. See
  [dynamic-opacity.html](https://flekschas.github.io/regl-scatterplot/dynamic-opacity.html) for an example.

## v0.17.1

- Add [`scatterplot.export`](README.md#scatterplot.export) for exporting the current view as pixels.
- Add default canvas width/height (`'auto'`) to fix an issue on high-dpi monitors that do not style the canvas element (#41)
- Add resize handler so when width/height is set to `'auto'` the canvas element is automatically resized on `resize` and `orientationchange` events.
- Improve framebuffer rendering and add `gamma` prop to allow controlling the opacity blending (#42)

## v0.17.0

- Add the ability to color point connections by their segment via `pointConnectionColorBy: 'segment'`
- Fix an issue with normalizing RGB(A) values
- Avoid the unnecessary use of the logical-assignment-operators (#39)

## v0.16.2

- Stop calling `camera.refresh()` as that is unnecessary since `v1.2.2`
- Avoid empty lasso events
- Fix incorrectly initialized `pointConnectionColorBy`, `pointConnectionOpacityBy`, and `pointConnectionSizeBy`
- Fix an issue with the color conversion to RGBA
- Fix an issue blurring an active point connection

## v0.16.1

- Allow inheriting `pointConnectionColor` from `pointColor` by setting `pointConnectionColor: 'inherit'`. Same for `pointConnectionColorActive` and `pointConnectionColorHover`.
- Fix incorrectly initialized `opacity`
- Fix incorrectly initialized `colorBy`, `opacityBy`, and `sizeBy`
- Fix not assigned `pointConnectionOpacity`
- Rename `pointConnectionOpacitySelection` and `pointConnectionSizeSelected` to `pointConnectionOpacityActive` and `pointConnectionSizeActive` for consistency

## v0.16.0

- Allow the point-associated data values to be either categorical or continuous instead of enforcing one to be categorical and the other one to be continuous. For continuous data, assign [0,1]-normalized data as the third or forth component of a point. For categorical data, assign an integer-based data. For instance, if a point looks `[1, 2, 3, 4]` we assume that the first (`3`) and second (`4`) data value is categorical. If you would instead have points like `[1, 2, 0.33, 0.44]` where the third and forth component are within [0,1] then we would assume that those components represent continuous data. For backward compatibility, `colorBy: 'category'` refers to coloring by the third component and `colorBy 'value'` refers to the forth component. Additionall, you can now reference the third components via `value1`, `valueA`, `valueZ`, or `z` and the forth component via`value2`, `valueB`, `valueW`, or `w`.

- Add the ability to connect points visually via `draw(points, { connectPoints: true })`. This assumes that your points have a 5th component that identifies the point connection. Currently the order of points as they appear in `points` defines the order of how they would be connected. So assuming you have:

  ```javascript
  // prettier-ignore
  scatterplot.draw([
    1, 1, 0, 0, 0,
    2, 2, 0, 0, 0,
    3, 3, 0, 0, 1,
    4, 4, 0, 0, 1,
    5, 5, 0, 0, 0,
  ], { connectPoints: true });
  ```

  Then we would connext the points as follows:

  ```
  1 -> 2 -> 5
  3 -> 4
  ```

  For an example see [example/connected-points.js](example/connected-points.js).

- Add support for multi-selections by holding down <kbd>CMD</kbd> (by default) during click- or lasso-selections. The modifier key for multi-selections can be adjusted via `scatterplot.set({ keyMap: { merge: 'ctrl' } })`
- Add `performanceMode` to allow drawing up to 20 million points
- Add `opacityBy` to allow encoding one of the two data properties as the point opacity.

- Fix an issue with the point size between devices with different pixel ratios
- Fix an issue with detecting points when using variable point sizes
- Fix an issue that drew the background image before it was loaded

**Breaking changes:**

- Removed the following deplicated properties:
  - `background` (use `backgroundColor` instead)
  - `distance` (use `cameraDistance` instead)
  - `rotation` (use `cameraRotation` instead)
  - `target` (use `cameraTarget` instead)
  - `view` (use `cameraView` instead)

## v0.15.1

- Make sure the `keyMap` is properly initiated.
- Fix a memory leak by properly destroying the camera on `scatterplot.destroy()`.
- Add test for rotation, key mapping, and the lasso initiator

## v0.15.0

- Add `mouseMode` for switching betweem mouse modes programmatically. Currently supported modes are `panZoom`, `lasso`, and `rotate`.
- Add `keyMap` for remapping modifier key-induced actions. Available modifier keys are `alt`, `shift`, `ctrl`, `cmd`, and `meta`. Available actions are `lasso` and `rotate`.
- Add `lassoInitiator` (boolean) for enabling a way to lasso points without having to use a modifier key. When activated, you can click into the void and a circle will appear. You can then start lassoing by mousing down + holding onto the circle and dragging. Since clicking into the void can be challenging when working with many points you can also long clicking anywhere and the circle for initiating the lasso will appear anywhere.
- Improve point selection

## v0.14.0

- Add `lassoStart`, `lassoExtend`, and `lassoEnd` events

**Breaking changes:**

- Renamed event `background-image-ready` to `backgroundImageReady` for consistency
- Switched to asynchronously broadcasted events to decouple regl-scatterplot's execution flow from the event handler. You can switch back to the old behavior if you like by initializing the scatterplot via `createScatterplot({ syncEvents: true })`

## v0.13.0

- Add support for transitioning points via `scatterplot.draw(updatedPoints, { transition: true})`
- Add support for size encoding the points' category or value via `scatterplot.set({ sizeBy })`

## v0.12.0

- Add `deselectOnDblClick` and `deselectOnEscape` to allow disabling deselection on double click or escape when set to `false` (#31)
- Fix an issue updating the x and y scale domains (#30)

## v0.11.0

- Allow synchronizing D3 x and y scales with the scatterplot view. See [README.md](README.md#synchronize-d3-x-and-y-scales-with-the-scatterplot-view) for more details. (#29)

## v0.10.1

- Make sure `backgroundImage` supports base64-encoded images
- Fix missing texture destruction before recreating the texture (#22)

## v0.10.0

- Add `lassoClearEvent` property to allow customizing when the lasso is cleared.
- Add `preventEvent` option to `scatterplot.select()` and `scatterplot.deselect()` to prevent publishing the `select` and `deselect` events
- Add Promise-based return value to `scatterplot.draw()` to enable the parent application to determine when the points were drawn
- Add support for `get('lassoMinDelay')` and `get('lassoMinDist')`

## v0.9.1

- Only listen on mouse down events within the instance's canvas element (#16)

## v0.9.0

- Renames `target`, `distance`, `rotation`, and `view` to `cameraTarget`, `cameraDistance`, `cameraRotation`, and `cameraView`
- Add getter and setter for `cameraTarget`, `cameraDistance`, `cameraRotation`, and `cameraView`
- Fix setting initial camera position (#15)
- Improve documentation on how to color points (#14)

## v0.8.0

- Add background to lasso
- Fix horizontally-flipped background image (#13)
- Rename the `background` property to `backgroundColor` for clarity
- Split the `colors` property into `pointColor`, `pointColorActive`, and `pointColorHover` for clarity
- Update camera

## v0.7.6

- Increase floating point precision
- Fix #5

## v0.7.5

- Add `clear()` to clear the scatter plot
- Update the camera on refresh
- Fix issue when drawing new points: do not wrap setting new points with withRaf(). Only wrap the pure draw call!
- Fix a regression from removed scroll library

## v0.7.4

- Increase floating point precision (#5)
- Fix a rare glitch in the lasso selection where the lasso would be drawn with a far away point
- Smoothify lasso by lowering the min delay and min dist
- Update 2D camera and many dev packages
- Remove the minified ESM build as it's unnecessary

## v0.7.3

- Fix glitch in the npm release of `v0.7.2`.

## v0.7.2

- Provide proper ESM instead of pointing to the source code.

## v0.7.1

- Replaced `hover` event with `pointover` and `pointout` to be able to know when a point is not hovered anymore.

## v0.7.0

- Allow changing the lasso smoothness via `set({ lassoMinDelay, lassoMinDist })` where `lassoMinDelay` is the minimum number of milliseconds between mousemove events before the lasso is extended and `lassoMinDist` is the minimum number of pixels the mouse has to move.

## v0.6.0

- Simplify API: `style()`, `attr()`, `scatterplot.canvas`, `scatterplot.regl`, and `scatterplot.version` are merged into `get()` and `set()`. The function signature is identical to `style()` and `attr()` so all you have to do is rename.
- Add recticle. It's not shown by default but can be activated with `set({ showRecticle, recticleColor })`.
- Fix a regression that caused interrupted panning

## v0.5.1

- Fix a bug in categorical color encoding

## v0.5.0

- Set default aspect ratio to 1. It can be changed via `attr({ aspectRatio })`
- Add property to set `lassoColor` via `style({ lassoColor })`
- Expose helper (`createTextureFromUrl`) for creating a texture from an image URL
- Expose regl instance via `scatterplot.regl`
- Replace `mouse-position` and `mouse-pressed` with internal code
- Avoid click selections upon mousedown + mousemove + mouseup
- Add tests for all public API endpoints
- Fix several smaller bugs

## v0.4.0

- Use a combination of linear and log2 scaling for point size
- Add support for background images
- Add API documentation
- Switch to single quotes
- Export version

## v0.3.3

- Add endpoint for changing the background color
- Allow setting view on initialization
- Remove event listeners on `destroy()`
- Rename `camera` event to `view` and publish the view matrix
- Fix issues with setting colors
- Fix resetting view

## v0.3.2

- Update third party libraries
- Switch to browser-based tests
- Set more strict linting

## v0.3.1

- Fix nasty floating point issue when working with large textures (> 100.000 points)
- Make point size dependent on zoom level

## v0.3.0

- Optimize rendering: up to about 500K points render fine. Usable for up to 1M points.
- Add support for one categories and one value per point for color encoding.
- Add visual outline for selected points for better highlighting.
- Add test setup and some base tests.
- Many bug fixes and under the hood improvements.

## v0.2.0

- Add fast lasso selection
- Support rotations

## v0.1.0

- Initial working version. **Warning:** this version is not optimized yet and only works fluidly for up to 50.000 points.
