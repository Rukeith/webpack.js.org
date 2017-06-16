---
title: 概念
sort: 1
contributors:
  - TheLarkInn
  - jhnns
  - grgur
  - johnstew
---

*webpack* 是一個現代 JavaScript 應用程式的 _模組打包工具(module bundler)_。它是 [可高度設置的](/configuration)，但在開始之前你只需要先理解**四個核心概念**：入口(entry)、輸出(output)、載入器(loaders)和插件(plugins)。

此文件旨在對這些概念進行**高階**的概述，同時提供概念具體範例的詳細敘述的連結。

## 入口 Entry

webpakc 會建立一個所有你的應用程式的相依關係圖表。其圖表的起點就是所謂的 _入口點(entry point)_。這個 _入口點(entry point)_ 會告訴 webpack _從哪裡開始_，並依照這個相依圖表來知道 _要打包什麼_。你可以將你的應用程式的 _入口點(entry point)_ 當作是 **根上下文(contextual root)** 或是 **app 的第一個啟動檔案**。

在 webpack 中，我們使用 [webpack 設置物件](/configuration) 中的 `entry` 屬性來定義我們的 _入口點(entry points)_。

以下是一個最簡單的範例：

**webpack.config.js**

```javascript
module.exports = {
  entry: './path/to/my/entry/file.js'
};
```

這裡有許多方式可以宣告你的應用程式所需要的特定 `entry` 屬性。

[暸解更多!](/concepts/entry-points)

## 輸出 Output

將你所有的靜態資源都打包一起後，你仍然需要告訴 webpack **去哪裡** 打包你的應用程式。webpack 的 `output` 屬性可以告訴 webpack **如何處理打包好的程式碼**。

**webpack.config.js**

```javascript
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  }
};
```

在上面的範例，我們使用了 `output.filename` 和 `output.path` 屬性來告訴 webpack bundle 的名稱和我想要生成在哪裡。

T> 你可能會在我們整個文件和[插件 API](/api/plugins)中看到**生成(emitted 或 emit) **。它是“生產(produced)或排出(discharged)”的特殊術語。

`output` 屬性有[更多可設置的功能](/configuration/output)，但我們先花點時間暸解一些`output`屬性的常見使用案例。

[暸解更多!](/concepts/output)


## 載入器 Loaders

目標是讓 webpack 去關注你專案中的所有靜態資源，而不是瀏覽器（這並不意味著他們都需要打包在一起）。webpack 把[每個檔案 (.css, .html, .scss, .jpg, etc.) 當作模組](/concepts/modules)。然而，webpack **只能理解 JavaScript**。

**載入器在 webpack _會將這些檔案轉換到模組_，並添加到相依圖表中。**

在高階層，webpack 的配置有兩個目的。

1. 辨識出哪些檔案應該被相對應的載入器轉換（`test` 屬性）。
2. 轉換檔案後，添加到你的相依圖表內（甚至是添加到你的 bundle 中）（`use` 屬性）。

**webpack.config.js**

```javascript
const path = require('path');

const config = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/, use: 'babel-loader'
      }
    ]
  }
};

module.exports = config;
```

以上的配置對一個單一的模組定義了一個 `rules` 屬性，包括了兩個必須的屬性：`test` 和 `use`。這告訴 webpack 的 compiler 如下：

> "嘿，webpack comiler，當你經過在 `require()`/`import` 宣告中被解析為 '.js' 或 '.jsx' 的檔案時，在它們被你加到 bundle 之前**使用** `babel-loader` 去轉換"。

W> It is important to remember when defining rules in your webpack config, you are defining them under `module.rules` and not `rules`. However webpack will yell at you when doing this incorrectly.

There are more specific properties to define on loaders that we haven't yet covered.

[Learn more!](/concepts/loaders)

## Plugins

Since Loaders only execute transforms on a per-file basis, `plugins` are most commonly used (but not limited to) performing actions and custom functionality on "compilations" or "chunks" of your bundled modules [(and so much more)](/concepts/plugins). The webpack Plugin system is [extremely powerful and customizable](/api/plugins).

In order to use a plugin, you just need to `require()` it and add it to the `plugins` array. Most plugins are customizable via options. Since you can use a plugin multiple times in a config for different purposes, you need to create an instance of it by calling it with `new`.

**webpack.config.js**

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
const webpack = require('webpack'); //to access built-in plugins
const path = require('path');

const config = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      {test: /\.(js|jsx)$/, use: 'babel-loader'}
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};

module.exports = config;
```

There are many plugins that webpack provides out of the box! Check out our [list of plugins](/plugins) for more information.

Using plugins in your webpack config is straight-forward, however there are many use-cases that are worth discussing further.

[Learn more!](/concepts/plugins)
