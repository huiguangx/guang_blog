---
title: fontend-interview
published: 2023-09-01
description: 'Read more about frontend interview knowledge in guang_blog'
image: ''
tags: [interview, frontend]
category: 'interview'
draft: false 
---
前端面试题

> 在首页你可以对空间进行说明，比如包含以下内容。

# 一、HTML

1. ### async和defer的区别

Async: 异步执行，defer: 延迟执行

两个都是异步加载脚本，加载过程均不影响页面解析，async加载完成会马上执行脚本，执行过程是无序的，阻碍页面解析；defer则会等待页面解析完毕后按顺序执行。

1. ### document.documentElement.clientWidth与window.screen.width、window.innerWidth的区别

document.documentElement.clientWidth表示视口宽度不包括滚动条，可见区域，随页面缩放而改变，反应可视区域的css像素大小

window.innerWidth同上，但包括滚动条

window.screen.width表示设备屏幕宽度，不会因为缩放网页而改变

涉及移动端适配：

布局视口：网页布局区域（980px），比移动设备屏幕宽，会导致滚动条出现

理想视口：不需要缩放完美呈现内容的视口大小

视觉视口：用户实际看到的内容区域。会根据缩放操作变化

# 二、 CSS

# 三、 JavaScript

1. 事件循环

先执行主线程的同步代码，遇到异步任务会将其推到任务队列中，同步代码执行完毕后，事件循环会去检测微任务队列，取出所有微任务放入主线程执行，然后再到宏任务队列取出下一个任务执行，这个过程不断循环，形成事件循环

宏任务：setTimeOut,setInterval,setImmediate,io,ui渲染

微任务：promise.then,nextTick,mutationObserver

1. 

# 四、 Vue2

1. ## v-if和v-show的区别

两者都是用来控制元素的显隐，v-if是创建和销毁dom，而v-show控制的是css的display属性，v-if的切换会触发组件的生命周期，有更高的消耗。

1. ## Vue实例挂载的过程

1. ## Vue的生命周期

`created`是在组件实例一旦创建完成的时候立刻调用，`mounted`是在页面`dom`节点渲染完毕之后就立刻执行的，请求应当放在created后，放在mounted的话，因为dom已经渲染了，所以会有可能导致页面闪动。

1. ## v-if和v-for的优先级

vue2中v-for优先级更高，先循环后条件判断，会带来不必要的性能消耗，解决方法是外层嵌套template，加一层先判断后内部循环

1. ## 为什么data是一个函数而不是对象

使用函数返回一个全新的对象，如果使用对象，则组件实例都会引用同一个对象，导致数据污染。根实例是单例不会产生数据污染

1. ## 组件化、插件

组件化其实就是将视图结构和页面逻辑抽取出来进行开发的模式。降低系统耦合度，提高可重用性和可维护性。

插件是用来为vue添加全局功能，可添加全局方法和属性，全局资源，vue-router，vue指令

1. ## Vue通信方式

组件数据共享：父子、兄弟、祖孙、非关系型组件间通信

props，$emit，ref，EventBus，$parent，$root，attrs，listeners，provide，inject，Vuex

1. ## mixin和hooks的区别

都是对状态和逻辑的抽象，mixin会有属性方法的同名覆盖问题以及追溯困难问题；hooks追溯清晰，同时它从hooks方法获取，可以用别名获取属性方法，不存在同名覆盖问题，同时实现了状态逻辑高内聚。

1. ## history和hash模式的区别

为什么使用history和hash？单页面应用中只有一个html，所以我们既要实现url变化，又不能触发页面请求重载。

hash模式是监听hashchange方法，实现路由组件匹配

history模式是通过pushState来重写url，匹配路由组件，但不重载页面，如果刷新页面则请求服务器，但文件并不存在404，开发模式下脚手架会抹平这个bug，但是生产环境需要nginx进行转发规则重写

# 五、Vue3

# 六、webpack

## webpack的运行过程

Webpack的运行过程可以概括为以下几个主要步骤：

