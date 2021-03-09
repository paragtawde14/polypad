# Polypad API Docs

## Setup

Our JavaScript API allows you to add interactive Polypad canvases to any website. You simply need to include our JS source file, create a parent element for Polypad, and then call `Polypad.create()`:

```html
<script src="https://static.mathigon.org/api/polypad-v1.0.js"></script>
<div id="polypad" style="width: 800px; height: 500px;"></div>
<script>Polypad.create(document.querySelector('#polypad'))</script>
```


## JSON Schema

Every Polypad canvas consists of a collection of __tiles__ and __strokes__. The state of a Polypad can be serialised as JSON, with the following schema:

```ts
interface TileData {
  name: string,       // The tile type (since 'type' is a reserved field in many databases)
  options?: string,   // Parameters for this tile
  id?: string;        // Unique identifier
  x?: number,         // X-position
  y?: number,         // Y-position
  rot?: number,       // Rotation (in degrees)
  size?: number;      // Used for some resizable tiles like images or text boxes
  colour?: string;    // HEX colour (e.g. "#ff0044")
  flipped?: boolean;
  locked?: boolean;
}

interface StrokeData {
  id?: string;         // Unique identifier
  points: string;      // SVG path
  colour?: string;     // HEX colour
  brush: 'pen'|'marker'|'highlighter'
}

interface PolypadData {
  title: string;         // Polypad title
  background: string;    // HEX Background color (not currently supported!)
  grid: 'none'|'square2-grid'|'square-dots'|'square-grid'|'tri-dots'|'tri-grid'|'tri2-dots'|'tri2-grid';
  noLabels: boolean;
  tiles: TileData[];
  strokes: StrokeData[];
}
```

Please that the maximum length of the `tiles` and `strokes` array is 2000. With more items on the
same page, you might see performance issues on some devices.


## Tile Types

Polypad supports a large number of different tile types.

| Tile            | Name             | Options String |
| --------------- | ---------------- | -------------- |
| Polygons        | `polygon`        | Either a named polygon like `square`, `reg-hexagon` or `kite`, or a string of vertex coordinates, e.g. `0 0,1 0,1 1,0 1`|
| Custom Polygons | `custom-polygon` | _Same as for Polygon_ |
| Polyominoes     | `pentomino`      | Index from `0` to `11` for pentominoes and `12` to `16` for tetrominoes |
| Tangram         | `tangram`        | Index from `0` to `6` |
| Tangram Egg     | `egg`            | Index from `0` to `8` |
| Ruler           | `ruler`          | Width, e.g. `400` |
| Protractor      | `protractor`     | Width, e.g. `200` |
| Penrose Tiles   | `penrose`        | Either `0` or `1` |
| Penrose Nature  | `garden`         | Index from `0` to `7` |
| Kolam Tiles     | `kolam`          | Index from `0` to `5` |
| Fractals        | `fractal`        | Index from `0` (large) to `4` (small) |
| Tantrix Tiles   | `tantrix`        | Index from `0` to `13` |
| Number Tiles    | `number-tile`    | `${width}:${count}`, e.g. `10:100` for a 10x10 block of tiles |
| Number Bars     | `number-bar`     | Width from `1` to `10` |

TODO: Documentation for other tile types (`fraction-bar`, `fraction-circle`, `text`, `algebra-tile`, `grid`, `image`, `dice`, `coin`, `number-line`, `penrose`, `prime-disk`, `balance`, `decimal-grid`, `spinner`, `custom-spinner`, `token`, `geo`, `dot-machine`, `dot`, `axes`, `question-blank`, `equation`, `number-card`)


## Methods

The `Polypad.create()` function takes an options argument with many ways to customise Polypad:

```ts
interface Polypad {
  create: ({
    sidebar?: boolean;  // Whether to show the sidebar
    toolbar?: boolean;  // Whether to show the toolbar
    settings?: boolean;  // Whether to show the settings bar
    initial?: PolypadData;  // Initial data to show
    themeColours?: Record<string, string>;  // Override the default theme colours
    imageUpload?: (file: File) => Promise<string>;  // Provide a way to upload image files
  }) => PolypadInstance;
}
```

`Polypad.create()` then returns a `PolypadInstance` object with additional methods for manipulating the canvas. Please note that most of these methods don't type-check their input arguments. If you pass in incorrect data, the behaviour is undefined.

```ts
interface PolypadInstance {
  // Bind or unbind event listeners. See below for all supported events.
  on: (event: string, callback: (e: unknown) => void) => void;
  off: (event: string, callback: (e: unknown) => void) => void;

  // Serialize or un-serialize a polypad state from JSON data. See above for types.
  serialize: () => PolypadData;
  unSerialize: (data: PolypadData) => void;

  // Create a PNG image of a given size. This asynchronous function returns a Data URI string.
  pngImage: (width: number, height: number) => Promise<string>;

  // Add a new tile or stroke to the canvas, with a given JSON data. Returns the ID of the item.
  addTile: (data: TileData) => string;
  addStroke: (data: StrokeData) => string;

  // Updates an existing tile with a given ID. Note that you cannot change the 'name' of a tile.
  updateTile: (id: string, properties: TileData) => void;

  // Delete an existing tile or stroke.
  deleteTiles: (...tileIds: string[]) => void;
  deleteStrokes: (...strokeIds: string[]) => void;

  // Get the current user selection, or programmatically select multiple existing tiles.
  getSelection: () => string[];
  selectTiles: (...tileIds: string[]) => void;

  // Set the colour scheme of the canvas.
  setTheme: (theme: 'light'|'dark') => void;
}
```


## Events

Using the `.on()` and `.off()` methods on `PolypadInstance`, you can bind and unbind many different
event listeners. The following events are supported:

| Name          | Callback Options    | Description |
| ------------- | ------------------- | --------------------------------------------- |
| `add-tile`    | `{id: string}`      | Triggered whenever a user creates a new tile. |
| `add-stroke`  | `{id: string}`      | Triggered whenever a user draws a new stroke. |
| `change`      | `undefined`         | Triggered whenever the canvas changes.        |
| `select`      | `{ids: string[]}`   | Triggered whenever the selection changes.     |

TODO: Documentation for other events


## Requirements and Global Namespace Pollution

Including the Polypad API JS file creates a global `window.Polypad` variable and registers the `x-polypad`, `x-polypad-sidebar`, `x-polypad-toolbar` and `x-polypad-settings` custom HTML elements.

Polypad requires [Custom Web Components](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_custom_elements) and the [Web Animations API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Animations_API). If you want to use browsers that don't support these APIs, you have to include a polyfill, e.g. [mathigon.org/polyfill.js](https://mathigon.org/polyfill.js).

Our goal is to support the latest version of Chrome, Firefox, Opera and Edge on all mobile and desktop devices.


## Known Issues

* [ ] Add templates for other languages
* [ ] Expose additional, internal methods and events
* [ ] Customise which items to show in the sidebar, toolbar and settings menu
