## 什么是组件
- 依赖声明

## CommonJS规范
JS模块化需要解决两个问题：
* 使用何种模块规范对代码进行组织。这是“分”。
* 如何将分散的模块有机地结合起来，使浏览器接受所使用的模块规范。这是“合”。

在浏览器执行JS时，需要解决命名冲突的问题。这通常是通过遵循一定的规范来实现的，譬如[CommonJS]、[AMD]。
这两种规范的实现代表分别有[Node.js]、[requirejs]。
考虑到服务器端大量运行[Node.js]，为了使浏览器中运行的代码规范与其保持一致，所以[reduce-js]默认支持的是[CommonJS]规范。

在[Node.js]中，通过`require`函数和`module`对象来实现模块划分、组织。

```js
// salute.js

// module.exports是暴露给外面的接口
module.exports = 'Hello, '

```

```js
// world.js

// require的返回结果是对应的module.exports
var salute = require('./salute.js')
module.exports = salute + 'world!'

```

可见，`require`是依赖表示，`module`是接口输出。更多内容请参考[Node.js#modules]。

除了照搬[Node.js]对`require`和`module`的实现外，浏览器端的模块加载机制还需要解决模块下载的问题。

在服务器端，所有的模块都已经在本机上直接可读取，因此`require`时通过文件系统读取内容再解析即可。
在浏览器端，出于性能的考虑，通常不会将整站所有的模块全都下载。
此时，可以定义入口模块的概念。
```js
// 入口模块

// 功能一
require('./ga')

// 功能二
require('./dialog')

// ...

```

每个入口模块确定了一棵模块的依赖树。只要依赖树中的模块都下载了，这个入口模块便可以正常运行。
因此，可以通过给每个页面定义不同的入口模块，进而确定各自需要的代码，将其打包到一起给浏览器，页面便可以正常运行，且可以只加载确实需要的代码。
这便是[webpack]和[browserify]解决的基本问题。

对于单页应用而言，就无法通过入口模块来拆分代码了，需要按需加载。这要求对规范做一定的扩展，如[webpack]的[code splitting]。


## CSS规范
- 依赖声明

[AMD]: https://github.com/amdjs/amdjs-api/wiki/AMD
[browserify]: https://github.com/substack/node-browserify
[code splitting]: http://webpack.github.io/docs/code-splitting.html
[CommonJS]: http://www.commonjs.org/
[gulp.spritesmith-multi]: https://github.com/reducejs/gulp.spritesmith-multi
[Node.js]: https://nodejs.org/
[Node.js#modules]: https://nodejs.org/api/modules.html
[reduce-css]: https://github.com/reducejs/reduce-css
[reduce-js]: https://github.com/reducejs/reduce-js
[reduce-web-component]: https://github.com/reducejs/reduce-web-component
[requirejs]: https://github.com/requirejs/requirejs
[webpack]: https://github.com/webpack/webpack
