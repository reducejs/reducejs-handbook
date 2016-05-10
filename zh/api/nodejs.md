
## API
```js
var reduce = require('reduce-web-component')
var config = require('your-configure-file')
reduce.bundle(config).then(() => {})
reduce.watch(config)

```

* `reduce.bundle(config)`
打包，并返回一个在打包结束时resolve的Promise。

* `reduce.watch(config)`
打包，并watch文件变化。

Refer to [configure](configure.md) for more information.

## Gulp

```js
var gulp = require('gulp')
var reduce = require('reduce-web-component')
var config = require('your-configure-file')

gulp.task('build', () => {
  return reduce.bundle(config)
})

gulp.task('watch', (cb) => {
  reduce.watch(config).on('close', cb)
})

```

