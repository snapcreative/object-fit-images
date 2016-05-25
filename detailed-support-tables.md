# Detailed support of object-fit-images

> OFI in short

## Notes about specific combinations

### *object-fit-images* + [*picturefill*](https://github.com/scottjehl/picturefill)

💛 If you need `srcset`/`picture`, load/execute *picturefill* **after** OFI, or else images with `srcset` will be blank (happens in Edge 12+)

### *object-fit-images* + `srcset` + Edge 12

💔 [Edge 12 doesn't support `currentSrc`](https://blogs.windows.com/msedgedev/2015/06/08/introducing-srcset-responsive-images-in-microsoft-edge/) and therefore OFI only uses `src`

### *object-fit-images* + `srcset` + `object-position` + Safari 8.4-

💛 Safari (iOS 8.4- and OSX 8-) doesn't support `currentSrc` so to add support for `object-position` OFI will discard `srcset` and use `src`.

To discard `object-position` instead, use the option `preferSrcsetOverPosition`, example:

```js
objectFitImages('img', {
	preferSrcsetOverPosition: true
});
```

### *object-fit-images* + `picture`

💛 Supported only in IEdge 9-12 and Android 4.4.4 with [*picturefill*](https://github.com/scottjehl/picturefill).

Feature tracked here: https://github.com/bfred-it/object-fit-images/issues/3

## Can I Use

* [`object-fit`](http://caniuse.com/#feat=object-fit)
* [`srcset`](http://caniuse.com/#feat=srcset)
* [`picture`](http://caniuse.com/#feat=picture)
