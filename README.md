# object-fit-images

> Polyfill `object-fit` and `object-position` on images on IE9, IE10, IE11, Edge, Safari, ...

[![gzipped size](https://badges.herokuapp.com/size/github/bfred-it/object-fit-images/gh-pages/dist/ofi.browser.js?gzip=true&label=gzipped%20size)](#readme) [![Travis build status](https://api.travis-ci.org/bfred-it/object-fit-images.svg?branch=gh-pages)](https://travis-ci.org/bfred-it/object-fit-images) [![npm version](https://img.shields.io/npm/v/object-fit-images.svg)](https://www.npmjs.com/package/object-fit-images) 

This adds support of `object-fit` and `object-position` to **IEdge 9-13, Android 4.4-, Safari (OSX 9.1-, iOS 9.3-)** and skips browsers that already support them.

Take a look at the [demo.](http://bfred-it.github.io/object-fit-images/demo/) 

## Main features

- CPU-light code
- No additional elements are created or necessary
- Once set, position is taken care by the browser
- You can normally get and set the `<img>`'s `src` attribute: `img.src = 'other-image.jpg'`
- `srcset` support

## Comparison table with alternative solutions

### Support

|                                 | object-fit-images                                              | [tonipinel/object-fit-polyfill](https://github.com/tonipinel/object-fit-polyfill)           | [jonathantneal/fitie](https://github.com/jonathantneal/fitie)
:---                              | :---                                                           | :---                                                                                        | :---
Browsers                          | IEdge 9-14, Android < 5, Safari < 10                    | "All browsers"                                                                              | IE 8-11, Edge
Tags                              | `img`                                                          | `img`                                                                                       | `img`, `video`
`cover/contain`                   | 💚                                                              | 💚                                                                                           | 💚
`fill`                            | 💚                                                              | 💚                                                                                           | 💚
`none`                            | 💚                                                              | 💚                                                                                           | 💔
`scale-down`                      | 💚 [`{watchMQ:true}`](#media-query-affects-object-fit-value) suggested | 💔                                                                                           | 💔
`object-position`                 | 💚                                                              | 💔                                                                                           | 💔
`srcset` support                  | 💚 Native or [picturefill](https://github.com/scottjehl/picturefill), but [see notes](detailed-support-tables.md)                                                               | 💔                                                                                           | 💔
`picture` support                 | 💛 Exclusively where picturefill [acts*](detailed-support-tables.md#object-fit-images--picture) | 💔                                                                                           | 💔

Performance and ease of use considerations in [extended-comparison.md](extended-comparison.md)

## Usage

Because it's nearly impossible to read unsupported properties, `object-fit-images` reads the value of `object-fit` and `object-position` from the `font-family` property on `img`.

```css
.your-favorite-image {
	object-fit: contain;
	font-family: 'object-fit: contain;'
}
.your-second-favorite {
	object-fit: contain;
	object-position: bottom;
	font-family: 'object-fit: cover; object-position: bottom'
}
```

This has no effect on the rendering because it's ignored by the browser.

The `font-family` property could be generated automatically with the [**PostCSS plugin**](https://github.com/ronik-design/postcss-object-fit-images) or with the **SCSS/SASS/Less** mixins in the [`preprocessor`](/preprocessors) folder.

### JS

Fix all the images on the page, present and future (auto mode)

```js
objectFitImages();
// but you should still run it on DOM ready or at the bottom of the page
// if you use jQuery, the code is: $(objectFitImages);
```

Alternatively, just fix them once. The first parameter can be:

```js
// a selector
objectFitImages('img.some-image');

// an array/NodeList
var someImages = document.querySelectorAll('img.some-image');
objectFitImages(someImages);

// a single element
var oneImage = document.querySelector('img.some-image');
objectFitImages(oneImage);

// or with jQuery
$('img.some-image').get().forEach(objectFitImages);
```

You can run `objectFitImages()` on the same elements more than once without issues (for example if you decide to change anything on resize)

#### Media query affects object-fit value

If your media queries change the value of `object-fit`, like this:

```css
                            img { object-fit: cover }
@media (max-width: 500px) { img { object-fit: contain } }
```

... then you need to enable the media queries support like this:

```js
objectFitImages('img.some-image', {watchMQ: true});
// or objectFitImages(false, {watchMQ: true}); // for the auto mode
```

## Load and enable with plain HTML

```html
<script src="dist/ofi.browser.js"></script>
<script>objectFitImages();</script>
```

## Load with with browserify

```sh
npm install --save object-fit-images
```

```js
var objectFitImages = require('object-fit-images');
objectFitImages();
```

## API

### `objectFitImages([images, [opts]])`

parameter                         | description
:---                              | :---
**`images`**                      | Type: `string` (as a selector) or `array`-like *optional* <br> The images to apply the fix on. If it's not supplied (or `false`), OFI will enter the automatic mode (which means that new images in the DOM will automatically be fixed).
**`opts`**                        | Type: `object` *optional* <br> Set to `{watchMQ: true}` if you expect `object-fit` to vary in a media query or if you use `scale-down` <br> Set to `{preferSrcsetOverPosition: true}` if you want to change [the behavior in Safari](detailed-support-tables.md#object-fit-images--srcset--object-position--safari-84-)

## Known issues

Take a look at [possible issues and limitations](detailed-support-tables.md#notes-about-specific-combinations) of object-fit-images.

## License

MIT © [Federico Brigante](http://twitter.com/bfred_it)
