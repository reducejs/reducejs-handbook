## 功能说明
实现特定web功能的单元可称之为组件，通常包含JavaScript、CSS、图片等静态资源。
[reducejs]以这样的组件为输入，解析依赖，打包生成最终浏览器可使用的静态资源形式。

## 为什么要使用reducejs
相比[webpack], [browserify]而言：

- 支持组件级别的打包。通过定义组件的依赖声明形式，可自动将组件所需要的各种静态资源整合，生成最终浏览器可使用的形式。
- CSS打包方式对开发者更友好，不需要在JS中引入CSS。

## 支持哪些功能
**CSS预处理**

>默认使用[PostCSS]进行**CSS预处理**。
更多说明请参见[组件说明][component]

**插件机制与Transform机制**

>底层使用[browserify]进行JS的打包，因此集成了[browserify]的功能，包括它的插件机制和Transform机制。
更多说明请参见[插件][plugin]与[Transform][transform]。

**灵活定制打包策略**

>底层使用[common-bundle]对JS与CSS进行打包，可方便地进行包的拆分与公共包的提取。
[common-bundle]即将支持[webpack]中实现的[Code Splitting]功能。
更多说明请参见[定制打包策略][bundle]。

**模块热替换**

>基于[browserify-hmr]实现了[模块热替换][hmr]的功能，其接口与[webpack]保持一致。

**watch新增和删除的入口**

>对[watchify]做了一些改动，使其在watch时可检测出新增的入口，具体请见[watchify2]。

[component]: ../tutorials/component
[plugin]: ../tutorials/plugin
[transform]: ../tutorials/transform
[bundle]: ../tutorials/how-to-bundle

[browserify]: https://github.com/substack/node-browserify
[webpack]: https://github.com/webpack/webpack
[reducejs]: https://github.com/reducejs/reduce-web-component
[common-bundle]: https://github.com/reducejs/common-bundle
[PostCSS]: https://github.com/postcss/postcss
[Code Splitting]: http://webpack.github.io/docs/code-splitting.html
[browserify-hmr]: https://github.com/AgentME/browserify-hmr
[hmr]: http://webpack.github.io/docs/hot-module-replacement-with-webpack.html
[watchify]: https://github.com/substack/watchify
[watchify2]: https://github.com/reducejs/watchify2
