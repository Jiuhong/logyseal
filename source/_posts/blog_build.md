---
title: 博客搭建
date: 2023-02-06 16:15:52
toc: true
tags: [hexo, netlify]
categories:
- [Web, 个人博客]
description: 记录是文明发展的动力，笔记是个人进步的支柱，写博客能够使你的所学所识得以发酵、沉淀、传播，这不仅是个人的收益，也是全世界的收益。
cover: https://www.585dg.com/wp-content/uploads/2022/09/113025S2Lqo.jpg
---
# 概述

本站点整个技术框架主要是由 `hexo` 网页生成框架、`github` 代码托管平台、`netlify` 网页托管平台三部分组成。

# 环境准备

- 安装 `nodejs` 、`npm`

```bash
sudo apt install nodejs	# ubuntu 安装版本默认为 node V10.
sudo apt install npm # 安装 npm
node -v # 查看 node 版本
npm -v # 查看 npm 版本
```

- 更新 `npm` 包管理工具的安装源

```bash
npm config get registry # 查看原来的源
npm config set registry https://registry.npm.taobao.org # 修改为淘宝源
npm config get registry # 查看修改后的源
```

- 安装 `hexo` 博客框架

```bash
sudo npm install hexo-cli -g # 全局安装 hexo 命令行工具
```

# `Hexo` 框架命令 

- 初始化博客目录

```bash
mkdir myblog # 创建博客目录
cd myblog # 进入博客目录
hexo init # 初始化博客目录并安装相关依赖
```

- 构建博客

```bash
hexo new post "test" # 会在 source/_posts/ 目录下生成文件 ‘test.md’，打开编辑
hexo generate        # 生成静态HTML文件到 /public 文件夹中
hexo server          # 本地运行server服务预览，打开 http://localhost:4000 即可预览你的博客
```

> 问题一：

在执行 `hexo generate` 时会报下列错误：

```tex
INFO  Validating config
FATAL 
TypeError: Object.fromEntries is not a function
    at exists.then.filter.then.modules (/home/minjiuhong/workspace/sealblog/sealblog/node_modules/hexo/lib/hexo/load_plugins.js:43:19)
    at tryCatcher (/home/minjiuhong/workspace/sealblog/sealblog/node_modules/bluebird/js/release/util.js:16:23)
    at Promise._settlePromiseFromHandler (/home/minjiuhong/workspace/sealblog/sealblog/node_modules/bluebird/js/release/promise.js:547:31)
    at Promise._settlePromise (/home/minjiuhong/workspace/sealblog/sealblog/node_modules/bluebird/js/release/promise.js:604:18)
    at Promise._settlePromise0 (/home/minjiuhong/workspace/sealblog/sealblog/node_modules/bluebird/js/release/promise.js:649:10)
    at Promise._settlePromises (/home/minjiuhong/workspace/sealblog/sealblog/node_modules/bluebird/js/release/promise.js:729:18)
    at Promise._fulfill (/home/minjiuhong/workspace/sealblog/sealblog/node_modules/bluebird/js/release/promise.js:673:18)
    at MappingPromiseArray.PromiseArray._resolve (/home/minjiuhong/workspace/sealblog/sealblog/node_modules/bluebird/js/release/promise_array.js:127:19)
    at MappingPromiseArray.init (/home/minjiuhong/workspace/sealblog/sealblog/node_modules/bluebird/js/release/promise_array.js:75:18)
    at Promise._settlePromise (/home/minjiuhong/workspace/sealblog/sealblog/node_modules/bluebird/js/release/promise.js:601:21)
    at Promise._settlePromise0 (/home/minjiuhong/workspace/sealblog/sealblog/node_modules/bluebird/js/release/promise.js:649:10)
    at Promise._settlePromises (/home/minjiuhong/workspace/sealblog/sealblog/node_modules/bluebird/js/release/promise.js:729:18)
    at _drainQueueStep (/home/minjiuhong/workspace/sealblog/sealblog/node_modules/bluebird/js/release/async.js:93:12)
    at _drainQueue (/home/minjiuhong/workspace/sealblog/sealblog/node_modules/bluebird/js/release/async.js:86:9)
    at Async._drainQueues (/home/minjiuhong/workspace/sealblog/sealblog/node_modules/bluebird/js/release/async.js:102:5)
    at Immediate.Async.drainQueues [as _onImmediate] (/home/minjiuhong/workspace/sealblog/sealblog/node_modules/bluebird/js/release/async.js:15:14)
    at runCallback (timers.js:705:18)
    at tryOnImmediate (timers.js:676:5)
    at processImmediate (timers.js:658:5)
```

> 分析：

经搜索相关资料可知，最新版本的 `hexo` 需要依赖的 `nodejs` 版本最低为 `V14`

> 解决方法：

需要将 `nodejs` 的版本进行升级

```bash
sudo npm install -g n
sudo n stable  # latest    #(升级node.js到最新版) stable #（升级node.js到最新稳定版）
node -v
```

- 添加建站脚本

为了后续 `netlify` 建站方便，我们可以在 `package.json` 里面添加一个命令：

```json
{
    // ......
    "scripts": {
        "build": "hexo generate",
        "clean": "hexo clean",
        "deploy": "hexo deploy",
        "server": "hexo server",
        "netlify": "npm run clean && npm run build" // 这一行为新加
    },
    // ......
}
```

# `Github` 项目文件托管

- 推送新项目

```bash
git init
git add .
git commit -m "my blog first commit"
git remote add origin "远端github仓库地址"
git branch -M main
git push -u origin main
```

# `Netlify` 网页托管

这部分可参考以下博客：

- [博客搭建](https://blog.cuijiacai.com/blog-building/) 

# `Butterfly` 主题使用

这部分可参考以下博客：

- [butterfly教程](https://butterfly.js.org/)
