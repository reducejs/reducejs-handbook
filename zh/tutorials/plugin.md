## 插件类型
Reduce中的插件根据发生作用的阶段可分为以下两类：
* 打包插件：控制包生成的插件。
* 后处理插件：包生成后的处理插件。

这里的“包”指的是[vinyl]对象。也就是说，第一类插件主要影响[vinyl]对象的生成，第二类插件则是对[vinyl]对象做进一步处理。

Reduce使用[browserify]处理JS，[depsify]处理CSS，两者都通过创建基于[Node.js stream]的管道来实现。
打包插件便可用来修改管道的处理逻辑。包括[browserify]和[depsify]的所有插件，分别作用于JS和CSS。

还有些插件两者都适用，如[watchify]、[common-bundle]。

<p class="note">
Reduce默认使用了一些插件，不能再指定这些插件。
</p>
默认使用的打包插件：
* [watchify2]。提供watch功能。
* [common-bundle]。提供打包策略的订制功能。
* [reduce-css-postcss]。支持[PostCSS]预处理。

后处理插件实际就是处理[vinyl]流的插件，包括所有[gulp]插件。
<p class="note">
实际使用时需要确定插件是处理JS还是CSS。
</p>
<p class="note">
由于CSS中可能存在url，部分插件不能使用。
</p>
譬如[gulp-rename]，[vinyl-fs#dest]。（但可以对JS使用）

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
[gulp]: https://github.com/gulpjs/gulp
[vinyl]: https://github.com/gulpjs/vinyl
[vinyl-fs#dest]: https://github.com/gulpjs/vinyl-fs#destfolder-options
[gulp-rename]: https://github.com/hparra/gulp-rename

