# fib-pug

[![Build Status](https://travis-ci.org/richardo2016/fib-pug.svg?branch=master)](https://travis-ci.org/richardo2016/fib-pug)
[![NPM version](https://img.shields.io/npm/v/fib-pug.svg)](https://www.npmjs.org/package/fib-pug)


[pugjs]:https://github.com/pugjs/pug
[semver]:https://www.npmjs.com/package/semver

[pugjs] is one popular js template engine formerly known as "Jade". It's indentation-sensitive with high-performance. But at some point, it could only run in nodejs (some version of "Jade" can run in browser), because from that point [pugjs] support APIs such as `renderFile` which required I/O runtime.

## Usage
```bash
npm i -S fib-pug
```

## I/O
As of `fs` module, luckily, fibjs has similar API with nodejs, we can run pugjs in fibjs, just do some magic hack with fibjs's `sandbox` module.

## Mechanism
```javascript
const vm = require('vm');
const sbox = new vm.SandBox({
  fs: require('fs'),
  path: require('path'),
  http: require('http'),
  https: require('http'),
  assert: require('assert'),
  url: require('url'),
  os: require('os'),
  util: require('util')
});

// one valid pug module
const pug = sbox.require('pug', __filename)
```

as code above, just provide property module for orignal `pug` module in nodejs, you can run pugjs in fibjs.

But I can't test all version of fibjs and pug, so I mark the tested version of fibjs and pug in `package.json`<del>and would notice developer if version when running this module in DEBUG mode</del>.

## API

I just support some API of [pugjs], but I belive it's enough for your project in most cases.

- `compile`
- `render`
- `renderFile`

view the parameters of this api in [pugjs]'s page.

## Sample

```javascript
#!/usr/bin/env fibjs

const locals = {testVar: '测试变量'}
const rawText = 'div\n\t| #{testVar}'

try {
  html = pug.compile(rawText, {pretty: true})(locals);
} catch (e) {
  html = ''
  error = e.message
}

console.log(html) // '<div>测试变量</div>'
```

## Specical Features

### Filters
[jstransformer-stylus]:https://www.npmjs.com/package/jstransformer-stylus
[jstransformer-typescript]:https://www.npmjs.com/package/jstransformer-typescript

If you don't know about this feature, learn more about it [here](https://pugjs.org/language/filters.html)

`pug` takes `jstransformer-*`-like packages(such as [jstransformer-stylus]) as default filter. That is, if you write `:stylus` in pug, it would try to find `jstransformer-stylus` in nodejs's common pattern.

Some `jstransformer-*`-like packages may be incompatible with fibjs, You can also customize filter as you like. View more sample usages for **filter** [here](/test/filters)
