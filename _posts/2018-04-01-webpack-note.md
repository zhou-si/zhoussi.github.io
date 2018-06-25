---
layout: post
title: Webpack 笔记
tag: 工具
---

![webpack]({{site.baseurl}}/images/posts/webpack.png)

## 概念

本质上：

- webpack 是一个现代 JS 应用程序的静态模块打包器（modulr bundler）；
- 它会递归地构建一个依赖关系图（dependency graph）;
- 将包含应用程序需要的每个模块打包成一个或多个`bundle`

四个 **核心概念**：

- 入口（entry）
- 输出（output）
- loader
- 插件（plugins）

### 入口（entry）

- **入口起点（entry point）** ，作为webpack构建起内部依赖图的开始的模块 ；
- webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的；
- 依赖被处理，输出到称之为 *bundles*的文件夹。

在`webpack.config.js`（webpack配置）中配置`entry`属性，来指定一个/多个入口起点：

```js
// webpack.config.js

module.exports = {
    entry: './path/to/my/entry/file.js'
};
```

### 出口（output）

output 属性告诉 webpack 在哪里输出它所创建的 *bundles*，以及如何命名这些文件：

```js
// webpack.config.js

module.exports = {
    entry: './path/to/my/entry/file.js',
    output: {
// 通过output.path 和 output.filename 属性，告诉 webpack bundle的名称，以及想要生成（emit）到哪里
        path.resolve(__dirname, 'dist'),
        filename: 'my-first-webpack.bundle.js'
    }
};
```

术语 **生成**（emitted 或 emit）是“生产（produced）”或 “释放（discharged）”的特殊术语。

### loader

- webpack 自身只理解 JS；
- loader可以让所有非 JS 文件转换为 webpack 能够处理的有效模块；
- 本质上，webpack loader 将所有类型的文件，转换为应用程序的依赖图（和最终的bundle）可以直接引用的模块。

> 注意，loader 能够 `import`导入任何类型的模块，这是 webpack特有的功能，其他打包程序或任务执行器可能不支持

webpack 的配置中 **loader**有两个目标：

1. `test`属性，用于标识出应该被对应的 loader 进行转换的某个或某些文件；
2. `use`属性，标识进行转换时，应该使用哪个 loader。

```js
// webpack.config.js

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
                test: /\.txt$/,
                use: 'raw-loader'
            }
        ]
    }
};

module.exports = config;
```

告诉webpack编译器，碰到“在 `require()/import`语句中被解析为`.txt`的路径时，对它打包之前，先使用 raw-loader 转换一下。

> 要记得，在 webpack 配置中定义 loader 时， 要定义在 `module.rules`中， 而不是 `rules`。

### 插件（plugins）

- loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。
- 插件的范围包括：从打包优化和压缩，一直到重新定义环境中的变量；
- 插件接口功能极其强大，可以用来处理各种各样的任务。

使用：

- 使用一个插件，你只需要`require()`它，然后把它添加到`plugins`数组中；
- 多数插件可以通过选项（option）自定义；
- 你也可以在一个配置文件中因为不同目的多次使用同一个插件，这时需要通过使用`new`操作符来创建它的一个实例

```js
// webpack.config.js

const HtmlWebpackPlugin = require('html-webpack-plugin');   // 通过 npm 安装
const webpack = require('webpack');                         // 用于方位内置插件
const path = require('path');

const config = {
    entry: './path/to/my/entry/file.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'my-first-webpack.bundle.js'
    },
    module: {
        rules: [
            test: /\.txt$/,
            use: 'raw-loader'
        ]
    },

    plugins: [
        new webpack.optimize.UglifyJsPlugin(),
        new HtmlWebpackPlugin({templare: './src/index.html'})
    ]
};

module.exports = config;

```
## 入口起点（Entry Points）

### 单个入口（简写）语法

用法： `entry: string|Array<string>`

```js
// webpack.cofig.js

const config = {
    entry: './path/to/my/entry/file.js'
};

module.exprots = config;
```
entry 属性的单个入口语法，是下面的简写：
```js
const config = {
    entry: {
        main: './path/to/my/entry/file.js'
    }
};
```

> 当你向`entry` 传入一个数组时，将创建多个主入口（multi-main entry），当你想要多个依赖文件一起注入，并且将他们的依赖导向（graph）到一个“chunk”时，传入数组的方式就很有用。

适用于：只有一个入口起点的应用程序或工具（即 library）快速设置 webpack 配置的时候；但是词语发在扩展配置时有失灵活性。

### 对象语法

