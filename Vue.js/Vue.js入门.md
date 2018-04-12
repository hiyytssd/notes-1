# Vue.js入门

>从 Vue.js 的安装开始，一步步记录入门使用 Vue.js 的过程。

## 目录
- [安装 Vue-cli ](#安装Vue-cli)




### 安装Vue-cli


1. 使用命令行工具全局安装 Vue-cli
```
npm install -g Vue-cli
```
>这里可以使用 cnpm 来安装，即：[淘宝镜像](https://npm.taobao.org/)
    
安装完成后可以使用 `vue -V` (V 是大写的)来检查 Vue-cli 的版本号。

2. 初始化 Vue-cli

初始化之前，先创建一个目录，这个目录时用来存放 Vue-cli 初始化后的项目用的。

如果不创建一个新目录，当使用初始化命令时，Vue-cli 就会在所在目录下直接以项目名称创建一个目录


```
mkdir vue-demo
```
**可以省略创建目录这一步，直接进入需要安装 Vue-cli 的目录即可**

开始初始化
```
vue init <template-name> <project-name>
````
init：表示用 vue-cli 来初始化项目

\<template-name\>：表示模板名称，vue-cli 官方提供了5种模板

```
webpack – 一个全面的 webpack + vue-loader 的模板，功能包括热加载，linting 检测和 CSS 扩展。
webpack-simple – 一个简单 webpack + vue-loader 的模板，不包含其他功能，让你快速的搭建 vue 的开发环境。
browserify – 一个全面的 Browserify + vueify 的模板，功能包括热加载，linting 单元检测。
browserify-simple – 一个简单 Browserify + vueify 的模板，不包含其他功能，让你快速的搭建 vue 的开发环境。
simple – 一个最简单的单页应用模板。
```
\<project-name\>：标识项目名称，这个可以根据自己的项目来起名字。

输入命令后，会询问我们几个简单的选项，根据需要进行填写。

```
Project name：项目名称 ，如果不需要更改直接回车就可以了。注意：这里不能使用大写
Project description：项目描述，默认为A Vue.js project，直接回车，不用编写。
Author：作者，如果你有配置 git 的作者，他会读取。
Install  vue-router？ 是否安装 vu e的路由插件
Use ESLint to lint your code？ 是否用 ESLint 来限制你的代码错误和风格。如不需要输入 n，如果是大型团队开发，最好是进行配置。
setup unit tests with  Karma + Mocha？ 是否需要安装单元测试工具 Karma+Mocha。如不需要，所以输入 n。
Setup e2e tests with Nightwatch？ 是否安装e2e来进行用户行为模拟测试。如不需要，所以输入 n。
```

到了这里已经可以运行 Vue-cli 了，当中命令行中数输入下列代码
```
cd vue-demo01
npm run dev
```

如果一切顺利，在命令行工具里可以看到 `I  Your application is running here: http://localhost:8080` 这句话了，直接在浏览器中输入 `http://localhost:8080` 就可以看到一个 Vue 的欢迎界面了。

[vue欢迎界面](../images/vue.png)