1. **初始化参数**：Webpack从配置文件（通常是`webpack.config.js`）和命令行参数中读取并合并所有参数，形成最终的配置对象。
2. **开始编译**：使用上一步得到的配置对象初始化一个`Compiler`对象，加载所有配置的插件，并执行`Compiler`对象的`run`方法，正式开始编译过程。
3. **确定入口**：根据配置文件中的`entry`属性，确定入口文件。Webpack会从这些入口文件开始，构建内部依赖图。
4. **编译模块**：
   1. **加载模块**：从入口文件开始，递归地加载每个模块。
   2. **使用Loader**：对每个模块使用配置的`loader`进行转换处理。Loader可以将模块从不同的语言转换为JavaScript，或者将内联图像转换为Base64编码等。
   3. **解析依赖**：在转换过程中，Webpack会解析每个模块所依赖的其他模块，并将这些依赖加入到待处理的模块列表中。
5. **完成模块编译**：在所有模块都经过Loader处理之后，Webpack会得到每个模块被编译后的最终内容以及它们之间的依赖关系。
6. **输出资源**：
   1. **组装Chunk**：根据入口和模块之间的依赖关系，Webpack会将模块组装成一个个的`chunk`。
   2. **生成文件**：将每个`chunk`转换成单独的文件，并将这些文件加入到输出列表中。在这个过程中，可以使用插件对输出内容进行最后修改。
7. **输出完成**：在确定输出内容后，Webpack根据配置确定输出的路径和文件名，将文件内容写入到文件系统中。
8. **广播事件**：在整个编译过程中，Webpack会在特定的时间点广播特定的事件，插件可以监听这些事件，并在合适的时候介入编译过程，进行自定义操作。

简而言之，Webpack的运行过程是一个串行的过程，它读取配置，从入口出发，递归加载模块，对模块使用相应的loader进行转换，加载过程会分析依赖，根据依赖图将模块组装成chunk，并最终将chunk转换为文件输出到指定的目录中，同时在整个过程中，插件可以通过监听事件来改变Webpack的默认行为。

## 常用的loader

Webpack中的Loader是用来将模块的源代码转换为新的格式，这些转换后的代码可用于打包到最终的bundle中。以下是Webpack中一些常用的Loader及其用途：

1. **`style-loader`**：将CSS代码添加到style标签。
2. **`css-loader`**：解析CSS文件中的`@import`和`url()`，并将CSS代码转换为JavaScript模块。
3. **`less-loader`** / **`sass-loader`**：分别将Less或Sass代码转换为CSS。通常与`css-loader`和`style-loader`配合使用。
4. **`babel-loader`**：使用Babel将ES6+代码转换为向下兼容的JavaScript代码，以便在当前和旧版浏览器中运行。
5. **`ts-loader`** / **`awesome-typescript-loader`**：将TypeScript代码转换为JavaScript。
6. **`file-loader`**：将文件（如图片、字体）发送到输出目录，并返回文件的URL。
7. **`url-loader`**：与`file-loader`类似，但可以将小于指定大小的文件转换为Base64编码的URL。

## 常用的plugin

Webpack的插件（Plugins）是用来拓展Webpack功能的工具，它们在Webpack的生命周期中的特定时刻被调用，来执行相应操作，以影响打包结果。以下是Webpack中一些常用的插件及其用途：

1. **`HtmlWebpackPlugin`**：生成一个HTML文件，用于承载打包后的JavaScript和CSS文件。这个插件经常用于创建一个简单的HTML入口文件，它会自动注入所有生成的bundle。
2. **`CleanWebpackPlugin`**：在每次构建前清理构建目录，以确保构建结果是最新的。
3. **`CopyWebpackPlugin`**：将单个文件或整个目录复制到构建目录中，通常用于处理不需要Webpack处理的静态资源。
4. **`SplitChunksPlugin`**：用于将公共的依赖模块提取到单独的chunk中，可以用于代码分割和缓存共享。

