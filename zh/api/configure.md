## 各配置项含义
配置对象`configure`中各项的含义：
- [`js`]
  - [`js.entries`]
  - [`js.bundleOptions`]
  - [`js.reduce`]
  - [`js.plugin`]
  - [`js.dest`]
  - [`js.on`]
- [`css`]
  - [`css.entries`]
  - [`css.bundleOptions`]
  - [`css.reduce`]
  - [`css.plugin`]
  - [`css.dest`]
  - [`css.on`]
  - [`css.postcss`]
- [`getStyle`]
- [`watch`]
- [`reduce`]
- [`dest`]
- [`on`]

<p class="note">
0.7版本将区分不同环境下的配置。
</p>

## js
### js.entries
*Optional*

用来定位入口模块。
与[vinyl-fs#src]一样，支持`**`与`*`等通配符。

Type: `String`, `Array`

一些实例：

* `'page/**/index.js'`
* `'lib/*.js'`
* `['lib/*.js', '!lib/test.js']`。`!`表示排除。

如果不是绝对路径，则必须是相对于`js.reduce.basedir`的。

### js.bundleOptions
*Optional*

指定打包策略。

#### 单包模式

Type: `String`

所有JS打成一个包，指定值为这个包的路径（相对于[`js.dest`]）。

实例：`bundle.js`。

#### 多包模式

Type: `Object`

可以指定`groups`和`common`字段。

**groups**

Type: `String`，`Array`。

与[`js.entries`]一样的形式，匹配到的文件将独立打成包。
相匹配部分的值，为相对于[`js.dest`]的路径。

**common**

*Optional*

Type: `String`

指定公共包路径。

通过`groups`确定多个包后，从中提取出所有包公用的模块，生成一个公共包。

**实例**
```js
{
  js: {
    reduce: { basedir: '/path/to/src' },
    bundleOptions: {
      groups: 'page/**/index.js',
      common: 'common.js',
    },
    dest: '/path/to/build',
  },
}

```

如果有模块`/path/to/src/page/hi/index.js`，则生成包`/path/to/build/page/hi/index.js`。
同时，从所有匹配生成的包中提取出公共模块，生成公共包`/path/to/build/common.js`。

### js.reduce
直接传递给[browserify]的配置，请参考[这里][browserify-opts]。

**注意**：不能再指定`entries`。

下面是几个常见的配置项。

**basedir**

Type: `String`

模块相对路径的根目录。

**plugin**

指定[browserify]插件。

**transform**

指定[browserify]使用的Transform。

### js.plugin
指定处理JS包的插件。
由于JS包即[vinyl]文件，所以实际上[gulp]插件都可用在这里。

**注意**：这里指定的是在JS包生成后进行处理的插件，与[browserify]的插件不同。具体请参考[插件说明](../tutorials/plugin.md)和相关[实例](https://github.com/reducejs/reduce-web-component/tree/master/example/plugin)。

### js.dest
指定生成JS包的根目录。

直接传给[vinyl-fs#dest]。
如果需要指定第二个参数，可以将`js.dest`指定为数组。

### js.on
监听JS打包相关的事件。

```js
{
  js: { 
    on: { 
      'common.map': (bundleMap, inputMap) => {},
      'reduce.end': (bytes, duration) => {},
      log: msg => {},
      error: err => {},
    }
  }
}

```

- `common.map`。由[`common-bundle`]触发，提供模块和生成包的对应关系。
- `reduce.end`。每次生成包写入磁盘后自动触发。


## css
### css.entries
与[`js.entries`]一样。

### css.bundleOptions
与[`js.bundleOptions`]一样。

### css.reduce
直接传递给[depsify]的配置。

**注意**：不能再指定`entries`。

下面是几个常见的配置项。

**basedir**

Type: `String`

模块相对路径的根目录。

**plugin**

指定[depsify]插件。

**transform**

指定[depsify]使用的Transform。

### css.plugin
与[`js.plugin`]一样。

### css.dest
指定生成CSS包的根目录。

如果指定为数组，前面两个直接传给[vinyl-fs#dest]。
第三个参数用于配置url变换：

- `maxSize`: 小于该大小（kb）的资源会以base-64的形式内嵌
- `assetOutFolder`: 非内嵌的资源被拷贝到该目录下
- `name`: 重命名拷贝的资源
- `baseUrl`: 指定最终CSS中`url()`的前缀

实例：
```js
{
  css: { 
    dest: [ 
      '/path/to/build',
      null,
      { 
        // 小于5k的资源将被内嵌
        maxSize: 5,

        // 超过5k的资源被拷贝到/path/to/build/assets
        assetOutFolder: '/path/to/build/assets',

        // 被拷贝的目录将重命名
        // 例如，octocat_setup.png重命名为octocat_setup.84f6371.png
        name: '[name].[hash]',

        // 如果指定的话，所有资源都用7位sha1重命名
        // useHash: true,

        // 如果指定的话，CSS中非内嵌的资源`url()`将以`i`为前缀
        // 同时资源被拷贝到相应的目录
        // 如`i/octocat_setup.84f6371.png`
        // baseUrl: 'i',
      }
    ]
  }
}

```

### css.on
与[`js.on`]一样。

### css.postcss
详见[定制PostCSS](../tutorials/postcss.md)。

## getStyle
指定JS与CSS的绑定关系。
如果某个JS有绑定的CSS，则当其它JS依赖它时，两者绑定的CSS会自动形成对应的依赖关系。

Type: `Function`

Signature: `cssFiles = getStyle(jsFile)`

`cssFiles`即与`jsFile`绑定的样式。

```js
{
  // 将page/hi/index.js与page/hi/index.css绑定起来
  getStyle: jsFile => { 
    return path.dirname(jsFile) + '/index.css'
  }
}

```

返回值也可以是`Promise`。

## watch
直接传给[watchify2]的参数。

```js
{
  watch: { 
    // 监听新增的入口
    js: { entryGlob: 'page/**/index.js' },
    css: { entryGlob: 'page/**/index.css' },
  }
}

```

## reduce
同时指定[`js.reduce`]与[`css.reduce`]中相应的字段。

## dest
同时指定[`js.dest`]与[`css.dest`]。

## on
同时[`js.on`]与[`css.on`]。

[`js`]: #js
[`js.reduce`]: #jsreduce
[`js.dest`]: #jsdest
[`js.entries`]: #jsentries
[`js.plugin`]: #jsplugin
[`js.dest`]: #jsdest
[`js.on`]: #json
[`js.bundleOptions`]: #jsbundleoptions
[`css`]: #css
[`css.reduce`]: #cssreduce
[`css.dest`]: #cssdest
[`css.entries`]: #cssentries
[`css.plugin`]: #cssplugin
[`css.dest`]: #cssdest
[`css.on`]: #csson
[`css.bundleOptions`]: #cssbundleoptions
[`css.postcss`]: #csspostcss
[`getStyle`]: #getstyle
[`watch`]: #watch
[`reduce`]: #reduce
[`dest`]: #dest
[`on`]: #on

[vinyl-fs]: https://github.com/gulpjs/vinyl-fs
[vinyl]: https://github.com/gulpjs/vinyl
[gulp]: https://github.com/gulpjs/gulp
[vinyl-fs#src]: https://github.com/gulpjs/vinyl-fs#srcglobs-options
[vinyl-fs#dest]: https://github.com/gulpjs/vinyl-fs#destfolder-options
[browserify]: https://github.com/substack/node-browserify
[depsify]: https://github.com/reducejs/depsify
[browserify-opts]: https://github.com/substack/node-browserify#browserifyfiles--opts
[common-bundle]: https://github.com/reducejs/common-bundle
[watchify2]: https://github.com/reducejs/watchify2
