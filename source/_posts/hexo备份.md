---
title: hexo备份
date: 2022-04-30 20:12:40
author: JonQuet
top: true
cover: true
tags:
- hexo
categories:
- 博客
typora-copy-images-to: upload
---

## hexo备份

## 前言

​    使用Hexo在github搭建的博客，博客作为一个单独的GitHub仓库存在，但是这个仓库只有生成的静态网页文件，并没有Hexo的源文件，如果要换电脑或者重装系统后，就比较麻烦了，这里推荐一种方法。

## 原理

我们的博客是托管到 GitHub（Coding） 上的。而我们每次上传（`hexo d`）的是网页文件，不是我们的文章，所以我们如果想上传文章，但同时不会干扰到网页部署，就在 GitHub 的博客仓库上建立一个分支 hexo。

这个 hexo 分支的作用就是用来保存我的博客所有文件。所以第一步我们就获得了博客仓库的 .git 文件夹，作用就是利用它连接到我们的博客仓库，而且建立分支 hexo。所以拿到这个文件夹，我们就把除了它的其他文件删掉。

然后利用这个分支，把我们的 MarkDown 文章和其他文件上传到 GitHub 托管。这样 `hexo d`推送的是 master 分支，而 `git push` 推送的是 hexo 分支，互不干扰。

## 步骤

### 1. 获取.git文件

1. 我们先建立一个文件夹，名字随便，我这里叫 hexo，在该文件夹空白处，启动 GitBash

2. 先克隆我们博客的仓库

   ```java
   git clone https://e.coding.net/fenghen0918/fenghen0918.git
   ```

这里仅仅只是为了获得版本管理的 **.git** 隐藏文件夹。

### 2. 建立分支

建立一个分支，我这里分支名为 hexo ，输入代码

```java
git checkout -b hexo
```

### 3. 清空 hexo 分支

克隆下来的都是编译后的静态页面，直接删除留下.git即可。

1. 删除除了 .git 文件夹的所有文件。我们只需要这个版本管理，在删除后通过代码 `git status` 查看

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220501132706.png)

1. 增加到暂存区

   ```java
   git add --all
   ```

2. 提交到本地仓库

   ```java
   git commit -m  "清空hexo分支仓库"
   ```

3. 最后我们推送到远端更新

   ```java
   git push --set-upstream origin hexo
   ```

这里同时设置了以后默认为hexo分支，回到博客的根目录下就能看到。

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220501132711.png)

> 注意：这时默认分支为hexo，但博客部署的还是master分支，此配置在config.yml中配置的
>
> ```java
> deploy:
>   type: git
>   repo: https://github.com/Witman1999/Witman1999.github.io.git
>   branch: master #提交的默认分支
> ```

### 4. 移动.git文件

把 .git 文件夹移动到博客的根目录下

### 5. 提交源文件

> 注意：如果你的主题文件，是克隆 Github 下来的，那么会带有该主题的 Github 的 .git 版本管理文件，也就是 .git 文件夹。所以主题下面的要删除 .git 文件夹和 .gitignore 文件，否则会忽略这个 next 主题的上传。

推送 Github 的仓库的步骤，在博客的根目录下，输入

```git
git add --all
git commit -m “提交源文件”
git push （这里要确保提交的分支为 hexo ，在前面的步骤可以查看，如果不是可以输入 git checkout hexo切换分支）
```

此时会发现在远程仓库的Hexo分支中已经推送了绝大部分文件，只有一些被忽略的文件，这些被忽略的并不需要拷贝

1. `_config.yml`站点的配置文件，需要拷贝；
2. `themes/`主题文件夹，需要拷贝；
3. `source`博客文章的.md文件，需要拷贝；
4. `scaffolds/`文章的模板，需要拷贝；
5. `package.json`安装包的名称，需要拷贝；
6. `.gitignore`限定在push时哪些文件可以忽略，需要拷贝；
7. `.git/`主题和站点都有，标志这是一个git项目，不需要拷贝；
8. `node_modules/`是安装包的目录，在执行`npm install`的时候会重新生成，不需要拷贝；
9. `public`是`hexo g`生成的静态网页，不需要拷贝；
10. `.deploy_git`同上，`hexo g`也会生成，不需要拷贝；
11. `db.json`文件，不需要拷贝。

其实不需要拷贝的文件正是`.gitignore`中所忽略的。

## 部署

在本地对博客修改（包括修改主题样式、发布新文章等）后：

1. 依次执行`git add .`、`git commit -m "推送"`、`git push origin hexo`来提交hexo网站源文件，因为我设置了默认hexo分支，直接输入`git push`即可；
2. 执行`hexo g -d`生成静态网页部署至Github上，我使用了gulp插件压缩代码，所以直接输入`gulp`，`hexo d`即可。

## 恢复

### 1.配置Hexo环境

重装电脑后，或者在其它电脑上想修改博客：

1. 安装git；
2. 安装Nodejs和npm；
3. 使用`git clone git@github.com:WincerChan/WincerChan.github.io.git`将仓库拷贝至本地；
4. 在文件夹内执行以下命令`npm install hexo-cli -g`、`npm install`、`npm install hexo-deployer-git`。

### 2.添加ssh-keys

1. 在终端下运行：`ssh-keygen -t rsa -C "yourname@email.com"`，一路回车；
2. 会在.ssh目录生成`id_rsa`、`id_rsa.pub`两个文件，这就是密钥对，id_rsa是私钥，千万不能泄漏出去；
3. 登录Github，打开「Settings」-->「SSH and GPG keys」，然后点击「new SSH key」，填上任意Title，在Key文本框里粘贴公钥id_rsa.pub文件的内容，注意不要粘贴成`id_rsa`，最后点击「Add SSH Key」。
