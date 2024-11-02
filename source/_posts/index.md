---
title: 使用Hexo - hexo-theme-redefine搭建自己的博客
---
# Hexo

## 安装和使用Hexo

```bash
npm install -g hexo-cli
hexo init <folder>
cd <folder>
npm install
```
## Hexo目录

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

## 主题安装极其使用

```bash
npm install hexo-theme-redefine@latest
```
### 启用配置

```yml
# _config.yml
theme: redefine
```

### 在 Hexo 根目录下创建 _config.redefine.yml 文件。
并将[此处](https://github.com/EvanNotFound/hexo-theme-redefine/blob/main/_config.yml)的所有内容复制进去


## 部署到github上

- 配置文件
```yml
# .github/workflow/pages.yml
name: Pages

on:
  push:
    branches:
      - main # default branch

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          # If your repository depends on submodule, please see: https://github.com/actions/checkout
          submodules: recursive
      - name: Use Node.js 18
        uses: actions/setup-node@v4
        with:
          # Examples: 20, 18.19, >=16.20.2, lts/Iron, lts/Hydrogen, *, latest, current, node
          # Ref: https://github.com/actions/setup-node#supported-version-syntax
          node-version: "20"
      - name: Cache NPM dependencies
        uses: actions/cache@v4
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public
  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

## 问题
`记得更改配置文件中的url,否则静态资源请求不过来404`








