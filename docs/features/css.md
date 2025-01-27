# Css
Farm support Css out of box, just import the css file:

```tsx
import './index.css';
```

Then farm will auto enable HMR for css module, and generating bundled resources for css.

## Css Modules
Farm support css modules out of box, the modules end with `.module.css|less|scss|sass` will be treated as css modules by default.

```tsx title="comp.tsx"
// ...
import styles from './index.module.css'

export function Comp() {
  return <div className={styles.main}>Main</div>
}
```
```css title="index.module.css"
.main {
  color: green;
}
```
You can configuring css modules by [`css.modules`](/docs/config/farm-config#cssmodules). for example you can set `css.modules.paths` to `['.css|sass|less|scss']` then all css files will be treated as css modules.

## Css Pre-Processor
Farm provide official sass, less, postcss plugins to support css pre-processor.

### Sass
Farm sass plugin is a Rust Plugin and use `sass-embeded`(we may migrate to [grass](https://github.com/connorskees/grass) in the feature).

Steps to compile `sass/scss` modules in Farm.
1. Install dependencies
```sh
# npm or yarn or pnpm, choose your favorite package manager
npm install @farmfe/plugin-sass
```

2. Configure the plugin
```ts
import type { UserConfig } from '@farmfe/core';

export default <UserConfig> {
  // ...
  plugins: ['@farmfe/plugin-sass'] // to use a rust plugin, just configure its package name as a string
  // if you want to specify options for plugin-sass, use
  // plugins: [
  //   ['@farmfe/plugin-sass', { sourceMap: false }]
  // ]
};
```

3. Import sass module
```ts
import './index.scss';
```

To use sass with css modules, change the file name from `index.scss` to `index.module.scss`, see [css modules](/docs/config/farm-config#cssmodules).

`@farmfe/plugin-sass` supports a lot of options, use the array syntax of `plugins` to specify options for plugin sass:

```ts
import type { UserConfig } from '@farmfe/core';

export default <UserConfig> {
  // if you want to specify options for plugin-sass, use
  plugins: [
    [
      '@farmfe/plugin-sass',
      // all supported options as below
      {
        sourceMap: true // bool
        sourceMapIncludeSources: true, // bool
        alertAscii: true, // bool
        alertColor: true, // bool
        charset: true, // bool
        quietDeps: true, // bool
        verbose: false, // bool
        style: 'expanded' | 'compressed' // output code style
      }
    ]
  ]
};
```


### Less
Farm less plugin is a Js Plugin. Steps to compile `less` modules in Farm.

1. Install dependencies
```sh
# npm or yarn or pnpm, choose your favorite package manager
npm install @farmfe/js-plugin-less
```

2. Configure the plugin
```ts
import type { UserConfig } from '@farmfe/core';
import less from '@farmfe/js-plugin-less';

export default <UserConfig> {
  // ...
  plugins: [less()] // pass argument to the less function like `less({ /* your options */ })` to specify less options
};
```

3. Import sass module
```ts
import './index.less';
```

To use sass with css modules, change the file name from `index.less` to `index.module.less`, see [css modules](/docs/config/farm-config#cssmodules)

### Postcss
The Farm postcss plugin is a JS plugin. The steps to introduce postcss in Farm are as follows:

1. Install dependencies
```sh
# npm or yarn or pnpm, choose your favorite package manager
npm install @farmfe/js-plugin-postcss
```

2. Configure the plugin
```ts
import type { UserConfig } from '@farmfe/core';
import postcss from '@farmfe/js-plugin-postcss';

export default <UserConfig> {
   //...
   plugins: [postcss()] // pass argument to the less function like `less({ /* your options */ })` to specify less options
};
```

3. Configure `postcss.config.js` and import the required postcss plugins

```js title=postcss.config.js
module.exports = {
   plugins: [
     require('postcss-pxtorem')({
       rootValue: 16,
       propList: ['*'],
     }),
     require('tailwindcss'),
   ]
}
```