第三方库单独提取，并行加载

## webpack热更新是如何实现的？

Webpack的热更新（Hot Module Replacement, HMR）是Webpack的一个功能，它允许在不完全重新加载页面的情况下通过替换更新的模块来实现实时预览更新。以下是热更新的原理和实现步骤：

小问题：增量编译还是全量编译？module、chunk、bundle是啥

### 原理：

1. **WebSocket连接**：Webpack通过`webpack-dev-server`启动一个本地开发服务器，该服务器与客户端建立一个WebSocket连接，用于实时通信。
2. **监听文件变更**：Webpack在watch模式下运行，监听源代码文件的变更。一旦检测到文件变化，Webpack将重新编译变更的模块。
3. **编译与更新**：编译完成后，Webpack将更新的模块通知`webpack-dev-server`，`webpack-dev-server`通过WebSocket将更新的模块的详细信息（如模块的ID和新的哈希值）发送给客户端。
4. **客户端接收更新**：客户端接收到更新通知后，请求新的模块代码。然后，客户端Webpack通过JSONP请求获取到新的模块代码。
5. **模块替换**：客户端Webpack用新的模块替换掉旧的模块。这是通过调用模块的`module.hot.accept`方法实现的，如果模块支持HMR，则会替换，否则可能需要手动刷新。
6. **状态保留**：因为页面没有进行完全的重载，所以页面状态可以得以保留，提高了开发效率。

> 开启dev-server,服务端和客户端建立websocket连接，webpack监听文件变化，重新编译，并通知客户端进行更新，客户端通过jsonp请求获取新的模块代码，将旧模块替换为新模块

# 七、浏览器

1. 强缓存和协商缓存

强缓存和协商缓存是http控制缓存的两种方式。

当浏览器请求资源时，服务器会返回资源并携带缓存相关的http头信息，cache-control和expires，浏览器下一次发起请求，会先检查强缓存，命中缓存则使用缓存（http1.0使用expires，通过绝对时间来判断过期时间，http1.1使用cache-control，通过时长来判断过期时间）

【no-cache 直接用协商缓存，no-store 直接不用缓存】

当强缓存失效的时候，浏览器会在请求头携带last-Modified或Etag向服务器发起请求，服务器会判断资源是否更新，更新则直接返回新资源和200ok状态码，如果没有更新，则返回304，让浏览器使用缓存 （last-Modified记录的是时间点，Etag记录的是文件哈希，last-Modified缺点是编辑文件，但内容没有改变，则会导致缓存失效了，而且它是秒级的，一秒内多次使用则并不体现出被修改）

1. 浏览器本地存储

Cookie、WebStorage、IndexedDB、WebStorage分为localStorage和SeesionStorage

区别：

存储大小：cookie只有4KB，webstorage达到5M

存储有效期不同：cookie可以设置过期时间，localstorage存储持久数据，需要手动删除，sessionstorage仅在当前会话有效

作用域：cookie、localstorage适用于所有同源窗口，sessionstorage仅在当前浏览器窗口中有效，cookie会自动发送到浏览器，其他两个存储在本地

1. 输入url

网络链路：构建请求，查找强缓存，dns解析，建立tcp连接，发送http请求，网络响应。

渲染链路：

1. 

# 八、版本控制

> 这里你可以介绍知识空间的主要内容和面向的成员群体。

# 九、手写题

> 这里你可以将需要关注的文档链接粘贴在此处，方便成员快速访问。

1. ### Object.create

```JavaScript
function create(obj) {
    function F() { };
    F.prototype = obj;
    return new F();
}
//测试
let obj = {a: 1 , b: 2};
let obj1 = create(obj);
console.log(obj1.__proto__);//{ a: 1, b: 2 }
//思路 ： 传入一个对象 作为返回对象的原型
//目的 ： 为一个对象指定原型
```

1. ### instanceof

