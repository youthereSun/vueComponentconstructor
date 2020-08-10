# Custom Components

> 自定义组件
> 基于webpack1.13.4，vue2.6.11

## 准备

* node下载并安装。下载地址：http://nodejs.cn/download/

## Warnings

* package.json 中的vue版本请根据项目中的vue版本进行升降级，其他依赖尽量不要更改，以免打包出问题(如果你熟悉webpack可以随意)




``` bash
# install dependencies
npm install/cnpm install 随意

# serve at localhost:8090
npm run dev

# build for production with minification
npm run dist
```

## 主要目录结构说明



* dist: `npm run dist` 最终生成的包存储位置

* src

   * components: 自定义组件目录
   * App.vue：测试环境程序根节点
   * index.js: `npm run dist` 生产程序入口，所有的自定义组件都在该文件中注册
```js
const install = function (Vue) {
  Vue.component(cusDiv.name, cusDiv)
  ...
}
```
   * main.js： `npm run dev` 开发环境程序入口，作为自定义组件的测试入口

## 开发环境测试

* 将自定义组件放入App.vue父组件中，然后运行 `npm run dev` ，在浏览器中输入 http://localhost:8090


## eslint

增加文件 .eslintignore
================
build/*.js
src/*
==============
==============
.eslintrc.js
module.exports = {
  "extends": "vue"
}
================


