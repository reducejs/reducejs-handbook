## Requirements
Node.js >= 4

## Node.js
```bash
npm install --save-dev reduce-web-component

```

```js
var reduce = require('reduce-web-component')

// Start bundling
var donePromise = reduce.bundle(config)

// Start bundling and watch file changes, addition and deletion
var watchDonePromise = reduce.watch(config)

```

More information:
* [Configure](/api/configure/)
* [API](/api/nodejs/)

## Command line
```bash

npm install reduce-web-component -g

```

Run `reduce -h` to see the man page.

