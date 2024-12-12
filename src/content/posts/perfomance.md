---
title: 前端性能优化
published: 2023-12-21
tags: [性能优化]
category: 性能优化
draft: false
---

# 前端性能监控

## 前端性能优化与异常监控

本文你能学到关于性能监控的知识如下：

- 性能优化
- 性能优化指标
- 性能指标的计算方式

### 性能指标

- FP（首屏渲染）
- FCP（首次内容渲染）
- FMP（）
- FCP（最大内容渲染）
- CLS（布局偏移 Mutation Observer）
- TTI （交互时间）

### 异常监控

1. 异常：

- 内存溢出
- 白屏
  - MutationObserver
- 资源加载异常
- js执行异常
- 异步处理异常
  - Promise、unhandledrejection
- 网络请求异常
  - Ajax
  - Fetch

### 性能优化

- 打包构建
- 代码
  - 虚拟列表
  - 懒加载
  - 异步加载
  - tree shaking （rollup、webpack、vite、respack）
- 网络
  - http2
  - cdn
  - Prefetch、preload

### 用户行为埋点

SDK埋点

- 埋点类型
  - 手动埋点
  - 自动埋点
  - 可视化埋点

### 性能指标计算-performance

1. 页面加载时间

```js
function pageLoadTime(){
	const time = performance.timing.loadEventEnd - performance.timing.navigationStat
	console.log('pageLoadTime time', time)
}
pageLoadTime()
```

2. 最大内容渲染

```js
function lcp() {
  new PerformanceObserver((entryList) => {
    const entries = entryList.getEntries();
    const lastTime = entries[entries.length - 1];
    const lcpTime = lastTime.startTime;
    console.log("lcp time", lcpTime);
  }).observe({ type: "largest-contentful-paint", buffered: true });
}
lcp();
```

3. 交互时间

```js
function tti() {
  const timeToInteractive =
    performance.timing.domInteractive - performance.timing.navigationStart;
  console.log("tti time", timeToInteractive);
}
tti();
```

### 异常捕获

1. 资源加载异常

```js
window.addEventListener("error", (e) => {
  console.log("source error", e);
});
```

2. js异常

```js
window.onerror = function (e) {
  console.log("js error", e);
};
```

3. promise错误

```js
window.addEventListener("unhandledrejection", (e: PromiseRejectionEvent) => {
  e.preventDefault();

  e.promise.catch((error) => {
    // 区分 promise 的两种错误消息
    let msg = error?.message || error;
    console.log("unhandledrejection", msg);
  });
});
```

### 数据上报

1. Img 基于img.src (解决跨域)
2. fetch
3. Navigator.sendBeacon

## 性能监控sdk开发

使用 `tsup` 来构建 SDK 的基础配置框架非常简洁，因为 `tsup` 是一个快速的 TypeScript 打包工具，具有零配置的优点。你可以通过最简单的配置来实现开发和打包的功能。

### 1. 安装依赖

首先，确保你已经安装了 `tsup` 和 TypeScript。如果还没有安装，可以通过以下命令安装：

```
npm install --save-dev tsup typescript
```

或者使用 `yarn`：

```
yarn add --dev tsup typescript
```

### 2. 项目结构

假设你的 SDK 项目结构如下：

```
my-sdk/
├── src/
│   └── index.ts
├── package.json
├── tsconfig.json
└── tsup.config.ts
```

- `src/index.ts`：SDK 的主入口文件。
- `package.json`：项目的基本配置。
- `tsconfig.json`：TypeScript 配置。
- `tsup.config.ts`：`tsup` 配置文件。

### 3. 编写 SDK 代码（`src/index.ts`）

首先，在 `src/index.ts` 中编写你的 SDK 的基础功能：

```js
// src/index.ts
export const greet = (name: string) => {
  return `Hello, ${name}!`;
};
export const add = (a: number, b: number) => {
  return a + b;
};
```

### 4. TypeScript 配置（`tsconfig.json`）

确保你的 TypeScript 配置文件正确，支持编译并生成打包文件。以下是一个常见的配置：

```json
{
  "compilerOptions": {
    "target": "esnext",          // 输出的 ECMAScript 目标
    "module": "esnext",          // 使用 ES 模块
    "moduleResolution": "node",  // Node.js 模块解析策略
    "esModuleInterop": true,     // 兼容 CommonJS 和 ES 模块
    "declaration": true,         // 生成类型声明文件 (.d.ts)
    "outDir": "./dist",          // 输出目录
    "strict": true,              // 启用严格模式
    "skipLibCheck": true,        // 跳过库文件类型检查
    "resolveJsonModule": true    // 支持导入 JSON 文件
  },
  "include": ["src/**/*.ts"],
  "exclude": ["node_modules"]
}
```

### 5. 配置 `tsup`（`tsup.config.ts`）

接下来，创建 `tsup.config.ts` 配置文件。`tsup` 是一个零配置的打包工具，但你可以通过配置文件进一步定制构建选项。以下是一个基础的配置示例：

```json
// tsup.config.ts
import { defineConfig } from 'tsup';

export default defineConfig({
  entry: ['src/index.ts'],     // 入口文件
  format: ['cjs', 'esm'],      // 输出 CommonJS 和 ESM 格式
  target: 'esnext',            // 目标 ECMAScript 版本
  sourcemap: true,             // 生成源码映射
  splitting: true,             // 开启代码拆分（用于多入口）
  dts: true,                   // 生成类型声明文件 (.d.ts)
  clean: true,                 // 构建前清理 dist 目录
  minify: true,                // 压缩输出文件
});
```

### 6. 更新 `package.json` 的 `scripts`

接下来，在 `package.json` 中添加构建脚本来运行 `tsup`。以下是常见的脚本配置：

```json
json
{
  "name": "my-sdk",
  "version": "1.0.0",
  "main": "dist/index.cjs.js",  // CommonJS 格式入口
  "module": "dist/index.esm.js", // ESM 格式入口
  "types": "dist/index.d.ts",    // 类型声明文件
  "scripts": {
    "build": "tsup",
    "dev": "tsup --watch"       // 开发时监听文件变动
  },
  "devDependencies": {
    "tsup": "^6.0.0",
    "typescript": "^5.0.0"
  }
}
```

### 7. 运行构建

现在，运行以下命令来构建 SDK：

```js
npm run build
```

或者在开发模式下监听文件变化：

```js
npm run dev
```

### 8. 输出结果

构建完成后，`dist` 目录下会生成以下文件：

```js
dist/
├── index.cjs.js      // CommonJS 格式的打包文件
├── index.esm.js      // ES Module 格式的打包文件
├── index.d.ts        // TypeScript 类型声明文件
├── index.js.map      // 源码映射文件（如果启用了 sourcemap）
```

### 9. 发布到 npm

如果一切配置正常，你可以将 SDK 发布到 npm。

首先确保你已经登录到 npm：

```
npm login
```

然后运行：

```
npm publish
```

### 总结

这就是使用 `tsup` 打包 TypeScript SDK 的基础配置。通过这个配置，你可以实现以下功能：

- TypeScript 编译。
- 生成不同模块格式（CommonJS 和 ES Module）。
- 生成类型声明文件。
- 开发模式支持自动重载。