```JavaScript
function myinstanceof(obj, Fn) {
    let objProto = Object.getPrototypeOf(obj);//获取对象obj的原型
    let fProto = Fn.prototype;//获取构造函数的原型
    while (true) {
        if (!objProto) return false;//如果遍历到原型对象为null 则表示找不到
        if (objProto === fProto) return true;//当前对象原型===构造函数原型 则表示找到
        objProto = Object.getPrototypeOf(objProto);//获取对象原型的原型，循环向上寻找
    }
}
//测试
function Fn() {
}
let obj = new Fn();
console.log(myinstanceof(obj, Fn));//true
```

1. ### new 操作符

```JavaScript
function _new(Fn, ...args) {
    let obj = {};//1、创建一个空对象
    obj.__proto__ = Fn.prototype;//2、实例对象的原型指向构造函数Fn的prototype对象
    let res = Fn.call(obj, ...args);//3、构造函数Fn绑定this指向obj实例，并调用构造函数 返回对象res
    return typeof res === 'object' ? res : obj;
}
function Person(name, age) {
    this.name = name;
    this.age = age;
    // return {
    //     name: name,
    //     age: age
    // }
}
//测试
let personImpl = _new(Person, 'guang', '28');
console.log(personImpl);
```

1. ### Promise.all

```JavaScript
function myPromiseAll(promises) {//接收一个Promise的可迭代对象Array，Map，Set
    return new Promise((resolve, reject) => {//返回一个Promise实例
        if (!Array.isArray(promises)) {
            return new TypeError('arguments must be an array');//判断是否为一个数组 不是则抛出类型异常
        }
        let count = 0;//设置计数器 计算被解决的期约Promise个数
        let result = [];//定义一个数组保存所有被解决的期约Promise
        let n = promises.length;//获取可迭代对象的长度（算是一种小优化吧）
        for (let i = 0; i < n; i++) {//遍历所有参数，并发执行每一个promise
            Promise.resolve(promises[i]).then(res => {
                result[i] = res;//将每一个promise的resolve回调结果丢进result数组
                count++;//每循环一次计数器加一
                if (count === promises.length) {//计数器等于数组长度 则返回结果数组result
                    resolve(result);
                }
            }).catch(err => reject(err));//catch不仅可以捕获reject时的状态 同时能够捕获then回调中的错误
        }
    })
}
// 测试
let p1 = Promise.resolve(1);
let p2 = Promise.resolve(2);
let p3 = Promise.resolve(3);
myPromiseAll([p3, p1, p2]).then(res => {
    console.log(res) // [3, 1, 2]
})
Promise.all([p3, p1, p2]).then(res => {
    console.log(res);
})
```

1. ### Promise.race

```JavaScript
Promise.race = function (args) {//传入可迭代对象args
    return new Promise((resolve, reject) => {//返回一个promise实例（包装期约）
        for (let i = 0, len = args.length; i < len; i++) {
            args[i].then(resolve, reject)// Promise.then()函数最多接收两个参数，分别进入兑现和拒绝状态时执行，执行某一个就直接返回当前状态的期约
        }
    })
}
// 测试
let p1 = Promise.resolve(1);
let p2 = Promise.resolve(2);
let p3 = Promise.resolve(3);
Promise.race([p3, p1, p2]).then(res => {
    console.log(res);// [3, 1, 2]
})
```

1. ### 防抖

```JavaScript
function debounce(fn, time) {//fn为回调函数(需要防抖的函数)，time是自定义间隔时间 debounce是对需要防抖的函数的封装
    let timer = null;
    return function () {
        clearTimeout(timer);//debounce执行时需要把之前的setTimeout计时器清除再重新计时
        let args = arguments;//不同函数可能会有不同的参数传入
        timer = setTimeout(() => {
            fn.apply(this, args);//需要apply绑定this并调用传进来的原函数 ？？疑问：为什么要绑定this
        }, time)
    }
}
let addBtn = document.getElementById('add')
function addOne() {
    console.log('增加一个')
}
addBtn.addEventListener('click', debounce(addOne))//添加click点击事件，对addOne实现防抖
```

