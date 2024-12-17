---
title: 前端构建工具
published: 2023-03-01
description: '前端工程化的碎碎念，温故而知新'
image: ''
tags: [前端工程化]
category: '前端工程化'
draft: false 
---

## 1. 前端工程化

### 1.1 webpack

#### 1.1.2 webpack配置字段

#### 1.1.3 loader 加载器 

laoder主要将代码转换为浏览器能识别的原生js代码，本质就是个函数转换器

##### 常见的loader

Babel-loader: 将es6语法转换为es5语法

Css-loader: 解析css 文件，处理依赖关系，@import url()，生成commons模块代码

Style-loader:  通过style标签将css插入html

File-loader:  解析文件路径，将文件输出到目录

Url-loader:  和上面差不多👆，但是它可以将指定大小的文件转成base64的data URL格式，减少网络请求，浏览器会自动解码

Sass-loader:  将sass转换为css

less- loader： 将less转换为css

Postcss-loader:   css后处理器，添加css浏览器前缀等

Vue-loader:  将vue 单文件转化为 js 代码

#### 1.1.4 plugin 插件

插件是用来增强webpack的功能的，监听webpack生命周期广播的事件，然后执行响应操作

##### 常见的plugin

HtmlWebpackPlugin：生成HTML文件，并自动将打包后的javaScript和CSS文件引入到HTML文件中。

CleanWebpackPlugin：清除输出目录。

ExtractTextWebpackPlugin：将CSS代码提取到单独的CSS文件中。

DefinePlugin：定义全局变量。

UglifyJsWebpackPlugin：压缩JavaScript代码。

HotModuleReplacementPlugin：热模块替换，用于在开发环境下实现热更新。

MiniCssExtractPlugin：与ExtractTextWebpackPlugin类似，将CSS代码提取到单独的CSS文件中。

BundleAnalyzerPlugin：分析打包后的文件大小和依赖关系。

#### 1.1.5 webpack的构建流程

初始化参数：合并shell和webpack.config.js的配置参数

开始编译： 初始化comliler对象，注册插件，插件监听事件

确定入口：递归遍历解析文件，构建依赖图

编译模块：使用loader进行代码转换，完成编译得到最终结构和依赖关系

输出资源： 将模块组装成chunk，再将chunk转换为文件输出

#### 1.1.6 webpack的热更新

不刷新页面直接新代码替换旧代码

开启devServer，通过websocket和浏览器建立连接，当本地资源发生变化，webpack会将新打包的模块代码推送到浏览器，同时会附带构建时的hash。

#### 1.1.7 code splitting 代码分割

将一个大的chunk分割成多个小chunk，实现按需加载。optimization.splitChunks配置项来开启代码分割

#### 1.1.8 sourcemap

源码和构建后的代码的映射关系，主要用于开发阶段调试

Devtool 设置 sourcemap

#### 1.1.9 treeshaking

同样是通过import和export，在分析代码的时候，未引入的和未使用的依赖不会打包，需要进行配置

#### 1.1.10 提高webpack的打包速度

使用缓存

treeShaking

多进程多线程

预先打包好第三方库

#### 1.1.11 减少打包后的体积

code splitting：

tree shaking：

压缩代码： terser、br、gzip

Cdn: 静态资源包cdn加速

#### 1.1.12 process.env 项目环境变量

由webpack + dotenv + webpack DefinePlugin形成的工具链

Dotenv 加载 .env 文件，添加到process.env属性上，如果env有与系统环境变量重复的则跳过。

问题1: dotenv在node环境运行，但是我们的代码在浏览器运行，那么环境变量上如何注入的，很明显是在打包的时候进行了静态注入。DefinePlugin内置插件在编译时将代码中的process.env相关变量替换为其他值

### 1.2 vite

#### 1.2.1 vite为什么比webpakc快

Vite比Webpack快的主要原因是它在开发环境下直接使用浏览器原生的ES模块加载，能做到按需动态编译，而不是像Webpack那样进行复杂的打包和构建。这使得Vite在开发过程中加载速度更快，提供了更流畅的开发体验。

1-一个按需编译，一个全量打包

#### 1.2.2 为什么可以直接加载

它通过HTTP服务器提供开发服务，并利用浏览器的ESM支持实现快速的模块加载，通过http请求直接获取文件。缓存优化，将模块放到内存中。

#### 1.2.3 什么是esbuild预构建

预构建依赖，无需实现编译

#### 1.2.4 热更新的处理

得益于go语言，编译速度比js的loader快，还有就是vite能更细粒度的进行热更新，vite改动一个模块，浏览器只会重新请求该模块，而webpack则会重新将与该模块有依赖关系的模块重新编译一下。

热更新原理，文件变动会重新编译，通过web socket 将文件hash传给浏览器，浏览器会进行模块对比，然后发起jsonp请求获取，此时的js在内存中





