---
title: 移动端适配
published: 2022-09-15
description: '深入解析移动端适配方案，探索不同屏幕尺寸与分辨率下的最佳实践，实现高效、流畅的用户体验。'
image: ''
tags: [移动端]
category: '移动端'
draft: false 
---

# 1、viewport视口

### 1.1、viewpoint视口是指**显示页面的区域**

**viewport/视口基础**

- ①pc端浏览器可视区域等于视口大小
- ②移动端视口大于浏览器可视区

**移动端有三种类型的 viewport: layoutviewport、visualviewport、idealviewport**

- layoutviewport：大于实际屏幕
- visualviewport：浏览器可视区域宽度
- idealviewport：固定不变

### 2.1、viewport设置

在index的head标签上修改代码

```js
<meta name='viewport' content='width=device-width,initial-scale=1,user-scale=no' />
```

> - width：设置的是 layoutviewport 的宽度
> - initial-scale：设置页面的初始缩放值
> - user-scalable：是否允许用户进行缩放的设置

# 2、移动端适配方案

### 2.1、设备像素比dpr和1px物理像素

- 物理像素：屏幕最小显示单元

### 2.2、设备独立像素

- 也叫逻辑像素，也就是我们在CSS中设置的px

### 2.3、设备像素比dpr

- 设备像素比(dpr) = 物理像素/设备独立像素  图中dpr=2就是用4个物理像素显示一个CSS像素(虚拟像素/逻辑像素)

<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/12/10/1679612a0e09ccb3~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp" alt="屏幕快照 2018-10-22 下午4.25.50.png | left | 173x165" style="zoom:33%;" />

### 2.4、1px的物理像素的解决方案

从第一部分的讨论可知 viewport 的 initial-scale 具有缩放页面的效果。对于 dpr=2 的屏幕，1px压缩一半便可与1px的设备像素比匹配，这就可以通过将缩放比 initial-scale 设置为 0.5=1/2 而实现。以此类推 dpr=3的屏幕可以将 initial-scale设置为 0.33=1/3 来实现。

### 3.1、什么是rem

rem  rem是 css3 的一个长度单位 ，相对文档跟元素 html；比如设置html font-size=100px;那么1rem=100px;之后的所有元素都可以用这个基准值来设置大小；

### 3.2、移动端rem适配

- [postcss-pxtorem](https://github.com/cuth/postcss-pxtorem) 是一款 postcss 插件，用于将 px 单位转化为 rem

- [lib-flexible](https://github.com/amfe/lib-flexible) 用于设置 rem 基准值

  > 插件介绍
  >
  > lib-flexible: 动态修改根节点的字体带下，默认为根节点字体大小为屏幕宽度的十分之一
  >
  > postcss-pxtorem:  自动把项目写的全部px单位转换为rem，

### 3.3、 使用 lib-flexible动态设置 REM 基准值(html 标签的字体大小)

- ①安装依赖

```js
# yarn add amfe-flexible
npm i amfe-flexible
```

- ②在 main.js中加载执行该模块：

```js
import 'amfe-flexible'
```

### 3.4、使用 postcss-pxtorem将 px 转为 rem

- ①安装依赖

```js
# yarn add -D postcss-pxtorem
# -D 是 --save-dev 的简写 开发环境下安装
npm install postcss-pxtorem -D：
```

- ②在项目根目录中创建 postcss.config.js 文件：

> **1、rootValue** 应该如何设置呢？
>
> 如果你使用的是基于 lib-flexable 的 REM 适配方案，则应该设置为你的设计稿的十分之一。
>
> 例如设计稿是 750 宽，则应该设置为 75。
>
> 大多数设计稿的原型都是以 iPhone 6 为原型，iPhone 6 设备的宽是 750。
>
> **2、下面我们以vant组件库为例子来设置rootValue**
>
> Vant 建议设置为 37.5，为什么呢？
>
> 因为 Vant 是基于逻辑像素 375 写的，所以如果你设置为 75 的话，Vant 的样式就小了一半。
>
> 所以如果设置为 37.5 的话，Vant 的样式是没有问题的，但是我们在测量设计稿的时候都必须除2才能使用，否则就会变得很大。
>
> 有没有更好的办法不用除以2呢？当然有了，这里给大家介绍两种方式，一种不用写代码，一种需要写代码。

```js
/* 
  PostCSS 的配置文件
  PostCSS 是基于 Node.js 运行的一个处理CSS的工具
  所以它的配置文件也是运行在Node.js中的
*/
// PostCSS 配置文件需要导出一个对象
module.exports = {
  // 需要配置的相关插件
  plugins: {
    // 自动添加浏览器厂商声明前缀,用来兼容不同的浏览器
    // 因为Vue-CLI是通过项目中的.browserslistrc 文件来配置要兼容的环境信息的 所以我们这里并不需要
     autoprefixer: {
       browsers: ['Android >= 4.0', 'iOS >= 8']
     },
    // 把px转为rem
    'postcss-pxtorem': {
      // 转换的根元素的基准值
      // 正常去情下按照你的设计稿来
      // 750 宽的设计稿， 750 / 10 = 75
      // 370 宽的设计稿， 370 / 10 = 37.5
      rootValue: 37.5,//
      // 需要转换的CSS属性，*就是所有属性都要转换
      propList: ['*']
    }
  }

```