1. ### 函数柯里化

```JavaScript
function toCurry(fn, args) {
    var length = fn.length;
    var args = args || [];
    return function(){
        newArgs = args.concat(Array.prototype.slice.call(arguments));
        if (newArgs.length < length) {
            return toCurry.call(this,fn,newArgs);
        }else{
            return fn.apply(this,newArgs);
        }
    }
}
// function toCurry(fn, ...args) {
//     if (args.length >= fn.length) {
//       return fn(...args);
//     } else {
//       return (...args2) => toCurry(fn, ...args, ...args2);
//     }
//   }

function multiFn(a, b, c) {
    return a * b * c;
}

var multi = toCurry(multiFn);

console.log(multi(2)(3)(4));
console.log(multi(2,3,4));
console.log(multi(2)(3,4));
console.log(multi(2,3)(4));
```

1. ### 浅拷贝

```JavaScript
let number1 = 1;
let number2 = 2;
console.log(number1, number2); // 1 2
number2 = number1;
number2 = 3
console.log(number1, number2); // 1 3
// 复制基本数据类型，系统会自动为新的变量在栈内存中分配一个新值,旧值不会被改变

let obj1 = {
    name: 'guang',
};
let obj2 = obj1;
console.log(obj1); // { name: 'guang' }
console.log(obj2); // { name: 'guang' }
obj2.name = 'me';
console.log(obj1); // { name: 'me' }
console.log(obj2); // { name: 'me' }
// 复制引用数据类型，并进行修改，会改变原本的值

// 浅拷贝的实现;

function shallowCopy(obj) {
    if (!obj || typeof obj !== "object") return;// 空对象和非对象类型 直接返回
    let newObj = Array.isArray(obj) ? [] : {};// 根据 obj 的类型判断是新建一个数组还是对象
    //for...in语句以任意顺序迭代对象的可枚举属性(包括它的原型链上的可枚举属性)。----for...of 语句遍历可迭代对象定义要迭代的数据。
    for (let key in obj) {// 遍历 obj，并且判断是 obj 自身的属性才拷贝
        if (obj.hasOwnProperty(key)) {
            newObj[key] = obj[key];
        }
    }
    return newObj;
}

//测试
const obj3 = {
    name: 'guang',
    like: ['eat', 'play'],
}
const arr1 = [1, 2, ['guang', 'JavaScript'], 4];
const obj4 = shallowCopy(obj3);
console.log(obj4);//{ name: 'guang', like: [ 'eat', 'play' ] }
console.log(obj3);//{ name: 'guang', like: [ 'eat', 'play' ] }
obj4.name = 'me';
console.log(obj4);//{ name: 'guang', like: [ 'eat', 'play' ] }
console.log(obj3);//{ name: 'guang', like: [ 'eat', 'play' ] }
```

1. ### 深拷贝

```JavaScript
function deepClone(obj, hash = new WeakMap()) {
  if (obj === null) return obj; // 如果是null或者undefined我就不进行拷贝操作
  if (obj instanceof Date) return new Date(obj);
  if (obj instanceof RegExp) return new RegExp(obj);
  // 可能是对象或者普通的值  如果是函数的话是不需要深拷贝
  if (typeof obj !== "object") return obj;
  // 是对象的话就要进行深拷贝
  if (hash.get(obj)) return hash.get(obj);
  let cloneObj = new obj.constructor();
  // 找到的是所属类原型上的constructor,而原型上的 constructor指向的是当前类本身
  hash.set(obj, cloneObj);
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      // 实现一个递归拷贝
      cloneObj[key] = deepClone(obj[key], hash);
    }
  }
  return cloneObj;
}
const obj = {
  a: 1,
  b: {
    c: 1,
    d: {
      d: 5,
    },
  },
};
const a = deepClone(obj);
obj.b.d = 66666;
console.log(a);
```

1. ### 数组扁平化

