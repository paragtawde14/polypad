# Polypad Changelog

## v1.5

### Breaking Changes

* The `'move'` event is now triggered continuously, rather than throttled to once every second.

### New Features

* New `noPanAndZoom` parameter when setting up a Polypad instance.
* Support text selection within equation editor

### Fixes

* Fixed ID generation and change events for Abakus and Equation tiles
* Fixed positioning of tile settings panels (e.g. for numerline)
* Trigger `change` when erasing with a single click (rather than click and drag)
* Trigger `delete`, rather than `change`, when deleting a text box by making its content `''`.
* Don't trigger a `change` event when adding a new Numberline or Axes tiles
* Don't close colour or settings popups when clicking on a button inside
* Fix motion of geometry utensil handles, range of colour picker sliders
* Fix equation editor cursor bugs, nicer styling for Abakus tiles


## v1.4

### New Features

* Abacus tool in sidebar
* Equation editor (option for "text box" tool)
* Pan tool (under zoom in/out)

### Fixes

* Improved accessibility for fractions
* Trigger "add" not "change" event when dragging tiles from the sidebar
* Fixed analytics scripts
* Fixed black background in fullscreen mode
* Fixed duplicate tile IDs when using copy+paste
* Trigger "add" event when drawing a single-dot stroke
* Fix colour for Tantrix tiles
* Fix cursor bugs in some browsers
