## 功能
Reduce使用[browserify]处理JS，[depsify]处理CSS，两者都通过创建基于[Node.js stream]的管道来实现。
它们的插件便可用来修改管道的处理逻辑。

[browserify]与[depsify]的处理管道有同样的结构，具体可参考[browserify-handbook][pipeline]。

因此，两者可以公用一些与JS或CSS编译无关的插件，如[watchify], [common-bundle]等。

事实上，Reduce默认使用了以下插件：
* [watchify2]。在[watchify]的基础上，支持watch新增的入口。
* [common-bundle]。灵活的公共包定制策略。
* [reduce-css-postcss]。只用于[depsify]，支持[PostCSS]。

## 如何写插件
所有插件都是有如下声明的函数：
```js
function myPlugin(b, opts) {
  // hack b.pipeline
}

```

`b`是一个[browserify]或[depsify]实例，`opts`是使用时传入的配置。
`b.pipeline`即是处理管道，它是一个[labeled-stream-splicer]实例，可以通过与数组类似的接口（如`push`、`splice`等方法）进行操作。

具体实例参见[这里](https://github.com/substack/browserify-handbook#compiler-pipeline)。

[browserify]: https://github.com/substack/node-browserify
[common-bundle]: https://github.com/reducejs/common-bundle
[depsify]: https://github.com/reducejs/depsify
[labeled-stream-splicer]: https://github.com/substack/labeled-stream-splicer
[Node.js stream]: https://nodejs.org/api/stream.html
[pipeline]: https://github.com/substack/browserify-handbook#compiler-pipeline
[PostCSS]: https://github.com/postcss/postcss
[reduce-css-postcss]: https://github.com/reducejs/reduce-css-postcss
[watchify]: https://github.com/substack/watchify
[watchify2]: https://github.com/reducejs/watchify2