```JavaScript
// 方法一： 递归实现
let arr = [1, [2, [3, 4, 5]]];
function flatten(arr) {
    let res = [];// 定义一个数组存储结果
    for (let i = 0; i < arr.length; i++) {//遍历数组
        if (Array.isArray(arr[i])) {// 如果当前数组元素是一个数组 则进入递归调用 扁平化
            res = res.concat(flatten(arr[i]));// 每一层扁平化都会返回一个数组 和上一层的数组res做一个拼接 
        } else {
            res.push(arr[i]);// 非数组则直接添加到res中
        }
    }
    return res;
}
console.log(flatten(arr)); //  [1, 2, 3, 4，5]

// 方法二：ES6中的flat方法 flat()方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回
// function flatten(arr) {
//     return arr.flat(Infinity);// 数组的嵌套层数不确定，最好直接使用 Infinity
// }

// 方法三：reduce函数迭代 reduce归并方法 传入一个函数 该函数可以接收4个参数，迭代数组所有项并构建一个最终返回值
//1：上一个归并值, 2：当前项 3：当前项索引 4：数组本身 一般只用到前面两个
// 第一个参数为数组的第一项 第二个参数为数组的第二项 函数返回的任何值都会作为下一次调用同一个函数的第一个参数
// function flatten(arr) {
//     return arr.reduce((pre, next) => {
//         return pre.concat(Array.isArray(next) ? flatten(next) : next)
//     }, [])// 第二个参数[]空数组作为初始值pre
// }
```

1. ### 数组去重

```JavaScript
// 方法一：使用Set集合
let arr = [1, 2, 3, 5, 1, 5, 9, 1, 2, 8];
// Array.from() 方法对一个类似数组或可迭代对象创建一个新的，浅拷贝的数组实例。
let res = Array.from(new Set(arr));
console.log(res);// [ 1, 2, 3, 5, 9, 8 ]

// 方法二：for循环 
function removeDuplicates(arr) {
    let map = {};// 定义个map对象,使用map存储不重复的数字
    let res = [];// 定义数组保存结果
    for (let i = 0; i < arr.length; i++) {// 遍历数组
        if (!map.hasOwnProperty([arr[i]])) {// 判断map对象自身是否有arr[i]属性
            map[arr[i]] = 1;// 没有的话 就把arr[i]作为属性 1为值添加到对象中
            res.push(arr[i]);// 因为没有map中没有arr[i] 所以arr[i]不是重复项 则添加进res数组中
        }
    }
    return res;// 返回res
}

let a = removeDuplicates(arr);
console.log(a);
```

1. ### 发布订阅

```JavaScript
// 用ES6的方式来写
class EventSubPub {
    constructor() {
        this.events = {};// 定义一个事件对象，存储订阅事件和回调函数(方法)
        // 如： {click : [handler1, handler2, ...], sleep : [handler1, handler2]...} 每一个事件有各种方法,形成方法数组
    }
    // 订阅事件/注册事件以及事件的回调函数
    on(eventName, handler) {// 参数为参数名和回调函数(就是触发该事件会执行某些方法)，这些方法可以是多个
        if (!this.events[eventName]) {// 如果本来没有注册事件
            this.events[eventName] = [handler]// 则创建数组并添加第一个回调函数
        } else {
            this.events[eventName].push(handler);// 如果本来注册过事件，则存入接下来的回调函数
            // this.event[eventName] 这是一个数组 是上面所创建的
        }
    }
    // 发布事件/触发事件回调 参数：事件名和事件回调函数参数
    emit(eventName, args) {// 需要传入事件名和参数
        if (!this.events[eventName]) {// 事件未注册 直接抛出错误
            return new Error('该事件未注册')
        }
        // 遍历对应事件里数组中的方法(回调函数) 并 执行所有回调函数(需要传入参数)
        this.events[eventName].forEach(handler => { handler(args) })
    }
    // 事件移除 参数 事件名和回调函数
    off(eventName, handler) {
        if(this.events[eventName]) {// 判断是否注册过该事件
            if(!handler) {// 判断是否传入了事件对应的回调函数
              delete this.events[eventName];// 没有传入直接删除事件
            } else {
                let index = this.events[eventName].indexOf(handler);// 在数组中找到第一个出现该回调函数handler的下标
                this.events[eventName].splice(index, 1);// 
                // slice 可以传入两个参数 若只传入一个 则返回该索引到数组末尾所有元素，若传入两个(a,b) 则切割索引a到b 不包括b的数组元素(左闭右开)
                // splice可以传入三个参数，第一个为开始位置，第二个为删除个数，第三个(还可以任意多个)为插入元素
            }
        }
    }
}

const events = new EventSubPub()
function fn1() {
    console.log("fn1");
}
function fn2() {
    console.log("fn2");
}
function fn3() {
    console.log("fn3 ");
}
events.on("event1", fn1)
events.on("event1", fn2)
events.on("event2", fn2)
events.on("event3", fn3)// 注册事件和回调函数
events.emit("event1")// fn1 fn2
events.off("event1", fn1);// 关闭订阅 移除事件及对应的回调函数
events.emit("event1")// fn2
events.emit("event2")// fn2
events.emit("event3")// fn3
events.off("event2")// 移除整个事件event2
events.off("event3")// 移除整个事件event3
events.emit("event2")// 无
events.emit("event3")// 无
```

