
## 实例
[实例代码地址](https://github.com/reducejs/reduce-web-component/tree/master/example/react)

关键配置在reduce.config.js的中的js属性，我们需要为js打包配置react

```
  js: {
    reduce: {
      transform: [
        ['babelify', {presets: ["es2015", "react"]}],
      ],
      extensions: '.jsx',
    },
    entries: 'page/**/index.jsx',
    bundleOptions: 'bundle.js',
  },
```


## 配备哪些Transform

__babelify__是reduce进行babel转译的transform。 

我们给它配置的两个presets: es2015和react, 表示它将使用es2015和react的转译规则对jsx文件进行转译


## 热替换

