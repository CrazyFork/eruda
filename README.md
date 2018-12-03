
# notes

annotation:
```
:n, notes

```


* read it buiding script first, at least give it peek, this provides some insight how this project is constructed.
* this lib make plenty use of EventEmitter pattern 
* `Network`, I'm pretty insterest in this section, wondering how network 



```
src
├── Console
│   ├── Console.js                          // entry
│   ├── Log.js                              // 模拟了chrome dev tools console log不同数据类型的能力, table, json 格式的支持
│   ├── Logger.js                           // 定义了如何输出log信息, 虽然没有完全弄清Logger和Log的区别, 但是大致知道怎么创建一个类似于chrome dev tools的log页面了
│   ├── cmdList.json                        // jquery, loadash 库命令
│   ├── libraries.json                      //  jquery, loadash 资源路径
│   └── stringify.js                        // 递归output string out of object的能力
├── DevTools                                // 整个DevTools页面, 包含其他Tools(Console, Elements, EntryBtn, Info...etc)
│   ├── DevTools.js
│   ├── NavBar.js
│   └── Tool.js
├── Elements                                // 用于展示目标元素样式. 
│   ├── CssStore.js                         // collect css rules that match a specific element, 
│   ├── Elements.js                         // Elements, 主要是用于展示元素样式
│   ├── Highlight.js
│   ├── Select.js
│   └── defComputedStyle.json
├── EntryBtn
│   ├── EntryBtn.js                         // eruda 的齿轮按钮, 定义了draggable行为, 
├── Info                                    // 信息面板
│   ├── Info.js
│   ├── defInfo.js
├── Network
│   ├── FetchRequest.js                     // 和 XhrRequest 对应的 fetch 函数的 counterpart, 收集network的行为
│   ├── Network.js                          // Network 面板, 看了下代码eruda收集网络请求是在xhr和fetch方法分别作了封装, 在请求周期特定位置插入了自定义的行为去收集网络请求的结果
│   ├── XhrRequest.js                       // XhrRequest, 对不同阶段的xhr处理的封装
│   └── util.js                             
├── Resources                               // Resource面板
│   ├── Resources.js
├── Settings
│   ├── Settings.js                         // 就是一个key value store 用了localStorage, 然后提供了一个UI修改
├── Snippets
│   ├── Snippets.js
│   ├── defSnippets.js
├── Sources
│   ├── Sources.js
├── index.js
├── lib
│   ├── JsonViewer.js
│   ├── Storage.js
│   ├── config.js
│   ├── emitter.js
│   ├── extraUtil.js
│   ├── getAbstract.js
│   ├── highlight.js
│   ├── logger.js
│   ├── stringify.js
│   ├── stringifyUtil.js
│   ├── stringifyWorker.js
│   └── util.js                             // 各种utils工具类, 写法有些古老
└── style
```



* `evalCss`: this function is used to load css style into page.
* utils:
    * `Logger`, the implementation is much simpler than i thought, just a console with a logger level. lower logger level than configed simply got skipped when logged to console.



## third party libs
* draggabilly, 通用化drag的操作封装



##

```
export function lenToUtf8Bytes(str) {
  let m = encodeURIComponent(str).match(/%[89ABab]/g)

  return str.length + (m ? m.length : 0)
}


// evaljs, todo: what use of these parenthesis for eval call 
let evalJs = jsInput => {
  let ret

  try {
    ret = eval.call(window, `(${jsInput})`)
  } catch (e) {
    ret = eval.call(window, jsInput)
  }

  return ret
}


// Resource面板中用于keep sync with the resources(cookie, localStorage, img, ...etc)的核心代码
// MutationObserver我记不清标准化了没有, 记得好像其他浏览器好像没有标准化这个东西
this._observer = new MutationObserver(mutations => {
    let needToRender = false
    each(mutations, mutation => {
    if (this._handleMutation(mutation)) needToRender = true
    })
    if (needToRender) this._render()
})
```

## todos
* `let entries = this._performance.getEntries()`
* `MutationObserver`











<a href="https://eruda.liriliri.io/" target="_blank">
    <img src="http://7xn2zy.com1.z0.glb.clouddn.com/github_eruda2.jpg">
</a>

[中文](doc/README_CN.md)

# Eruda