1. ### 观察者模式

```JavaScript
// Subject 被观察对象
function Subject() {
    this.observers = [];// 创建数组用于存储观察者
}
Subject.prototype = {// 在被观察者的原型上添加方法
    add(observer) {  // 添加观察者
        this.observers.push(observer);
    },
    notify() {  // 添加通知方法
        var observers = this.observers;// 获取所用添加进来的观察者
        for (var i = 0; i < observers.length; i++) {// 遍历调用观察者的更新方法 通知观察者们更新
            observers[i].update();
        }
    },
    remove(observer) {  // 删除观察者
        var observers = this.observers;
        for (var i = 0; i < observers.length; i++) {// 遍历所有观察者
            if (observers[i] === observer) {// 如果当前观察者和传进来的一样
                observers.splice(i, 1);// 则删除传进来的观察者
            }
        }
    },
}
// Observer 观察者对象
function Observer(name) {
    this.name = name;
}
Observer.prototype = {// 在观察者的原型上添加更新方法
    update() {  // 更新
        console.log('my name is ' + this.name);
    }
}
var sub = new Subject();
var obs1 = new Observer('guang1');
var obs2 = new Observer('guang2');
sub.add(obs1);
sub.add(obs2);
// 通知所有观察者
sub.notify();  //my name is guang1 my name is guang2
```

# 10、算法leetcode hot 100

### [1.两数相加](https://leetcode.cn/problems/add-two-numbers/)

给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 **一位** 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

**输入：**l1 = [2,4,3], l2 = [5,6,4] **输出：**[7,0,8] **解释：**342 + 465 = 807.

```JavaScript
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     val: number
 *     next: ListNode | null
 *     constructor(val?: number, next?: ListNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.next = (next===undefined ? null : next)
 *     }
 * }
 */

function addTwoNumbers(l1: ListNode | null, l2: ListNode | null): ListNode | null {
    let dummy = new ListNode() // 创建虚拟节点
    let curry = 0 // 进位
    let cur = dummy // 获取当前节点
    while(l1 || l2) { // 两个链表随便一个非空则执行循环
        let val1 = l1 ? l1.val : 0 // 获取当前l1节点值
        let val2 = l2 ? l2.val : 0 // 获取当前l2节点值
        let sum = val1 + val2 + curry // 节点值相加
        cur.next = new ListNode(sum % 10) // cur.next 赋值为当前相加值
        cur = cur.next // 指针移到下一位
        curry = Math.floor(sum / 10) // 记录进位
        if(l1) { // l1向后遍历
            l1 = l1.next
        }
        l2 && (l2 = l2.next) // l2向后遍历
    }
    if(curry > 0){ // 遍历到最后若存在进位 则将进位添加到最后
        cur.next = new ListNode(curry)
    }
    return dummy.next
};
```

