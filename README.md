# 在nuxt中使用cube-ui
直接通过普通编译使用 [cube-ui](https://didi.github.io/cube-ui/#/zh-CN/docs/quick-start) 是没有问题的。但如果你想自定义主题，那必须通过后编译的方式。

按照NUXT的文档，我们首先需要在 plugins 下新建 cube-ui.js，并在其中按需引入要使用的组件（全部引入见 [cube-ui](https://didi.github.io/cube-ui/#/zh-CN/docs/quick-start) 文档）

这里我们引入 Botton 组件，并在 index.vue 中使用
```javascript
import Vue from 'vue'
import {
  /* eslint-disable no-unused-vars */
  Style,
  Button
} from 'cube-ui'

Vue.use(Button)
```
此时默认会走 module 字段，也就是 cube-ui 的 src 目录。此时源码未经编译会报错。
让我们加入后编译的配置


## Build Setup

``` bash
# install dependencies
$ npm run install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm run start

# generate static project
$ npm run generate
```

For detailed explanation on how things work, checkout [Nuxt.js docs](https://nuxtjs.org).
