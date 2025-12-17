# NOTE

> [!NOTE] This is a fork of [mozilla/pdfjs]( https://github.com/mozilla/pdf.js), with 3 changes:
> 
> 1. Export ES modules in both .js and .mjs formats, since nginx's default MIME types do not serve .mjs files as `application/javascript`.
> 2. Downgrade the target browsers for the legacy build to ES6 (more core-js polyfills).
> 3. Added extra handling for web/DOM APIs used in worker scripts that are not polyfilled by core-js, since bundler like Vite do not automatically polyfill these APIs inside worker scripts.
>       - Add a `typeof` check for `FinalizationRegistry`, as it is not supported in older browsers.

 
> [!TIP] This repository is regularly merged and synchronized with the upstream [[mozilla/pdfjs]]. When a new version is released upstream, a new version with the same version number will be released here.

# Quick Start

install

```bash
npm install @okkkde/pdfjs-dist
```

use in vite

```typescript
const pdfjsLib = await import('@okkkde/pdfjs-dist');
// you can use the legacy worker for improved compatibility
const pdfjsWorker = await import('@okkkde/pdfjs-dist/legacy/build/pdf.worker.js?worker&url');
// or modern worker which works on modern browsers, choose at your own
const pdfjsWorker = await import('@okkkde/pdfjs-dist/build/pdf.worker.js?worker&url');
pdfjsLib.GlobalWorkerOptions.workerSrc = pdfjsWorker.default;
```

# 提示

> [!NOTE] 本仓库是 [[mozilla/pdfjs]] 的 fork，有三点改动：
>
> 1. 导出 ES 模块时同时提供 `.js` 与 `.mjs`，解决 nginx 默认不按 `application/javascript` 提供 `.mjs` 的问题。
> 2. legacy 构建的目标浏览器降级到 ES6（更多的 core-js 模块）。
> 3. 为 worker 脚本中未被 core-js 覆盖的 web/DOM API 做了额外处理，因为像 Vite 这类打包器不会自动在 worker 里填充这些 API。
>       - 为 `FinalizationRegistry` 增加 `typeof` 检查，以兼容不支持该特性的旧浏览器。

> [!TIP] 本仓库会定期与上游 [[mozilla/pdfjs]] 合并同步，上游有新版本时跟随发布新的版本号一致的版本。

# 快速开始

安装

```bash
npm install @okkkde/pdfjs-dist
```

在 vite 中使用

```typescript
const pdfjsLib = await import('@okkkde/pdfjs-dist');
// 为了更好的兼容性，可以使用 legacy worker
const pdfjsWorker = await import('@okkkde/pdfjs-dist/legacy/build/pdf.worker.js?worker&url');
// 或者使用现代 worker，适用于现代浏览器，根据需要选择
const pdfjsWorker = await import('@okkkde/pdfjs-dist/build/pdf.worker.js?worker&url');
pdfjsLib.GlobalWorkerOptions.workerSrc = pdfjsWorker.default;
```

# PDF.js [![CI](https://github.com/mozilla/pdf.js/actions/workflows/ci.yml/badge.svg?query=branch%3Amaster)](https://github.com/mozilla/pdf.js/actions/workflows/ci.yml?query=branch%3Amaster)

[PDF.js](https://mozilla.github.io/pdf.js/) is a Portable Document Format (PDF) viewer that is built with HTML5.

PDF.js is community-driven and supported by Mozilla. Our goal is to
create a general-purpose, web standards-based platform for parsing and
rendering PDFs.

## Contributing

PDF.js is an open source project and always looking for more contributors. To
get involved, visit:

+ [Issue Reporting Guide](https://github.com/mozilla/pdf.js/blob/master/.github/CONTRIBUTING.md)
+ [Code Contribution Guide](https://github.com/mozilla/pdf.js/wiki/Contributing)
+ [Frequently Asked Questions](https://github.com/mozilla/pdf.js/wiki/Frequently-Asked-Questions)
+ [Good Beginner Bugs](https://github.com/mozilla/pdf.js/issues?q=is%3Aissue%20state%3Aopen%20label%3Agood-beginner-bug)
+ [Projects](https://github.com/mozilla/pdf.js/projects)

Feel free to stop by our [Matrix room](https://chat.mozilla.org/#/room/#pdfjs:mozilla.org) for questions or guidance.

## Getting Started

### Online demo

Please note that the "Modern browsers" version assumes native support for the
latest JavaScript features; please also see [this wiki page](https://github.com/mozilla/pdf.js/wiki/Frequently-Asked-Questions#faq-support).

+ Modern browsers: https://mozilla.github.io/pdf.js/web/viewer.html

+ Older browsers: https://mozilla.github.io/pdf.js/legacy/web/viewer.html

### Browser Extensions

#### Firefox

PDF.js is built into version 19+ of Firefox.

#### Chrome

+ The official extension for Chrome can be installed from the [Chrome Web Store](https://chrome.google.com/webstore/detail/pdf-viewer/oemmndcbldboiebfnladdacbdfmadadm).
*This extension is maintained by [@Rob--W](https://github.com/Rob--W).*
+ Build Your Own - Get the code as explained below and issue `npx gulp chromium`. Then open
Chrome, go to `Tools > Extension` and load the (unpackaged) extension from the
directory `build/chromium`.

## Getting the Code

To get a local copy of the current code, clone it using git:

    $ git clone https://github.com/mozilla/pdf.js.git
    $ cd pdf.js

Next, install Node.js via the [official package](https://nodejs.org) or via
[nvm](https://github.com/creationix/nvm). If everything worked out, install
all dependencies for PDF.js:

    $ npm install

Finally, you need to start a local web server as some browsers do not allow opening
PDF files using a `file://` URL. Run:

    $ npx gulp server

and then you can open:

+ http://localhost:8888/web/viewer.html

Please keep in mind that this assumes the latest version of Mozilla Firefox; refer to [Building PDF.js](https://github.com/mozilla/pdf.js/blob/master/README.md#building-pdfjs) for non-development usage of the PDF.js library.

It is also possible to view all test PDF files on the right side by opening:

+ http://localhost:8888/test/pdfs/?frame

## Building PDF.js

In order to bundle all `src/` files into two production scripts and build the generic
viewer, run:

    $ npx gulp generic

If you need to support older browsers, run:

    $ npx gulp generic-legacy

This will generate `pdf.js` and `pdf.worker.js` in the `build/generic/build/` directory (respectively `build/generic-legacy/build/`).
Both scripts are needed but only `pdf.js` needs to be included since `pdf.worker.js` will
be loaded by `pdf.js`. The PDF.js files are large and should be minified for production.

## Using PDF.js in a web application

To use PDF.js in a web application you can choose to use a pre-built version of the library
or to build it from source. We supply pre-built versions for usage with NPM under
the `pdfjs-dist` name. For more information and examples please refer to the
[wiki page](https://github.com/mozilla/pdf.js/wiki/Setup-pdf.js-in-a-website) on this subject.

## Including via a CDN

PDF.js is hosted on several free CDNs:
 - https://www.jsdelivr.com/package/npm/pdfjs-dist
 - https://cdnjs.com/libraries/pdf.js
 - https://unpkg.com/pdfjs-dist/

## Learning

You can play with the PDF.js API directly from your browser using the live demos below:

+ [Interactive examples](https://mozilla.github.io/pdf.js/examples/index.html#interactive-examples)

More examples can be found in the [examples folder](https://github.com/mozilla/pdf.js/tree/master/examples/). Some of them are using the pdfjs-dist package, which can be built and installed in this repo directory via `npx gulp dist-install` command.

For an introduction to the PDF.js code, check out the presentation by our
contributor Julian Viereck:

+ https://www.youtube.com/watch?v=Iv15UY-4Fg8

More learning resources can be found at:

+ https://github.com/mozilla/pdf.js/wiki/Additional-Learning-Resources

The API documentation can be found at:

+ https://mozilla.github.io/pdf.js/api/

## Questions

Check out our FAQs and get answers to common questions:

+ https://github.com/mozilla/pdf.js/wiki/Frequently-Asked-Questions

Talk to us on Matrix:

+ https://chat.mozilla.org/#/room/#pdfjs:mozilla.org

File an issue:

+ https://github.com/mozilla/pdf.js/issues/new/choose
