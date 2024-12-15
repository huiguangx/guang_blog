---
title: Packages.json基本字段
published: 2022-07-01
description: Packages.json基本字段
tags: [Pakcages.json]
category: akcages.json
draft: true
---

基本字段
1. name
描述: 项目的名称。
类型: string
例子:
json
复制代码
"name": "my-project"
2. version
描述: 项目的版本号，通常遵循 semver 规范。
类型: string
例子:
json
复制代码
"version": "1.0.0"
3. description
描述: 项目的简短描述。
类型: string
例子:
json
复制代码
"description": "A simple project for learning."
4. main
描述: 项目的入口文件。Node.js 或其他工具通常会加载这个文件。
类型: string
例子:
json
复制代码
"main": "dist/index.js"
5. scripts
描述: 包含一组命令，可以通过 npm run <script-name> 或 yarn <script-name> 执行。
类型: object，键为脚本名称，值为命令。
例子:
json
复制代码
"scripts": {
  "start": "node dist/index.js",
  "build": "webpack --config webpack.config.js",
  "test": "jest"
}
6. keywords
描述: 项目相关的关键字，有助于搜索引擎和包管理器中发现项目。
类型: array of string
例子:
json
复制代码
"keywords": ["typescript", "node", "example"]
7. author
描述: 项目的作者，通常是一个对象，包含姓名、电子邮件等信息。
类型: string 或 object
例子:
json
复制代码
"author": "John Doe <john.doe@example.com>"
8. license
描述: 项目的许可证类型，通常是 MIT、Apache-2.0 等。
类型: string
例子:
json
复制代码
"license": "MIT"
9. repository
描述: 项目的仓库地址，通常用于开源项目。
类型: object 或 string
例子:
json
复制代码
"repository": {
  "type": "git",
  "url": "git+https://github.com/username/project.git"
}
10. bugs
描述: 项目的问题反馈链接。
类型: object
例子:
json
复制代码
"bugs": {
  "url": "https://github.com/username/project/issues"
}
11. homepage
描述: 项目的主页地址。
类型: string
例子:
json
复制代码
"homepage": "https://github.com/username/project#readme"
依赖相关字段
12. dependencies
描述: 项目运行时依赖的包。
类型: object
例子:
json
复制代码
"dependencies": {
  "lodash": "^4.17.21",
  "axios": "^0.21.1"
}
13. devDependencies
描述: 项目开发时依赖的包。
类型: object
例子:
json
复制代码
"devDependencies": {
  "typescript": "^4.3.5",
  "webpack": "^5.38.1"
}
14. peerDependencies
描述: 项目对其他包的兼容要求，通常用于库/插件开发时，指定你需要依赖的外部库的版本范围。
类型: object
例子:
json
复制代码
"peerDependencies": {
  "react": "^17.0.0",
  "redux": "^4.0.0"
}
15. optionalDependencies
描述: 可选依赖项，如果安装失败不会导致整个安装失败。
类型: object
例子:
json
复制代码
"optionalDependencies": {
  "fsevents": "^2.3.2"
}
16. engines
描述: 定义项目支持的 Node.js 和 npm/yarn 版本。
类型: object
例子:
json
复制代码
"engines": {
  "node": ">=14.0.0",
  "npm": "^6.0.0"
}
17. browserslist
描述: 用来指定支持的浏览器版本，常用于构建工具（如 Babel、Autoprefixer）来生成兼容的代码。
类型: array 或 object
例子:
json
复制代码
"browserslist": [
  "> 1%",
  "last 2 versions",
  "not ie <= 8"
]
其他字段
18. private
描述: 如果为 true，则项目不会被发布到 npm。
类型: boolean
例子:
json
复制代码
"private": true
19. config
描述: 可以用于定义一些自定义的配置，供脚本使用。
类型: object
例子:
json
复制代码
"config": {
  "port": "8080",
  "env": "development"
}
20. workspaces
描述: 用于 Monorepo 项目，定义了项目中的多个子包。
类型: array
例子:
json
复制代码
"workspaces": [
  "packages/*"
]
21. types
描述: 用于指定 TypeScript 类型声明文件的路径。
类型: string
例子:
json
复制代码
"types": "dist/index.d.ts"
22. main
描述: 这个字段指向项目的主入口文件，通常是一个 JavaScript 文件。
类型: string
例子:
json
复制代码
"main": "index.js"
23. module
描述: 指定 ES6 模块的入口文件，通常用于指定适用于 ES Module 的文件（*.mjs）。
类型: string
例子:
json
复制代码
"module": "index.mjs"
24. files
描述: 定义需要包含在发布包中的文件列表。
类型: array of string
例子:
json
复制代码
"files": [
  "dist/",
  "src/",
  "README.md"
]
25. bin
描述: 指定可执行文件的位置（例如 CLI 工具）。
类型: object
例子:
json
复制代码
"bin": {
  "mycli": "./bin/mycli.js"
}
26. scripts
描述: 定义一组可执行的命令，通常是通过 npm run <script-name> 或 yarn <script-name> 执行。
类型: object，键为脚本名称，值为脚本命令。
例子:
json
复制代码
"scripts": {
  "start": "node dist/index.js",
  "build": "webpack --config webpack.config.js",
  "test": "jest"
}

