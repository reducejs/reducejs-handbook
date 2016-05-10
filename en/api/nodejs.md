## API
```js
var reduce = require('reduce-web-component')
var config = require('your-configure-file')
reduce.bundle(config).then(() => {})
reduce.watch(config)

```

* `reduce.bundle(config)`
Return a promise which resolves when bundling finishes.

* `reduce.watch(config)`
Bundle and watch for file changes.

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


[Reduce]: https://github.com/reducejs/reduce-web-component
