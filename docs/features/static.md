# Static Assets
> Since v0.4
Farm supports three resource loading methods: `url`, `inline`, `raw` 。

## url
Import a image：
```jsx
import rocketUrl from './assets/rocket.svg'; // return the url of this image

export function Main() {
  return <img src={rocketUrl} /> // using the url
}
```
Default to use url method when import a image. When using url methods to import a image, the image will be emitted to the output dir directly, and the image module itself will be compiled to a js module like:

```js
export default '/rocket.<content hash>.svg'
```
using `compilation.output.assetFilename` to config your asset name。

## inline
Using query `?inline` to tell Farm that you want to inline your assets，then the assets will be transformed to base64，for example：

```js
// importer
import logo from './assets/logo.png?inline'; // logo is a base 64 str

// the image module will be compiled to:
export default 'data:image/png,base64,xxxxx==';
```

## raw
Using query `?raw` to tell Farm that you want to read the raw string of the assets, for example
```js
// import 
import logo from './assets/license.txt?raw'; // return the content string of the assets

// the txt file will be compiled to:
export default 'MIT xxxx';
```

## Configuring

```js
export default {
  compilation: {
    output: {
      assetFilename: 'assets/[resourceName].[hash].[ext]', // [] is a placeholder, Farm currently only these three kind of placeholders
    },
    assets: {
      include: ['txt'] // extra static asset extension
    }
  }
}
```