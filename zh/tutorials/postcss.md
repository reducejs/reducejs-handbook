## 什么是PostCSS
[PostCSS]是一款拥有非常灵活的插件机制的CSS预处理工具。
可以通过编写插件来实现具体的预编译功能，如变量声明、mixin等。

## 默认使用插件
Reduce通过使用[reduce-css-postcss]来支持[PostCSS]。
默认使用如下[PostCSS]插件来支持[SASS]风格的样式源码编写：

* [postcss-simple-import]。支持`@import`。
* [postcss-custom-url]。自动变换CSS中的url。
* [postcss-advanced-variables]。支持变量。
* [postcss-mixins]。支持mixin。
* [postcss-nested]。支持嵌套。
* [postcss-extend]。支持扩展。
* [autoprefixer]。自动添加浏览器厂商前缀。

## 如何修改插件列表

添加插件：
```js
var config = {
  css: {
    reduce: {
      postcss: [
        require('postcss-custom-properties'),
        require('postcss-strip-units'),
        require('postcss-calc'),
      ],
    },
  },
}

```

修改已有插件列表：
```js
var config = {
  css: {
    reduce: {
      postcss: function (pluginList) {
        // 使用postcss-simple-vars替换postcss-advanced-variables
        pluginList.splice(
          'postcss-advanced-variables', 1, require('postcss-simple-vars')
        )
        // 添加postcss-calc
        pluginList.push(require('postcss-calc'))
      },
    },
  },
}

```

`pluginList`是一个[postcss-processor-splicer]实例。

[autoprefixer]: https://github.com/postcss/autoprefixer
[PostCSS]: https://github.com/postcss/postcss
[postcss-advanced-variables]: https://github.com/jonathantneal/postcss-advanced-variables
[postcss-custom-url]: https://github.com/zoubin/postcss-custom-url
[postcss-extend]: https://github.com/travco/postcss-extend
[postcss-mixins]: https://github.com/postcss/postcss-mixins
[postcss-nested]: https://github.com/postcss/postcss-nested
[postcss-processor-splicer]: https://github.com/zoubin/postcss-processor-splicer
[postcss-simple-import]: https://github.com/zoubin/postcss-simple-import
[reduce-css-postcss]: https://github.com/reducejs/reduce-css-postcss
[SASS]: http://sass-lang.com/
