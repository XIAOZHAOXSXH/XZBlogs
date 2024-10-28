---
title: 使用Github和Cloudflare无服务器部署Hexo博客
tags: 技术分享
abbrlink: 33457
date: 2024-10-28 14:33:01
---

## 目录
1. **前期准备**
2. **Hexo部分**
   - 安装 Hexo CLI
   - 安装 Hexo
   - 安装主题
     - 3.1 安装远程主题
     - 3.2 本地安装主题
     - 3.3 安装 npm 依赖
     - 3.4 修改 `_config.yml`
     - 3.5 编辑 Hexo 根目录下的 `.gitignore` 文件
   - 创建文章
   - 创建列表分类
   - 文章中增加分类和 tag 标签
   - 生成和预览
3. **Github部分**
   - 生成 SSH 密钥对
   - 配置 Git 用户名和邮箱
   - 推送到 GitHub
   - 自动处理行尾符
4. **Cloudflare部分**
   - 在 Cloudflare 上部署 Hexo

---

## 前期准备

- 注册 Github: [https://www.github.com](https://www.github.com)
- 注册 Cloudflare: [https://www.cloudflare.com](https://www.cloudflare.com)
- Git 下载: [https://git-scm.com/download](https://git-scm.com/download)
- NodeJs 下载: [https://nodejs.org/zh-cn](https://nodejs.org/zh-cn)

---

## Hexo部分

### 1. 安装 Hexo CLI

需要全局安装 Hexo CLI 工具:

```bash
npm install -g hexo-cli
```

### 2. 安装 Hexo

```bash
# cd 到需要 Hexo 安装的盘符或文件夹，我这里安装到了 D 盘。
cd d:/

# 在盘下载 Hexo 并创建 blog 文件夹，blog 名称随意修改，在这里只是为了好记。
hexo init blog
```

### 3. 安装主题

#### 3.1 安装远程主题

创建 `.gitmodules` 文件，将以下代码复制到 `.gitmodules` 中并进行修改。远程主题不可控，无法进行详细修改，可使用 Fork 将主题仓库复制到你自己的 Github 中。

```bash
[submodule "themes/maupassant"]
    path = themes/maupassant
    url = https://github.com/tufu9441/maupassant-hexo.git
```

#### 3.2 本地安装主题

```bash
# cd 到 blog 文件夹中
cd blog

# 注意结尾的 themes/maupassant，表示默认将主题安装到 hexo 的 themes 目录下
git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant

# 安装 hexo-renderer-pug 插件
npm install hexo-renderer-pug --save

# 安装 hexo-renderer-sass 插件
npm install hexo-renderer-sass --save

# 开启 RSS
npm install hexo-generator-feed --save
```

#### 3.3 安装 npm 依赖

某些主题依赖 npm：

```bash
npm install
```

#### 3.4 修改 `_config.yml`

使用任意编辑器修改 Hexo 根目录下的 `_config.yml` 文件，大概格式如下：

```yaml
# Site
title: My Blog # 标题
subtitle: 'My Subtitle' # 名称
description: 'My Blog Description' # 描述
keywords: # 关键词
author: John Doe # 作者
language: zh-CN # 语言，这里默认的是 en，如果主题支持中文，这里修改为 zh-CN
timezone: ''
```

启用主题，在 `_config.yml` 查找 `theme:`，将主题文件夹名称替换成你的主题名（如 `maupassant`）：

```yaml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: maupassant
```

#### 3.5 编辑 Hexo 根目录下的 `.gitignore` 文件

删除 `.gitignore` 文件中的 `public/`，参考内容如下：

```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
.deploy*/
_multiconfig.yml
```

### 4. 创建文章

```bash
hexo new post "你的文章标题"
```

### 5. 创建列表分类

```bash
# categories 为分类名称
hexo new page categories
```

### 6. 文章中增加分类和 tag 标签

```yaml
title: 我的第一篇文章
date: 2024-07-12 10:00:00
# 分类栏目
categories:
  - 分类1
  - 分类2
# tags 标签
tags:
  - 标签1
  - 标签2
```

### 7. 生成和预览

安装和修改完成后，可在本地启用预览：

```bash
# 清理缓存
hexo clean

# 生成静态文件
hexo generate

# 启动服务
hexo server
```

根据返回地址在浏览器中访问：

```bash
# 调试地址
localhost:4000

# 停止调试服务
ctrl+c
```

---

## Github部分

### 1. 生成 SSH 密钥对

如果你还没有 SSH 密钥对，可以通过以下命令生成：

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

复制生成的公钥内容：

```bash
cat ~/.ssh/id_rsa.pub
```

登录 GitHub，进入 `Settings -> SSH and GPG keys -> New SSH key`，将公钥粘贴进去并保存。

### 2. 配置 Git 用户名和邮箱

在你的本地仓库目录下，配置 Git 用户名和邮箱：

```bash
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
```

如果 Github 中的邮箱添加了隐私设置，需要提前取消，或更改为 Github 提供的隐私邮件地址。取消方法：进入 `Settings -> Emails -> Keep my email addresses private`，取消勾选。

### 3. 推送到 GitHub

将 Hexo 项目推送到 GitHub 仓库：

```bash
git status
git init
git add README.md
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin git@github.com:/yourusername/your-repo.git
git push -u origin main
```

### 4. 自动处理行尾符

如果提示 `warning: in the working copy of 'package.json', LF will be replaced by CRLF the next time Git touches it`，执行下面的命令：

```bash
git config --global core.autocrlf true
```

---

## Cloudflare部分

### 1. 在 Cloudflare 上部署 Hexo

登录 Cloudflare 后，将语言切换到中文。接着导航到 `Workers 和 Pages`，选择 `Pages` 并连接到 GitHub 仓库。以下是关键步骤：

- **项目名称**：自定义，记住这是做什么的。
- **框架预设**：无
- **构建命令**：`npm run build`
- **输出目录**：`public`

最后点击：**开始部署**。

等待 Cloudflare 构建完成后，会随机分配一个 Cloudflare 提供的官方域名。后期可以绑定自己的域名。部署完成后，打开 Cloudflare 自动分配的域名预览是否安装正确。
