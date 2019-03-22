# webpack理解与使用

- [webpack理解与使用](#webpack%E7%90%86%E8%A7%A3%E4%B8%8E%E4%BD%BF%E7%94%A8)
  - [官方参考](#%E5%AE%98%E6%96%B9%E5%8F%82%E8%80%83)
  - [webpack是什么](#webpack%E6%98%AF%E4%BB%80%E4%B9%88)
  - [使用准备](#%E4%BD%BF%E7%94%A8%E5%87%86%E5%A4%87)
  - [基本使用](#%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
  - [webpack配置](#webpack%E9%85%8D%E7%BD%AE)
  - [预处理文件](#%E9%A2%84%E5%A4%84%E7%90%86%E6%96%87%E4%BB%B6)
  - [plugin插件](#plugin%E6%8F%92%E4%BB%B6)
  - [vue引入](#vue%E5%BC%95%E5%85%A5)

## 官方参考

webpack官方网址 <https://webpack.js.org>
</br>
webpack官方中文网址 <https://webpack.docschina.org>

## webpack是什么

webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。
</br>
webpack的作用有处理依赖、模块化、打包、压缩混淆、图片转base64等

## 使用准备

- node 中文网址: <https://nodejs.org/zh-cn>
</br>
    下载完之后安装，安装完之后在cmd面板输入 node -v 出现版本号说明安装成功，输入 npm -v 出现版本号说明自带的npm也安装成功。
- webpack
- webpack-cli(webpack 4.0以上需要)
- webpack-dev-server(静态资源的服务器) 主要是启动了一个使用express的Http服务器

不建议全局安装webpack、webpack-dev-server、webpack-cli

```bash
mkdir webpack-demo && cd webpack-demo
npm init -y
npm install --save-dev webpack webpack-cli webpack-dev-server
```

## 基本使用

创建以下目录结构、文件和内容：

项目目录下

```bash
    webpack-demo
    |- package.json
+   |- index.html
+   |- /src
+       |- index.js
```

src/index.js

```bash
function component() {
  var element = document.createElement('div');
  element.innerHTML = "Hello world";
  return element;
}
document.body.appendChild(component());
```

index.html

```bash
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>webpack</title>
</head>
<body>
    <script src="./dist/main.js"></script>
</body>
</html>
```

webpack npx命令进行打包编译：

```bash
npx webpack
```

命令行中提示无报错, 即编译成功

```bash
C:\work\work_all_demo\webpack-demo>npx webpack
Hash: ee4adc43a7dc9f5802f8
Version: webpack 4.29.6
Time: 318ms
Built at: 2019-03-21 23:29:19
  Asset      Size  Chunks             Chunk Names
main.js  70.3 KiB       0  [emitted]  main
Entrypoint main = main.js
[1] ./src/index.js 361 bytes {0} [built]
[2] (webpack)/buildin/global.js 472 bytes {0} [built]
[3] (webpack)/buildin/module.js 497 bytes {0} [built]
    + 1 hidden module

WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/concepts/mode/
```

package scripts命令打包编译:

package.json 的 scripts 中加入指令

```bash
"scripts": {
    "dev": "webpack-dev-server --open --hot",
    "build": "webpack --progress --hide-modules"
},
```

```bash
npm run build //打包编译
npm run dev // 启动静态资源服务器，开启热更新
```

## webpack配置

在 webpack 4 中，可以无须任何配置使用，然而大多数项目会需要很复杂的设置，这就是为什么 webpack 仍然要支持 配置文件。
</br>
配置完整文档网址<https://www.webpackjs.com/configuration/>

在项目根目录下新建webpack.config.js

```bash
const path = require('path');

module.exports = {
  // 入口
  entry: './src/index.js',
  // 输出
  output: {
    filename: 'bundle.js',
    // 注意整个配置中我们使用 Node 内置的 path 模块，并在它前面加上 __dirname这个全局变量。可以防止不同操作系统之间的文件路径问题，并且可以使相对路径按照预期工作。
    path: path.resolve(__dirname, 'dist')
  }
};
```

## 预处理文件

你可以使用 loader 告诉 webpack 加载 CSS 文件，或者将 LESS 转为 JavaScript。为此，首先安装相对应的 loader：

```bash
npm install --save-dev css-loader
npm install --save-dev less-loader
```

然后指示 webpack 对每个 .css 使用 css-loader，以及对所有 .less 文件使用 less-loader：
</br>
webpack.config.js

```bash
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' },
      { test: /\.less$/, loader: 'less-loader'}
    ]
  }
};
```

## plugin插件

在配置中添加插件
</br>
webpack.config.js

```bash
//...
  plugins: [
    new webpack.DefinePlugin({
      // Definitions...
    })
  ]
```

## vue引入

添加vue依赖

    npm install vue

webpack.config.js

```bash
module.exports = {
  // ...
  resolve: {
    alias: {
      // 'vue$': 'vue/dist/vue.common.js' webpack 1 的写法
      'vue$': 'vue/dist/vue.esm.js'
    }
  }
}
```