### [2. 两数相加](https://leetcode.cn/problems/add-two-numbers/)

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`*  的那 **两个** 整数，并返回它们的数组下标。

**示例 1：**

**输入：**nums = [2,7,11,15], target = 9 **输出：**[0,1] **解释：**因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

```JavaScript
function twoSum(nums: number[], target: number): number[] {
    let map = new Map() // map记录当前值的补数
    for(let i = 0; i < nums.length; i++) {
        if(map.get(nums[i]) !== undefined) { // 判断map中是否存有当前值
            return [i, map.get(nums[i])]
        }
        map.set(target - nums[i], i) // 当前值的补数作为key,下标作为value, 记录
    }
};
```

# 11、TypeScript

[先用着掘金链接](https://juejin.cn/post/7321542773076082699?searchId=2024080312411681D58834CD0A057298D4)

# 12、计算机网络

1. Get和post区别

Get url有长度限制、get安全性比post低、get有幂等性、get参数在url上post附加在请求体上

 post会发送两次请求，POST 请求发送 OPTIONS 请求是为了确保跨域请求的安全性，它是一种预检请求，用于询问服务器是否允许该请求。

1. http2

二进制分帧：将文本切分成帧，高效传输

多路复用：一个连接并行发送多个请求（http1有keep-alive实现一个连接发送多个请求，但是必须按顺序）

头部压缩：hpack算法压缩，减少数据体积

服务器推送：

1. Https

非对称加密+对称加密：前端发请求，后端返回证书，前端拿到证书，验证证书的合法性，拿到服务器的公钥，生成一个对称密钥，使用公钥对对称密钥进行加密，传给后台，后台拿到后，用私钥解密出对称密钥，将此对称密钥作为加密密钥进行加密传输。

1. http的状态

200：成功；301：永久重定向；304：走协商缓存；400：请求语法有问题； 403：拒绝访问；404：找不到资源；500； 服务器错误；502、504：网关错误，一个是服务器问题，一个是时间过长

1. 输入url到看到页面

- 网络层面：域名解析、建立tcp连接（三次握手）、发送http请求、服务器响应、浏览器解析html和渲染页面

1. 浏览器缓存优先级

Service worker缓存

Memory cache内存缓存

Http 缓存：强缓存（expires和cache-control）、协商缓存(last-modified和Etag)

硬盘缓存

推送缓存

1. 跨域请求

原因：浏览器同源策略的限制，协议、主机、端口

跨域方法：jsonp，只支持get请求；cors，确定是否允许跨域

代理服务器：通过代理服务器进行转发，前端请求其实发送到同源的服务器上，由代理服务器去请求不同源的资源

window.postmessage

documnet.domian

1. 浏览器渲染机制
2. 回流和重绘

回流必定引起重绘

回流：改变布局的行为，元素的几何信息，添加和删除dom

重绘：不改变布局，例如只改变元素背景色或者文字颜色

减少回流：css3动画和过渡（gpu加速）、

1. 浏览器事件循环和node事件循环的区别

1. 进程和线程

进程是资源分配的基本单位，而线程是CPU调度的基本单位。进程拥有独立的地址空间和资源，而线程共享进程的资源。在并发性和系统开销方面，线程较进程具有更高的效率和更低的开销。

1. 浏览器的渲染进程包括哪些线程

gui渲染线程

js引擎线程

定时器线程

http网络线程

事件触发线程

## 13、web安全

sql注入：

xss（跨站脚本攻击）：网页注入脚本，盗取用户信息

csrf（跨站请求伪造）：盗用用户身份，伪装请求

1. 浏览器存储数据的方式

cookie：用来做身份认证，存储大小只有4k，每次请求同一域名都会带上cookie

localstorage：永久保存数据，需要主动删除，大小几乎由5m

sessionstorage：只在当前会话下有效，关闭页面则删除

1. 