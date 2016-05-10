## 目录
- [CommonJS](#commonjs)
- [模块打包](#模块打包)
- [CSS规范](#css规范)
- [组件](#组件)

## CommonJS
JS模块化需要解决两个问题：
* 使用何种模块规范对代码进行组织。
* 如何将分散的模块有机地结合起来，使浏览器接受所使用的模块规范。

在浏览器执行JS时，需要解决命名冲突的问题。这通常是通过遵循一定的规范来实现的，譬如[CommonJS]、[AMD]。
这两种规范的实现代表分别有[Node.js]、[requirejs]。
考虑到服务器端大量运行[Node.js]，为了使浏览器中运行的代码规范与其保持一致，[reduce-js]默认支持的是[CommonJS]规范。

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

可见，`require`是依赖声明，`module`是接口输出。
更多内容请参考[Node.js#modules]。

## 模块打包
在服务器端，所有的模块都已经在本机上，直接可读取，因此`require`通过文件系统读取内容直接解析。
但在浏览器端，除了实现`require`与`module`外，还需要考虑模块下载的问题。

假定某个页面的初始化脚本如下：
```js
// 入口模块

// 功能一
require('./ga')

// 功能二
require('./dialog')

// 更多入口

```

从入口模块出发，分析依赖，可得一棵依赖关系树。
只要将整棵树中各节点对应的模块均下载下来，那么入口模块便可以被顺利执行。
所以，上面的初始化脚本便确定了该页面所需要的模块集合，将其封装成一个包下载即可。
这个打包问题便是[webpack]与[browserify]解决的基本问题。

## CSS规范
对于CSS而言，模块化还需要解决的主要问题有：
- 如何声明依赖
- 如何避免命名冲突

> 命名冲突的解决方案请参见[CSS命名空间](../recipes/css-namespace.md)。

由于CSS中可以使用自定义的At-rule，因此[Reduce]支持在CSS通过`@external "deps.css";`来声明依赖。

```css
/* a.css */

/* 声明a.css依赖b.css */
@external "./b.css";

a {}

```

“依赖”表示两个模块在最终打包生成的CSS中的先后顺序。
在上面的例子中，[Reduce]会确保最终b.css的内容会出现在a.css前。

<div class="note">
CSS中的`@import`可以引入另一个CSS的内容，即直接在当前位置插入目标文件的内容。
这与`@external`的含义是有区别的。
</div>

通常，在一些预编译工具中，`@import`可以引入一些辅助模块，如变量定义、mixins等。
这些模块本身不产生任何具体的CSS内容，但预处理生成CSS时需要它们。

下面是一个使用`@import`的例子。
```scss
/* a.css */
@import "./vars.css";
a {
  color: $red;
}

```

```scss
/* vars.css */
$red: #ff0000;

```

```scss
/* 打包结果 */
a {
  color: #ff0000;
}

```

如果上面的例子使用`@external`替代`@import`，打包结果将是：
```scss
/* 使用@external替代@import的打包结果 */

/* vars.css编译后内容为空 */

a {
  /* 没有引入变量的定义 */
  color: $red;
}

```

总结一下两者的区别：
- `@external`在预处理后起作用，而`@import`在预处理过程中起作用。
因此，任何辅助预处理的模块都**必须**使用`@import`来引入。
- `@external`声明预处理后CSS模块的依赖关系，主要影响打包结果。
只要有可能，对于具体的CSS模块，都使用`@external`来连接。

在下面的例子中，需要以a.css和b.css为入口，分别生成两个CSS文件，同时生成一个公共的common.css文件，供跨页面共享。

**输入**

```scss
/* a.css */

/* 在文件中的顺序不重要 */
@external "reset";

/* 必须在使用之前 */
@import "./vars";

a {
  color: $red;
}

```

```scss
/* b.css */

/* 在文件中的顺序不重要 */
@external "reset";

/* 必须在使用之前 */
@import "./vars";

b {
  color: $green;
}

```

```scss
/* reset.css */
html {}

```

**输出**
```scss
/* a.css打包结果 */
a {
  color: #ff0000;
}

```

```scss
/* b.css打包结果 */
b {
  color: #00ff00;
}

```

```css
/* common.css */
html {}

```

如果这个例子中的`@external`都用`@import`替换，则无法提取公共包，即common.css内容为空。

<p class="note">
@external与@import的路径解析规则与Node.js中的require保持一致。
具体请参考<a href="https://nodejs.org/api/modules.html#modules_all_together" target="_blank">官方文档</a>。
</p>

<p class="note">
与Node.js一样，Reduce在解析依赖时，也会参考package.json的信息，JS是main字段，CSS是style字段。
</p>

## 组件
在组件中，常常同时包含JS、CSS。

假设有组件calendar（包含calendar.js和calendar.css），和schedule（包含schedule.js和schedule.css）。
现在要在组件schedule中使用calendar，按一般的做法，
需要在schedule.js中去`require('calendar')`，同时在schedule.css中`@external "calendar";`。

但在[Reduce]中，可以只写`require('calendar')`，打包时便会自动补上schedule.css对calendar.css的依赖。
如此，就只需要在一处声明，便确定了组件之间的依赖关系。

为了能做到这点，需要calendar组件通过某种方式声明自己的JS与CSS的绑定关系。
譬如，在package.json中指定style字段，便表示该组件的入口JS模块应当与指定的样式文件同时使用。

<p class="note">
在0.6中，是通过在配置中指定`cssFile = getStyle(jsFile)`函数来做到的。
后续版本将支持在package.json中指定组件的JS与CSS之间的绑定关系。
</p>

[node-modules]: https://nodejs.org/api/modules.html#modules_all_together
[AMD]: https://github.com/amdjs/amdjs-api/wiki/AMD
[browserify]: https://github.com/substack/node-browserify
[code splitting]: http://webpack.github.io/docs/code-splitting.html
[CommonJS]: http://wiki.commonjs.org/wiki/Modules
[gulp.spritesmith-multi]: https://github.com/reducejs/gulp.spritesmith-multi
[Node.js]: https://nodejs.org/
[Node.js#modules]: https://nodejs.org/api/modules.html
[reduce-css]: https://github.com/reducejs/reduce-css
[reduce-js]: https://github.com/reducejs/reduce-js
[Reduce]: https://github.com/reducejs/reduce-web-component
[requirejs]: https://github.com/requirejs/requirejs
[webpack]: https://github.com/webpack/webpack
