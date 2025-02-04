---
title: "我的第一篇文章"
date: 2025-02-04
draft: false
---

# Hugo + Github Pages 个人博客网站
## 1.环境准备
```console
# 安装hugo
brew install hugo
```
# 2.创建 Hugo 项目
## 创建站点
```console
hugo new site my-blog
```
## 添加主题（以 PaperMod 为例）：
https://github.com/adityatelange/hugo-PaperMod.git  
```console
cd my-blog
git init
git submodule add https://gitee.com/lzhuii/hugo-PaperMod.git themes/PaperMod
echo "theme = 'PaperMod'" >> hugo.toml
```
## 配置hugo.toml
```toml
baseURL = "https://<你的GitHub用户名>.github.io/"
languageCode = "zh-cn"
title = "我的博客"
theme = "PaperMod"

[params]
  author = "你的名字"
  description = "这是我的技术博客"

```

# 3.添加新文章
```console
hugo new posts/Hugo_Github_Blog.md
```

# 4.本地预览
启动本地服务器：

```console
hugo server -D
```
打开浏览器访问 http://localhost:1313，查看效果。

# 5. 部署到 GitHub Pages
创建 GitHub 仓库
在 GitHub 上创建一个新仓库，命名为 <你的GitHub用户名>.github.io（例如：zhangsan.github.io）。

将本地项目关联到远程仓库：

```console
git remote add origin https://github.com/<你的GitHub用户名>/<你的GitHub用户名>.github.io.git
```

# 6.生成静态文件
```console
hugo
```
生成的静态文件会保存在 public 目录中。
```console
cd public
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/<你的GitHub用户名>/<你的GitHub用户名>.github.io.git
git push -u origin main
```

# 7.启用 GitHub Pages
进入 GitHub 仓库，点击 Settings → Pages。

在 Source 中选择 main 分支，点击 Save。

稍等片刻，访问 https://<你的GitHub用户名>.github.io，即可看到你的博客。

# 8.自动化部署（可选）
使用 GitHub Actions 实现自动构建和部署：

在项目根目录创建 `.github/workflows/deploy.yml`:

```yaml
name: Deploy Hugo Site

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: true

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```
每次推送后，GitHub Actions 会自动构建并部署博客。

# 9.绑定自定义域名（可选）
1. 在域名注册商处添加 CNAME 记录，指向 <你的GitHub用户名>.github.io。

2. 在 public 目录下创建 CNAME 文件，内容为你的域名（例如：blog.example.com）。

3. 推送更改：

```console
cd public
echo "blog.example.com" > CNAME
git add CNAME
git commit -m "Add custom domain"
git push origin main
``` 
在 GitHub Pages 设置中，填写自定义域名并启用 HTTPS。

# 10.完成
现在，你的 Hugo 博客已经成功部署到 GitHub Pages，并可以通过自定义域名访问！接下来可以继续优化内容、添加功能和主题定制。