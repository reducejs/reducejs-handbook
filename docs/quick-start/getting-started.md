## 安装
要求Node.js版本4及以上。

```js
npm install --save-dev reduce-web-component

```

> 命令行工具出来后，可选择全局安装。

## Node.js
```js
var reduce = require('reduce-web-component')

// 传入配置，或配置文件路径，开始打包
var donePromise = reduce.bundle(config)

// 传入配置，或配置文件路径，开始打包
// 并watch文件的增加、修改、删除
var watchDonePromise = reduce.watch(config)

```

更多信息：

* [配置说明](/api/configure/)
* [接口说明](/api/nodejs/)

## 命令行
> 命令行配置

