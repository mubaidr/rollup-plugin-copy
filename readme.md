# rollup-plugin-copy

[![Build Status](https://travis-ci.org/vladshcherbin/rollup-plugin-copy.svg?branch=master)](https://travis-ci.org/vladshcherbin/rollup-plugin-copy)
[![Codecov](https://codecov.io/gh/vladshcherbin/rollup-plugin-copy/branch/master/graph/badge.svg)](https://codecov.io/gh/vladshcherbin/rollup-plugin-copy)

Copy files and folders, with glob support.

## Installation

```bash
# yarn
yarn add rollup-plugin-copy -D

# npm
npm install rollup-plugin-copy -D
```

## Usage

```js
// rollup.config.js
import copy from 'rollup-plugin-copy'

export default {
  input: 'src/index.js',
  output: {
    file: 'dist/app.js',
    format: 'cjs'
  },
  plugins: [
    copy({
      targets: [
        { src: 'src/index.html', dest: 'dist/public' },
        { src: 'assets/images/**/*', dest: 'dist/public/images' }
      ]
    })
  ]
}
```

### Configuration

There are some useful options:

#### targets

Type: `Array` | Default: `[]`

Array of targets to copy. A target is an object with properties:

- **src** (`string` `Array`): Path or glob of what to copy
- **dest** (`string` `Array`): One or more destinations where to copy
- **rename** (`string` `Function`): Change destination file or folder name

Each object should have **src** and **dest** properties, **rename** is optional. [globby](https://github.com/sindresorhus/globby) is used inside, check it for glob pattern examples.

##### File

```js
copy({
  targets: [{ src: 'src/index.html', dest: 'dist/public' }]
})
```

##### Folder

```js
copy({
  targets: [{ src: 'assets/images', dest: 'dist/public' }]
})
```

##### Glob

```js
copy({
  targets: [{ src: 'assets/*', dest: 'dist/public' }]
})
```

##### Glob: multiple items

```js
copy({
  targets: [{ src: ['src/index.html', 'src/styles.css', 'assets/images'], dest: 'dist/public' }]
})
```

##### Glob: negated patterns

```js
copy({
  targets: [{ src: ['assets/images/**/*', '!**/*.gif'], dest: 'dist/public/images' }]
})
```

##### Multiple targets

```js
copy({
  targets: [
    { src: 'src/index.html', dest: 'dist/public' },
    { src: 'assets/images/**/*', dest: 'dist/public/images' }
  ]
})
```

##### Multiple destinations

```js
copy({
  targets: [{ src: 'src/index.html', dest: ['dist/public', 'build/public'] }]
})
```

##### Rename with a string

```js
copy({
  targets: [{ src: 'src/app.html', dest: 'dist/public', rename: 'index.html' }]
})
```

##### Rename with a function

```js
copy({
  targets: [{
    src: 'assets/docs/*',
    dest: 'dist/public/docs',
    rename: (name, extension) => `${name}-v1.${extension}`
  }]
})
```

#### verbose

Type: `boolean` | Default: `false`

Output copied items to console.

```js
copy({
  targets: [{ src: 'assets/*', dest: 'dist/public' }],
  verbose: true
})
```

#### hook

Type: `string` | Default: `buildEnd`

Rollup hook the plugin should use.

```js
copy({
  targets: [{ src: 'assets/*', dest: 'dist/public' }],
  hook: 'buildStart'
})
```

#### copyOnce

Type: `boolean` | Default: `false`

Copy items once. Useful in watch mode.

```js
copy({
  targets: [{ src: 'assets/*', dest: 'dist/public' }],
  copyOnce: true
})
```

All other options are passed to packages, used inside:
  - [globby](https://github.com/sindresorhus/globby)
  - [fs-extra copy function](https://github.com/jprichardson/node-fs-extra/blob/7.0.0/docs/copy.md)

## Original Author

[Cédric Meuter](https://github.com/meuter)

## License

MIT
