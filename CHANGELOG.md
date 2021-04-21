# Polypad Changelog

## v1.6

### New Features

* Return full stroke data in `change` events (no more max-length). Add options for max number of tiles, strokes and stroke length in `.serialize()` method.
* Add `.preventDefault()` to `undo` and `redo` events.
* New `options` event for the alternate colours and number labels settings, as well as a `.setOptions()` method to change these values programatically.
* New `.toggleSidebar()` method to expand or collapse the sidebar.
* New `.addCustomButton()` method to create custom toolbar or settings bar buttons.
* Paste images directly from the clipboard onto the canvas.

### Fixes

* Don't delete tiles when clicking backspace inside an input field (e.g. the number line or exploding dots settings fields).
* Trigger correct event (`added`, not `changed`) when creating text or equation tiles.
* Fix bugs when changing the number of exploding dots machine boxes.
* Correctly center pasted tiles on the canvas, even when the sidebar is open.
* Fix some bugs when pressing escape or shift keys while dragging tiles.
* Fix erasing and positioning of fraction circle tiles.
* Fix change event data for Abacus tile and equation boxes


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
