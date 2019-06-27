# 在nuxt中使用cube-ui
直接通过普通编译使用 [cube-ui](https://didi.github.io/cube-ui/#/zh-CN/docs/quick-start) 是没有问题的。但如果你想自定义主题，那必须通过后编译的方式。

首先安装好 PostCompilePlugin 和 TransformModulesPlugin 两个包

然后按照NUXT的文档，我们需要在 plugins 下新建 cube-ui.js，并在其中按需引入要使用的组件（全部引入见 [cube-ui](https://didi.github.io/cube-ui/#/zh-CN/docs/quick-start) 文档）

这里我们引入 Button 组件
```javascript
// plugins/cube-ui.js

import Vue from 'vue'
import {
  /* eslint-disable no-unused-vars */
  Style,
  Button
} from 'cube-ui'

Vue.use(Button)
```

现在让我们加入 后编译 和 按需引入 的配置
```javascript
// nuxt.config.js
build: {
  loaders: {
    stylus: {
      'resolve url': true,
      import: [path.resolve(__dirname, './assets/theme')]
    }
  },
  plugins: [
    new PostCompilePlugin(),
    new TransformModulesPlugin()
  ]
}
```
nuxt 为我们提供了修改 webpack 配置的入口。为 stylus-loader 添加 options，并添加我们后编译 和 按需引入的插件。theme.styl 就是自定义主题的文件。

此时，server 阶段不编译node_modules，也就不编译cube-ui，有两种解决方式：

解决一：（推荐）

nuxt 为我们提供了 transpile 使用Babel转换特定依赖项

解决二：（不推荐）

研究 @nuxt/webpack/dist/webpack.js 发现，node_modules 被加入了 externals 中，所以不走编译。从代码中发现，可通过变量 `standalone: true` 进行配置，但文档中并没有相关的说明，并且由于会编译整个 node_modules，所以会慢，不推荐。

```javascript
// nuxt.config.js
build: {
  // standalone: true,
  transpile: ['cube-ui'],
  loaders: {
    stylus: {
      'resolve url': true,
      import: [path.resolve(__dirname, './assets/theme')]
    }
  },
  plugins: [
    new PostCompilePlugin(),
    new TransformModulesPlugin()
  ]
}
```

现在项目就可以正常跑起来了，并且我们在 theme.styl 中将按钮的背景色，从蓝色修改为橙色。

备注：用 NUXT 脚手架初始化的的项目，nuxt 被放在了 `dependencies` 下，需要手动移动到 `devDependencies` 下，否则 windows 下有坑。本示例项目已经做了修改。

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