[![Join the chat at https://gitter.im/liriliri/eruda][gitter-image]][gitter-url]
[![NPM version][npm-image]][npm-url]
[![Build status][travis-image]][travis-url]
[![Test coverage][codecov-image]][codecov-url]
[![License][license-image]][npm-url]

[gitter-image]: https://badges.gitter.im/liriliri/eruda.svg
[gitter-url]: https://gitter.im/liriliri/eruda?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge
[npm-image]: https://img.shields.io/npm/v/eruda.svg
[npm-url]: https://npmjs.org/package/eruda
[travis-image]: https://img.shields.io/travis/liriliri/eruda.svg
[travis-url]: https://travis-ci.org/liriliri/eruda
[codecov-image]: https://codecov.io/github/liriliri/eruda/coverage.svg?branch=master
[codecov-url]: https://codecov.io/github/liriliri/eruda?branch=master
[license-image]: https://img.shields.io/npm/l/eruda.svg

Console for Mobile Browsers.

![Eruda](http://7xn2zy.com1.z0.glb.clouddn.com/eruda_screenshot4.jpg)

## Why

Logging things out on mobile browser is never an easy stuff. I used to include `window onerror alert` script inside pages to find out JavaScript errors, kind of stupid and inefficient. Desktop browser DevTools is great, and I wish there is a similar one on mobile side, which leads to the creation of Eruda.

## Demo

![Demo](http://7xn2zy.com1.z0.glb.clouddn.com/eruda_qrcode3.png)

Browse it on your phone: [https://eruda.liriliri.io/](https://eruda.liriliri.io/)

In order to try it for different sites, execute the script below on browser address bar.

```javascript
javascript:(function () { var script = document.createElement('script'); script.src="//cdn.jsdelivr.net/npm/eruda"; document.body.appendChild(script); script.onload = function () { eruda.init() } })();
```

## Features

* [Console](doc/TOOL_API.md#console): Display JavaScript logs.
* [Elements](doc/TOOL_API.md#elements): Check dom state.
* [Network](doc/TOOL_API.md#network): Show requests status.
* [Resource](/doc/TOOL_API.md#resources): Show localStorage, cookie information.
* [Info](doc/TOOL_API.md#info): Show url, user agent info.
* [Snippets](doc/TOOL_API.md#snippets): Include snippets used most often.
* [Sources](doc/TOOL_API.md#sources): Html, js, css source viewer.

## Install

You can get it on npm.

```bash
npm install eruda --save
```

Add this script to your page.

```html
<script src="node_modules/eruda/eruda.min.js"></script>
<script>eruda.init();</script>
```

It's also available on [jsDelivr](http://www.jsdelivr.com/projects/eruda) and [cdnjs](https://cdnjs.com/libraries/eruda).

```html
<script src="//cdn.jsdelivr.net/npm/eruda"></script>
<script>eruda.init();</script>
```

The JavaScript file size is quite huge(about 100kb gzipped) and therefore not suitable to include in mobile pages. It's recommended to make sure eruda is loaded only when eruda is set to true on url(http://example.com/?eruda=true), for example:

```javascript
;(function () {
    var src = 'node_modules/eruda/eruda.min.js';
    if (!/eruda=true/.test(window.location) && localStorage.getItem('active-eruda') != 'true') return;
    document.write('<scr' + 'ipt src="' + src + '"></scr' + 'ipt>');
    document.write('<scr' + 'ipt>eruda.init();</scr' + 'ipt>');
})();
```

## Configuration

When initialization, a configuration object can be passed in.

* container: Container element. If not set, it will append an element directly
under html root element.
* tool: Choose which default tools you want, by default all will be added.

For more information, please check the [documentation](doc/API.md).

```javascript
let el = document.createElement('div');
document.body.appendChild(el);

eruda.init({
    container: el,
    tool: ['console', 'elements']
});
```

## Plugins

It is possible to enhance Eruda with more features by writing plugins. Check source code of plugins below to learn how to write your own custom tool panels. Besides, [eruda-plugin](https://github.com/liriliri/eruda-plugin) is available for plugin initialization.

* [eruda-fps](https://github.com/liriliri/eruda-fps): Display page fps info.
* [eruda-features](https://github.com/liriliri/eruda-features): Browser feature detections.
* [eruda-timing](https://github.com/liriliri/eruda-timing): Show performance and resource timing.
* [eruda-memory](https://github.com/liriliri/eruda-memory): Display page memory info.
* [eruda-code](https://github.com/liriliri/eruda-code): Run JavaScript code.
* [eruda-benchmark](https://github.com/liriliri/eruda-benchmark): Run JavaScript benchmarks.
* [eruda-geolocation](https://github.com/liriliri/eruda-geolocation): Test geolocation.
* [eruda-dom](https://github.com/liriliri/eruda-dom): Navigate dom tree.
* [eruda-orientation](https://github.com/liriliri/eruda-orientation): Test orientation api.

When writing plugins, you can use utilities exposed by Eruda, see [docs](doc/UTIL_API.md) here.

## Contribution

Read [Contributing Guide](.github/CONTRIBUTING.md) for development setup instructions.
