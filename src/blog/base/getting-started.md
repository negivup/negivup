# 创建项目

使用 [vitepress](https://github.com/vuejs/vitepress) 搭建一个基础的博客项目，用于记录当前编程基础知识及平时工作中遇到的问题。

这篇文章主要介绍基础的搭建过程，能够启动一个首页即可。

## 步骤 1{#step-01}

创建并进入文件夹：

```bash
$ mkdir negivup && cd negivup
```

使用常用的包管理工具初始化项目：

```bash
$ pnpm init
```

## 步骤 2{#step-02}

安装开发依赖：

```bash
$ pnpm add --save-dev vitepress vue
```

在安装依赖之前，最好在根目录下配置当前项目的 `.npmrc` 文件，可以加快拉取依赖的速度及更改 [pnpm](https://github.com/pnpm/pnpm) 的缓存目录：

```
registry=https://registry.npmmirror.com
# 缓存目录根据自己需要自定义即可
store-dir=/Users/negivup/Documents/pnpm
```

使用 [pnpm](https://github.com/pnpm/pnpm) 安装依赖时，可能会出现丢失依赖的警告，如：

```
 WARN  Issues with peer dependencies found
.
└─┬ vitepress 1.0.0-alpha.13
  └─┬ @docsearch/js 3.2.1
    └─┬ @docsearch/react 3.2.1
      └─┬ @algolia/autocomplete-preset-algolia 1.7.1
        └── ✕ missing peer @algolia/client-search@^4.9.1
```

把对应的依赖加入到 `package.json` 中即可关闭这个警告：

```json
  "pnpm": {
    "peerDependencyRules": {
      "ignoreMissing": [
        "@algolia/client-search"
      ]
    }
  }
```

## 步骤 3{#step-03}

增加配置文件 `.vitepress/config.ts` 并配置文档源码目录及打包文件所在目录：

```ts
import { defineConfig } from 'vitepress';

export default defineConfig({
  srcDir: 'src',
  outDir: 'dist'
});
```

增加首页文件 `src/index.md`：

```md
# Hello VitePress
```

## 步骤 4{#step-04}

设置脚本：

```bash
# 启动开发环境
$ npm pkg set scripts.docs:dev="vitepress dev ."
# 打包文档代码
$ npm pkg set scripts.docs:build="vitepress build ."
# 预览打包后的代码
$ npm pkg set scripts.docs:serve="vitepress serve ."
```

在终端中运行 `pnpm run docs:dev`，然后打开 [http://localhost:5173](http://localhost:5173) 查看效果。