用法： `entry: {[entryChunkName: string]: string|Array<string>}`

```js
// webpack.config.js

const config = {
    entry: {
        app: './src/app.js',
        vendors: './src/vendors.js'
    }
};

```

适用于：对象与法会比较繁琐，然而，这是应用程序中定义入口的最可扩展的方式。

> **可扩展的 webpack 配置***是指，可重用并且可以与其他配置组合使用。这是一种流行的技术，用于将关注点（concern）从环境（environment）、构建目标（build target）、运行时（runtime）中分离。然后使用专门的工具（如 webpack-nmerge）将它们合并。

### 常用场景

#### 分离 应用程序（app）和 第三方库（vendor）入口

```js
// webpack.config.js

const config = {
    entry: {
        app: './src/app.js',
        vendors: './src/vendors.js'
    }
};

```

**这是什么？** 常见于只有一个入口起点（不包括 vendor）的单页应用程序（single page application）中；

**为什么？** 此设置允许你使用 `CommonsChunkPlugin` 从“应用程序 bundle”中提取 vendor 引用到 vendor bundle，并把引用 vendor 的部分替换为 ——webpack_require__() 调用。如果应用程序 bundle 中没有 vendor 代码，那么你可以在 webpack 中实现被称为长效缓存的通用模式。

#### 多页面应用程序

```js
// webpack.config.js

const config = {
    entry: {
        pageOne: './src/pageOne/index.js',
        pageTwo: './src/pageTwo/index.js',
        pageThree: './src/pageThree/index.js'
    }
};
```

**这是什么？**告诉 webpack 需要3个独立分离的依赖图；

**为什么？**在多页应用中，页面跳转时，服务器将为你获取一个新的HTML文档。页面重新加载新文档，并且资源被重新下载。然而，这给了我们特殊的机会去做很多事：

- 使用 `CommonChunkPlugin`为每个页面的应用程序共享代码创建 bundle；
- 由于入口起点增多，多页应用能够服用入口起点之间的大量代码/模块，从而可以极大地从这些技术中受益。

> 根据经验，每个 HTML 文件，只使用一个入口起点。

## 输出（Output）

配置 `output` 选项可以控制 webpack 如何想硬盘写入编译文件，注意，及时可以存在多个`入口`起点，但只指定一个`输出`配置。

### 用法（Usage） 

在webpack 中配置 output 属性的最低要求是，将它的值设置为一个对象，包括以下两点：

- `filename` 用于输出文件的文件名；
- 目标输出目录`path` 的绝对路径。

```js
// webpack.config.js

const config = {
    output: {
        filename: 'bundle.js',
        path: '/home/proj/public//assets'
    }
};

module.exports = config;

// 此配置将一个单独的 `bundle.js` 文件输出到 `/home/proj/public/assets` 目录中。
```

### 多个入口起点

如果配置创建了多个单独的“chunk”(例如，使用多个入口起点或使用像 CommonChunkPlugin 这样的插件)，则应该使用**占位符**（substitutions）来确保每个文件具有唯一的名称。

```js
{
    entry： {
        app: './src/app.js',
        search: './src/search.js'
    },
    output: {
        filename: '[name].js',
        path: __dirname + '/dist'
    }
}

// 写入到硬盘： ./dist/app.js , ./dist/search.js
```

### 高级进阶

以下是使用 CDN 和资源 hash 的复杂示例：

```js
// config.js

output: {
    path: "/home/proj/cdn/assets/[hash]",
    publicPath: "http://cdn.example.com/assets/[hash]/"
}
```
在编译时不知道最终输出文件的`publicPath`的情况下，`publicPath`可以留空，并且在入口起点运行时动态设置。如果你在编译时不知道`publicPaht`，可以先忽略它，并在入口起点设置`__webpack_public_path__`:
```js
__webpack_public_path__ = myRuntimePublicPath

// 剩余的应用程序入口
```

## Loader

- loader用于对模块的源代码进行转换；
- loader可以使你在`import`或"加载"模块时预处理文件。
- loader类似与其他构建工具中“任务（task）”，并提供了处理前端构建步骤的强大方法；
- loader可以将文件从不同的语言（如 TypeScript）转换为 JS，获奖内联图像转换为 data URL。
- loader甚至允许你在 JS模块中 import CSS 文件

### 示例

例如。使用 loader 告诉 webpack 加载 CSS 文件，或者将 TypeScript 转为 JS。为此首先安装相应的 loader:

```sh
npm install --save-dev css-loader
npm install --save-dev ts-loader
```
然后，只是 webpack对每个`.css`使用`css-loader`，以及对所有的`.ts`文件使用`ts-loader`:

```js
module.exports = {
    module: {
        rules: [
            {test: /\.css$/, use: 'css-loader'},
            {test:/\.ts$/, use: 'ts-loader'}
        ]
    }
};
```

### 使用 Loader

三种使用 loader 的方式：

1. 配置（推荐）：在 webpack.config.js 文件中指定 loader;
2. 内联：在每个`import`语句中显示指定 loader;
3. CLI：在shell命令中指定它们。

### 配置（configuration）

- `module.rules`允许你在 webpack 配置中指定多个 loader；
- 它是展示 loader 的一种简明方式，并且有助于是代码变得简洁；
- 同时让你对各个 loader 有个全局概览：

```js
module: {
    rules: [
        {
            test: /\.css$/,
            use: [
                {loader: 'style-loader'},
                {
                    loader: 'css-loader',
                    options: {
                        modules: true
                    }
                }
            ]
        }
    ]
}
```

### 内联

- 可以在 `import`语句或任何等效于“import”的方式中指定 loader;
- 使用`!`将资源中的`loader`分开，分开的每个部分都相对于当前目录解析。

```js
import Styles from 'style-loader!css-loader?modules!./style.css';
```

### CLI

你也可以通过CLI使用 loader：
```sh
webpack --module-bing jade-loader --module-bind 'css=style-loader!css-loader'

# 这会对 .jade 文件使用 jade-loader，对 .css 文件使用 style-loader 和 css-loader
```
### Loader 特性

- loader 支持链式传递。能够对资源使用流水线（pipeline）。一组链式的 loader 将按照相反的顺序执行；
- loader 可以是同步的，也可以是异步的；
- loader 运行在 Node.js 中， 并且能够执行任何可能的操作；
- loader 接受查询参数，用于对 loader 传递配置；
- loader 也能够使用 `options`对象进行配置
- 除了使用`package.json`常见的`main`属性，还可以将普通的`npm `模块导出为 loader，做法是在 `package.json`中定义一个`loader`字段；
- 插件（plugin）可以为 loader 带来更多特性；
- loader 能够产生额外的任意文件。

loader 通过（loader）预处理函数，为JS生态系统提供了更多能力。用户可以更加灵活地引入细粒度逻辑，例如压缩、打包、语言翻译等。

### 解析 Loader

- loader 遵循标准的模块解析，loader 将从模块路径（通常将模块路径认为是`npm install`, `node_modules`）解析；
- loader 模块需要导出为一个函数，并且使用 Node.js 兼容的 JS 编写；
- 通常使用 npm 进行管理，也可以将自定义 loader 作为应用程序中的文件；
- 按照约定，loader 通常被命名为`xxx-loader`

## 插件（Plugins）

- 插件是 webpack 的支柱功能；
- webpack 自身也是构建于，你在 webpack 配置中用到的相同的插件系统之上！
- 插件目的在于解决 loader 无法实现的其他事

### 剖析

- webpack 插件是一个具有`apply`属性的 JS 对象；
- `apply`属性会被 webpack compiler 调用，并且 compiler对象可在整个编译声明周期访问

```js
// ConsoleLogOnBuildWebpackPlugin.js

function ConsoleLogOnBuildWebpackPlugin() {

};

ConsoleLogOnBuildWebpackPlugin.prototype.apply = function(compiler) {
    compiler.plugin('run', function(compiler, callback) {
        console.log("webpack 构建过程开始");

        callback();
    });
};
```

### 用法

由于插件可以携带参数/选项，你必须在 webpack 配置中，向 `plugins` 属性传入 `new` 实例。

根据你的 webpack 用法， 有多种方式使用插件。

### 配置

```js
// webpack.config.js

const HtmlWebpackPlugin = require('html-webpack-plugin');   // 通过 npm 安装
const webpack = require('webpack);  // 访问内置的插件
const path = require('path');

const config = {
    entry: './path/to/my/entry/file.js',
    output: {
        filename: 'my-firest-webpack.bundle.js',
        path: path.resolve(__dirname, 'dist')
    },
    module: {
        loaders: [
            {
                test: /\.(js|jsx)$/,
                use: 'babel-loader'
            }
        ]
    },
    plugins: [
        new webpack.optimize.UglifyJsPlugin(),
        new HtmlWebpackPlugin({template: './src/index.html'})
    ]
};

module.exports = config;
```
### Node API

即使使用 Node API，用户也应该在配置中传入 plugins 属性。`compiler.apply`并不是推荐的使用方式。

