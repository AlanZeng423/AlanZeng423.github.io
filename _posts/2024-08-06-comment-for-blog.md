---
layout: post
title: Add comments in blog posts on GitHub Pages websites
description: 如何在GitHub Pages网站的博客文章中添加评论功能
date: 2024-08-06
tags: Development
---

# Add comments in blog posts on GitHub Pages websites with utteranc
> 如何在GitHub Pages网站的博客文章中添加评论功能

- This, https://alanzeng.com/, is my Github Pages static website and blog, powered by the Jekyll static site generator.
- I’ve added GitHub-Issue-based comments to the bottom of this website using the plugin called [utterances](https://utteranc.es), 
which is totally awesome and makes it trivial to add a beautiful and user-friendly commenting system to your static website or blog.

## utteranc config
Visit Utterances main page & installation info.: [https://utteranc.es/](https://utteranc.es/)
<img src="https://raw.githubusercontent.com/AlanZeng423/Pics/main/MarkDown_Pics/image-20240806185908366.png" alt="image-20240806185908366" style="zoom:67%;" />
### repo：设置存放评论的仓库
Utterances 使用 Github Issues 存储评论，所以需要一个仓库。你可以新建一个公开仓库专门用来放评论，也可以使用原有的仓库。要设置存放评论的仓库只需要将 repo="username/reponame" 这一行中的 username 改为你的 GitHub 用户名，reponame 改为你的仓库名，其它不变。

**仓库需满足以下条件：**
- 仓库必须为公开仓库，私有仓库访客无法查看对应 Issues 上的评论。
- 确保在仓库中安装了 Utterances 的 GitHub App，或是你自己注册的 GitHub App（自托管），否则用户将无法发表评论。
- 如果你的仓库是派生 (fork) 出的，请在仓库的 Settings 选项确认 Features 区 Issues 已勾选。

### issue-term：博客文章和 Issue 映射

**Utterances 使用以下几种规则建立博客文章和 GitHub Issues 的映射：**

- Issue 标题包含页面路径名（issue-term="pathname"）
- Issue 标题包含页面 URL（issue-term="url"）
- Issue 标题包含页面标题（issue-term="title"）
- Issue 标题包含页面 og:title（issue-term="og:title"）
- 特定的 issue 编号（issue-number="具体数字"）
- Issue 标题包含特定项（issue-term="你设置的特定内容"）

> 具体细节参考上图

### label：Issue 标签

如果你使用原有的仓库，但是担心 Issues 页面评论和问题混杂在一起，Utterances 支持设置标签（Label）来区分它们。设置 label="你的标签内容"，Utterances 将在创建 issue 时使用你设置的标签。

- 标签名区分大小写。
- 标签必须存在于你的仓库中（须提前在 GitHub Issues 页面创建好，不能使用不存在的标签）。
- 标签名支持 Emoji。例如：label="💬"

### theme：主题

Utterances 有多种主题，其中包括多款夜间模式主题。

- GitHub Light：theme="github-light"
- GitHub Dark：theme="github-dark"
- GitHub Dark Orange：theme="github-dark-orange"
- Icy Dark：theme="icy-dark"
- Dark Blue：theme="dark-blue"
- Photon Dark：theme="photon-dark"


### 配置
```html
<script src="https://utteranc.es/client.js"
        repo="[ENTER REPO HERE]"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>
```
**将这部分代码粘贴进`_layout/post.html`中, 大功告成!**


> referenced:
> - [link](https://gabrielstaples.com/github-pages-comments/#gsc.tab=0)
> - [link](https://blog.njilc.com/post/self-hosted-utterances-tutorial)
