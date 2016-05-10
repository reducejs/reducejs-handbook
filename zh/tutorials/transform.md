## 功能
Reduce使用[browserify]处理JS，[depsify]处理CSS，两者都通过创建基于[Node.js stream]的管道来实现。
[browserify]与[depsify]的处理管道有同样的结构，具体可参考[browserify-handbook][pipeline]。

其中`deps`便是依赖解析的阶段，也是Transform起作用的阶段：
每当读取一个模块内容后，先经过Transform的变换，再进行语法解析。
因此，预处理主要是通过Transform来实现。譬如[babelify]。

## 如何写Transform
所有Transform都是有如下声明的函数：
```js
function myTransform(file, opts) {
  return someTransform
}

```

`file`是当前处理的文件路径，`opts`是使用Transform时传入的配置。
每个Transform必须返回一个[Node.js stream]的Transform实例（或Duplex实例）。可以考虑使用[through2]来快速创建这样一个对象。

具体实例可参见[这里](https://github.com/substack/browserify-handbook#transforms)。

## 关于Stream的参考文献
* [官方文档][Node.js stream]
* [Node.js Stream - 基础篇](http://fe.meituan.com/stream-basics.html)
* [Node.js Stream - 进阶篇](http://fe.meituan.com/stream-internals.html)
* [Node.js Stream - 实战篇](http://fe.meituan.com/stream-in-action.html)

[babelify]: https://github.com/babel/babelify
[browserify]: https://github.com/substack/node-browserify
[depsify]: https://github.com/reducejs/depsify
[Node.js stream]: https://nodejs.org/api/stream.html
[pipeline]: https://github.com/substack/browserify-handbook#compiler-pipeline
[through2]: https://github.com/rvagg/through2

