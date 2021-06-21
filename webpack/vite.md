# 1.vite的基本使用

vite依赖`ESBuild`构建速度非常快

局部安装vite

> npm i vite -D

运行vite构建工具的项目

> npx vite

## 2.vite对css的处理

vite默认对css支持，不要使用相关的loader

vite对less的处理需要安装`less`，但是内部相关的配置都是设置好的

> npm i less

vite对`postcss`设置已经配置好

> npm i postcss postcss-preset-env -D

# 3.vite对ts的支持

可以直接使用`mul.ts`

```ts
export default function mul(num1: number, num2: number): number {
  return num1 * num2
}
```

`vite`对ts和less的支持都是将代码转换成相应的es6代码，直接传给浏览器。

# 4.vite对vue的支持

> npm install vue@next

> npm install @vue/compiler-sfc -D

> npm install @vitejs/plugin-vue

# 5.vite的打包

> npx vite build

对打包的文件进行预览

> npx vite preview

# 6.script配置

```json
"scripts": {
    "serve": "vite",
    "build": "vite build",
    "previre": "vite preview"
},
```

# 7.vite的脚手架

全局安装脚手架：

> npm install @vitejs/create-app -g

创建项目：

> create 项目名称

官网推荐方式：

```
npm init @vitejs/app
```

你还可以通过附加的命令行选项直接指定项目名称和你想要使用的模板。例如，要构建一个 Vite + Vue 项目，运行:

```
# npm 6.x
npm init @vitejs/app my-vue-app --template vue

# npm 7+, 需要额外的双横线：
npm init @vitejs/app my-vue-app -- --template vue

# yarn
yarn create @vitejs/app my-vue-app --template vue
```