```js
// some-node-script.js

const webpack = require('webpack'); //访问 webpack 运行时（runtime）
const configuration = require('./webpack.config.js');

let compiler = webpack(configuration);
compiler.apply(new webpack.ProgressPlugin());

compiler.run(function(err. stats){
    // ...
});
```

## 配置（Configuration）

- 很少有 webpack 配置看起来很完全相同；
- 因为 webpack 的配置文件，是淡出一个对象的 JS 文件；
- 此对象，由 webpack 根据对象定义的属性进行解析

因为webpack配置是标准的 mode.js CommonJS 模块，你可以做到一下事情：

- 通过 `require(...)` 导入其他文件；
- 通过 `require(...)` 使用 npm 的工具函数；
- 使用 JS 控制流表达式，例如`?:`操作符；
- 对常用值使用常量或变量；
- 编写并执行函数来生成部分配置。

虽然技术上可行，但赢避免以下做法：

- 在使用 webpack命令行接口（CLI）（应该编写自己的命令行接口或使用 --env），访问命令行接口参数；
- 导出不确定的值（调用 webpack 两次应该产生同样的输出文件）；
- 编写很长的配置（应该将配置拆分为多个文件）

### 最简单的配置

```js
// webpack.config.js

var path = require('path');

module.exports = {
    entry: './foo.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'foo.bundle.js'
    }
};
```

### 多个 Target

### 使用其他配置语言

## 模块（Modules）

- 在模块化变成中，开发者将程序分解成离散功能块（discrete chunks of functionality），并称之为模块；
- 每个模块具有比完整程序更小的接触面，使得校验、调试、测试轻而易举；
- 精心编写的模块提供了可靠的抽象和封装界限，使得应用程序中每个模块都具有条例清楚的设计和明确的目的

### 什么是 webpack 模块

对比 Node.js 模块， webpack 模块能够以各种方式表达它们的依赖关系，如下：

- ES2015 `import`语句
- CommonJS `require()`语句
- AMD `define`和`require`语句
- css/sass/less 文件中的 `@import` 语句
- 样式 （url(...)）或 HTML 文件（<img src=...>）中的突变链接（image url
- ……

### 支持的模块类型

- webpack 通过 loader 可以支持各种语言和预处理器编写模块；
- loader描述了 webpack 如何处理 非JS 模块，并在 bundle中引入这些依赖；

webpack 社区已经为各种流行语言和语言处理器构建了 loader，包括：
- CoffeeScript
- TypeScript
- ESNext(Babel)
- Sass
- Less
- Stylus

总的来说，webpack提供了可定制的、强大和丰富的 API，允许任何技术栈使用 webpack，保持了在你的开发、测试和生成流程中无侵入性（non-opinionated）

## 模块解析（Module Resolution）

- resolver 是一个库（library），用于帮助找到模块的绝对路径；
- 一个模块可以作为另一个模块的依赖模块，然后被后者引用，如下：

```js
import foo from 'path/to/module'

// or
require('path/to/module')
```
- 所依赖的模块可以是来自应用程序代码或第三方的库；
- resolver帮助 webpack 找到bundle中需要引入的模块代码，这些代码包含在每个`import/require`语句中；
- 当打包模块时，webpack 使用 enhanced-resolve 来解析文件路径

### webpack 中的解析规则

使用`enhanced-resolve`，webpack 能够解析三种文件路径：

- 绝对路径
- 相对路径
- 模块路径

### 绝对路径

```js
import "/home/me/file";

import "C:\\Users\\me\\file"
```

### 相对路径

```js
import "../src/file1";
import "./file2";
```

### 模块路径

```js
import "module";
import "module/lib/file"
```

模块将在`resolve.modules`中指定的所有目录内搜索。你可以替换初始模块路径，此替换路径通过使用`resolve.alias`配置选项来创建一个别名。

一旦根据上述规则解析路径后，解析器（resolver）将检查路径是否指向文件或目录。如果路径指向一个文件：

- 如果路径具有文件扩展名，则直接将文件打包；
- 否则，将使用`[resolve.extensions]`选项作为文件扩展名来解析，此选项告诉解析器在解析中能够接受哪些扩展名

如果路径指向一个文件夹，则采取一下步骤找到具有正确扩展名的正确文件：

- 如果文件夹包含`package.json`文件，则按照顺序查找`resolve.mainFields`配置选项中指定的字段。并且`package.json`中的第一个这样的字段确定文件路径。
- 如果`package.json`文件不存在或者`package.json`文件中的 main 字段没有返回一个有效路径，则按照顺查找`resolve.mainFiles`配置选项中指定的文件名，看是否能在 import/require 目录下匹配到一个存在文件名；
- 文件扩展名通过 `resolve.extensions`选项采用类似的方法进行解析。

