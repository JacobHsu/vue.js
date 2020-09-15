# NPM

## gh-pages

[![NPM](https://nodei.co/npm/gh-pages.png?downloads=true&stars=true)](https://nodei.co/npm/gh-pages/)

`$ yarn add gh-pages -D`

package.json

```js
"homepage": "https://jacobhsu.github.io/<project-name>/",
"scripts": {
  "deploy": "yarn build && gh-pages -d dist"
}
```

vue.config.js

```js
module.exports = {
    publicPath: process.env.NODE_ENV === 'production' ? '/<project-name>' : '/',
}
```

```js
module.exports = {
  publicPath: process.env.NODE_ENV === "production" ? "/<project-name>" : "/"
};
```

::: warning 
`'<project-name>'` error  watch `/`  
https://jacobhsu.github.io/project-name/project-name/css/app.{id}.css
:::

### Vue Cli 2

config\index.js  (vue cli 3 vue.config.js)

```js
  build: {
    ..

    // Paths
    assetsRoot: path.resolve(__dirname, '../dist'),
    assetsSubDirectory: 'static',
    assetsPublicPath: '/vue-webpack-vuex/',
```


::: danger 
[【狀況題】怎麼有時候推不上去](https://gitbook.tw/chapters/github/fail-to-push.html) 先拉再推   
`$ git push origin :gh-pages` 刪除遠端分支  
`$ git pull`  
`$ git push origin gh-pages`  

`$ git branch -a` 查看所有分支
`$ git branch gh-pages` 新建遠程分支
:::

## node-sass

[![NPM](https://nodei.co/npm/node-sass.png?downloads=true&stars=true)](https://nodei.co/npm/node-sass/)

::: danger 
[解决 Cannot find module 'node-sass'](https://segmentfault.com/a/1190000021127614)  
[Error: Node Sass does not yet support your current environment: Windows 64-bit with false](https://stackoverflow.com/questions/37415134/error-node-sass-does-not-yet-support-your-current-environment-windows-64-bit-w)
:::

```s
npm install node-sass -g
npm uninstall node-sass
npm install node-sass
```

```s
npm install -D sass-loader node-sass
```

> TypeError: loaderContext.getResolve is not a function

sass-loader 版本过高 

```s
npm install -D sass-loader@6.0.6
```

> npm ERR! Failed at the node-sass@4.7.2 postinstall script.

`npm uninstall node-sass -D`  
`npm install node-sass -D`  

> Module build failed: Error: Cannot find module 'node-sass'

`npm install node-sass@next -D`

package.json

```js
"devDependencies": {
  "node-sass": "^4.14.1",
}
```

## NPMs

[![NPM](https://nodei.co/npm/bootstrap-vue.png?downloads=true&stars=true)](https://nodei.co/npm/bootstrap-vue/)

[![NPM](https://nodei.co/npm/json-server.png?downloads=true&stars=true)](https://nodei.co/npm/json-server/)

[![NPM](https://nodei.co/npm/vue-multiselect.png?downloads=true&stars=true)](https://nodei.co/npm/vue-multiselect/)

[![NPM](https://nodei.co/npm/vuejs-datepicker.png?downloads=true&stars=true)](https://nodei.co/npm/vuejs-datepicker/)

## Data

[![NPM](https://nodei.co/npm/faker.png?downloads=true&stars=true)](https://nodei.co/npm/faker/)  
[![NPM](https://nodei.co/npm/vue-localstorage.png?downloads=true&stars=true)](https://nodei.co/npm/vue-localstorage/)

## Request

[![NPM](https://nodei.co/npm/axios.png?downloads=true&stars=true)](https://nodei.co/npm/axios/)

[![NPM](https://nodei.co/npm/humps.png?downloads=true&stars=true)](https://nodei.co/npm/humps/)

[![NPM](https://nodei.co/npm/json-server.png?downloads=true&stars=true)](https://nodei.co/npm/json-server/)

## i18n

[![NPM](https://nodei.co/npm/vue-i18n.png?downloads=true&stars=true)](https://nodei.co/npm/vue-i18n/)

[Vue I18n](https://kazupon.github.io/vue-i18n/) Vue I18n is internationalization plugin for Vue.js

## chartjs

[![NPM](https://nodei.co/npm/vue-chartjs.png?downloads=true&stars=true)](https://nodei.co/npm/vue-chartjs/)

[vue-chartjs](https://vue-chartjs.org/)  Easy and beautiful charts with Chart.js and Vue.js

## component

[![NPM](https://nodei.co/npm/vue-nav-tabs.png?downloads=true&stars=true)](https://nodei.co/npm/vue-nav-tabs/)  
📖[Documentation](https://cristijora.github.io/vue-tabs/#/)  
`npm install --save vue-nav-tabs`  
`npm install --save babel-helper-vue-jsx-merge-props`  

[![NPM](https://nodei.co/npm/vue-star-rating.png?downloads=true&stars=true)](https://nodei.co/npm/vue-star-rating/)  

## library

[![NPM](https://nodei.co/npm/lodash.png?downloads=true&stars=true)](https://nodei.co/npm/lodash/)

## Cli Tool

[![NPM](https://nodei.co/npm/npm-run-all.png?downloads=true&stars=true)](https://nodei.co/npm/npm-run-all/)

ex: regreceive / [exchange](https://github.com/regreceive/exchange)
[Error: Node Sass does not yet support your current environment: Windows 64-bit with false](https://stackoverflow.com/questions/37415134/error-node-sass-does-not-yet-support-your-current-environment-windows-64-bit-w)

## unit-testing

[![NPM](https://nodei.co/npm/flush-promises.png?downloads=true&stars=true)](https://nodei.co/npm/flush-promises/)

## cookie

[![NPM](https://nodei.co/npm/js-cookie.png?downloads=true&stars=true)](https://nodei.co/npm/js-cookie/)  

Try `npm install @types/js-cookie` if it exists or add a new declaration (.d.ts) file containing `declare module 'js-cookie';`

`yarn add @types/js-cookie`