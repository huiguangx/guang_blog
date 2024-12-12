---
title: 前端规范化
published: 2023-09-01
tags: [规范]
category: 前端规范
draft: false
---

# 前端规范化

项目规范化： 确保团队在项目开发过程中遵循统一的技术、风格、结构以及代码质量标准。包括但不限于代码风格、文件命名、目录结构、提交规范等多个方面。目的是为了确保项目可维护、一致性、可读性，提高团队开发效率。

细化：

- 文件目录结构、文件命名
- 代码规范、变量命名、函数、风格： Eslint + prettier + stylelint
- 提交规范： commitLint

本文仅介绍 2 、3

eslint原理： 基于编译原理，操作代码、解析代码（Babel 、 swc、esbuild），eslint parser 转化代码以后再对代码进行操作（规范、格式化）

### js 规范化

eslint ：用于对语言进行质量检测

### eslint 规范约定

- 规范等级
  - off
  - warn
  - error
- 核心规则
  - 语法检查：变量名是否使用、console
  - 风格一致性：分号、空格、缩进、换行
  - 最佳实践：原型链污染、eval
- 大量规范来控制代码编写
- Eslint + prettier 的集成
- CI/CD 通过调用脚本来实现自动化触发：`pnpm lint`

## ESlint 9深入实践

1. cjs和js的扩展名 module.exports = {}和export default {}
2. Package.json中加入type字段的作用
3. 9的配置写法改成了数组形式：新增js推荐规范工具集 @eslint/js 

​		相当于 Airbnb、Google、Standard、antfu 规则集

4. 项目引入ts，ts的相关校验eslint自带的espree解析器无法识别

​		则需要我们用ts解析器来解析

- @typescript-eslint/parser、typescript

​		languageOptions语言字段配置{ parser: } 

- Typescript-eslint 提供的套件

5. vue项目需要vue文件解析器

​	   配置外层指定解析器vue-parser ，内层ts-parser以及语法解析配置

​	   vue-eslint-parser 或者使用 eslint-plugin-vue 套件

## eslint的架构

eslint的核心引擎：负责整个ESlint的初始化、配置加载、文件解析和规则应用

解析器：ESlint通过解析器将源代码转换为抽象语法树AST，便于规则引擎进行静态分析

规则（rules）：规则系统主要负责根据AST节点应用特定校验逻辑

插件：通过插件机制，开发者可以引入额外的规则和功能，使ESlint支持更多的编程语言和框架。

配置：ESlint 允许开发者配置不同文件、规则和插件的组合，提高代码校验的灵活性

playground训练场：astexplorer.net

### eslint规则插件实践

规则插件的本质是一个对象，根据插件化协议，约定规则

```js
{
	meta: {} // 插件元信息
	create(context) { // 访问者模式 访问AST节点，进行处理
		return {
			Identifier	
		}
	} // 插件入口 能拿到上下问
}
```

文件结构：

-eslint

​	-plugins 

​		-eslint-plgin-me.js // 到处插件对象

```js
import { noMeVars } from './no-me-vars'
export const eslintMePlugin = {
	rules: {
		"no-me-vars": noMeVars, // 规则文件导入的具体实现
	}		
}
```

​	-rules

​		-no-me-vars.js 文件名字即为规则名

```js
{
	meta: {
    messages: {
      noMeVars: '不允许使用me'
    }
  } // 插件元信息
	create(context) { // 访问者模式 访问AST节点，进行处理
		return {
      // 访问者模式 访问AST节点，进行处理
			Identifier(node) {
        if(node.nmae === 'me') {
					context.report({
            node,
            messagId: "nomevars",// 对应元信息的messages  noMeVars: '不允许使用me'
            data: {
              name: node.name
            }
          })
        }
      }
		}
	} // 插件入口 能拿到上下问
}
```

插件消费

```js
// 插件导入
plugins: {
	nome: '导入的规则插件'
}
rules: {
	"nome/插件名称": 'error'
}
```