webpack 根据构建目标（build target）为这些选项提供了合理的默认配置、

### 解析 Loader(Resolving Loaders)

Loader 解析遵循与文件解析器指定的规则相同的规则。但是 `resolveLoader`配置选项可以用来为 Loader 提供独立的解析规则。

### 缓存

- 每个文件系统访问都被缓存，一遍更快触发对同一个文件的多个并行或串行请求。
- 在观察模式下，只有修改过的文件会从缓存中摘除；
- 如果关闭观察模式，在每次编译前清理缓存

## 依赖图（Dependency Graph）

任何时候，一个文件依赖于另一个文件，webpack 就把此视为文件之间有*依赖关系*。这使得 webpack 可以接收非代码资源(non-code asset)（例如图像或 web 字体），并且可以把它们作为*依赖*提供给你的应用程序。

webpack 从命令行或配置文件中定义的一个模块列表开始，处理你的应用程序。 从这些*入口起点*开始，webpack 递归地构建一个*依赖图*，这个依赖图包含着应用程序所需的每个模块，然后将所有这些模块打包为少量的 *bundle*- 通常只有一个 - 可由浏览器加载。

## 构建目标（Targets）

因为服务器和浏览器代码都可以用 JavaScript 编写，所以 webpack 提供了多种*构建目标(target)*，你可以在你的 webpack 配置中设置。

### 用法

要设置 `target` 属性，只需要在你的 webpack 配置中设置 target 的值

```js
// webpack.config.js

module.exports = {
    target: 'node'
};
```

在上面例子中，使用 `node` webpack 会编译为用于「类 Node.js」环境（使用 Node.js 的 `require` ，而不是使用任意内置模块（如 `fs` 或 `path`）来加载 chunk）。

每个*target*都有各种部署(deployment)/环境(environment)特定的附加项，以支持满足其需求。

### 多个 Target

- 尽管 webpack 不支持向 `target` 传入多个字符串；
- 你可以通过打包两份分离的配置来创建同构的库；

```js
// webpack.config.js

var path = require('path');
var serverConfig = {
    target: 'node',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'lib.node.js'
    }
    // ...
};

var clientConfig = {
    target: 'web', // <== 默认是'web'，可省略
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'lib.js'
    }
    // ...
};

module.exports = [serverConfig, clientConfig];
```

上面的例子将在你的`dist`文件夹下创建`lib.js`和`lib.node.js`文件。

## Mainifest

在使用 webpack构建的典型应用程序或站点中，有三种主要的代码类型：

1. 逆火你的团队编写的源码；
2. 你的源码会依赖的任何第三方的 library 或 ”vendor“代码；
3. webpack的 runtime 和 manifest ，管理所有模块的交互。

### Runtime

- runtime，以及伴随的 manifest数据，主要是指：在浏览器运行时，webpack 用来连接模块化的应用程序的所有代码；
- runtime包含：在模块交互时，连接模块所需的加载和解析逻辑。包括浏览器中的已加载模块的连接，以及懒加载模块的执行逻辑。

### Manifest

那么，一旦你的应用程序中，形如 `index.html` 文件、一些 bundle 和各种资源加载到浏览器中，会发生什么？?

你精心安排的 `/src` 目录的文件结构现在已经不存在，所以 webpack 如何管理所有模块之间的交互呢？?

这就是 manifest 数据用途的由来……

- 当编译器(compiler)开始执行、解析和映射应用程序时，它会保留所有模块的详细要点。这个数据集合称为 "Manifest"
- 当完成打包并发送到浏览器时，会在运行时通过 Manifest 来解析和加载模块。无论你选择哪种模块语法，那些 `import` 或 `require` 语句现在都已经转换为 `__webpack_require__` 方法，此方法指向模块标识符(module identifier);
- 通过使用 manifest 中的数据，runtime 将能够查询模块标识符，检索出背后对应的模块。

## 模块热替换（Hot Module Replacement）

模块热替换（HMR- Hot Module Replacement）功能会在应用程序运行过程中替换、添加或删除模块，而无需重新加载整个页面。主要通过一下集中方式加快开发速度：

- 保留在完全重新加载页面时丢失的应用程序状态；
- 只更新变更内容，以节省宝贵的开发事件；
- 调整样式更加快速- 集合相当于在浏览器调试器中更改样式。

### 