## 环境要求
Node.js版本4及以上。

## Node.js
```bash
npm install --save-dev reduce-web-component

```

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

```bash
npm install reduce-web-component -g
reduce -h

```

