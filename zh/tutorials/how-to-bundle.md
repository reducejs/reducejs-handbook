Reduce使用[common-bundle]进行模块的封装和打包，支持灵活的公共包提取策略。

```js
var reduce = require('reduce-web-component')
// 在config中指定打包策略
reduce.bundle(config).then(() => {
  // 打包结束
})

```

## 单包
可以将所有JS或CSS打成一个包。

假如页面A有入口
* js: `/path/to/src/page/A/index.js`
* css: `/path/to/src/page/A/index.css`

则可以使用如下配置将所有入口及其依赖打成一包：
```js
var config = {
  dest: 'build',
  reduce: {
    basedir: '/path/to/src',
  },
  js: { 
    entries: 'page/**/index.js',
    // 所有entries及其依赖出现在一个包中
    // 路径为/path/to/build/bundle.js
    bundleOptions: 'bundle.js',
  },
  css: { 
    entries: 'page/**/index.css',
    // 所有entries及其依赖出现在一个包中
    // 路径为/path/to/build/bundle.css
    bundleOptions: 'bundle.css',
  },
}

```

## 所有页面共享一个公共包
假如页面A有入口
* js: `/path/to/src/page/A/index.js`
* css: `/path/to/src/page/A/index.css`

则可以使用如下配置为每个入口创建一个包，并额外创建一个公共包以跨页面共享：

```js
var config = {
  dest: 'build',
  reduce: {
    basedir: '/path/to/src',
  },
  js: { 
    entries: 'page/**/index.js',
    bundleOptions: {
      // 每匹配到一个模块，便创建一个新的包
      // 其路径为/path/to/build/page/**/index.js
      groups: 'page/**/index.js',
      // 公共包路径为/path/to/build/common.js
      common: 'common.js',
    },
  },
  css: { 
    entries: 'page/**/index.css',
    bundleOptions: {
      // 每匹配到一个模块，便创建一个新的包
      // 其路径为/path/to/build/page/**/index.css
      groups: 'page/**/index.css',
      // 公共包路径为/path/to/build/common.css
      common: 'common.css',
    },
  },
}

```

如果去掉上面配置中的`common`选项，则不产生公共包。


[common-bundle]: https://github.com/reducejs/common-bundle

