---
title: 多台电脑的hexo博客同步实现
date: 2019-12-16 21:24:57
tags:
	- Hexo
	- Git
---

## 写在前面  
在hexo博客使用的过程中，突然想到了一个问题。因为hexo的工作原理是将本地写的博客，转换成html的文件部署到GitHub上的，那么如果我不只是在这一台电脑上来写博客（事实上也确实是这样的），怎么来实现博客的同步呢。  
<!--more-->  
## 实现方法  
通过在网上的查阅资料，用的最多的方法就是新建一个分支。通过在GitHub上新建一个分支，来保存本地的原始文件，另一个分支来保存hexo生成的静态网页。这里详细介绍以下步骤。  
假设要从电脑**A**将博客迁移到电脑**B**  
### 在电脑A上  
* 首先在仓库 [^username.github.io] 上新建一个分支*Hexo*，并且把它设置为**默认分支**，来保存本地的原始文件。此时该仓库有两个分支，一个是原来的存静态页面的仓库*master*，一个是新建的*Hexo*。  
[^username.github.io]: 你的静态网页存储仓库
* 在git bash中执行
> git clone git@github.com:username/username.github.io.git

将hexo分支拷贝到本地。 
* 将本地文件夹*username.github.io*文件夹里的所有文件删除，仅保留*.git*文件夹。  
* 将之前的博客目录拷贝至*username.github.io*文件夹下，一定要将*.gitingore.txt*拷贝过来。  
* 如果使用了主题，在主题文件夹下可能有*.git*的文件夹，要删除掉，否则无法push到GitHub上。  
* 依次执行<u>git add .</u>、<u>git commit -m ""</u>、<u>git push origin Hexo</u>将本地文件push到hexo分支上。  
此时在仓库[^username.github.io] 里就已经有两个分支，一个是存放本地的原始文件的分支Hexo，一个是存放生成静态网页的分支master。  
### 在电脑B上  
* 准备好之前建博客需要的Git、node.js等步骤。  

* 在git bash中执行

  > git clone git@github.com:username/username.github.io.git

将仓库克隆到本地，因为之前设置了默认分支，所以此时克隆的就是Hexo分支。  

* 在新克隆的文件夹下Git bash here，输入指令<u>cnpm install</u> .  
### 修改博客时  
在平时修改博客的时候，除了像以往用<u>hexo g</u>等指令发布，也要<u>git add .</u>、<u>git commit -m ""</u>、<u>git push origin Hexo</u>将改动推送到GitHub上。这样我们就实现了博客文件的版本控制，可以随时随地得修改博客了。  

## 其他问题  
有时候在主题文件下，将*.git*文件夹删除掉之后，push到GitHub上，主题文件夹会变成灰色，pull到本地时文件夹为空，像我所用的Next主题就遇到了这种情况。这种情况，在A电脑的博客目录下，执行
> git rm --cached themes/next  

就可以解决，后面的路径改成你的文件夹。具体原因我现在还没太搞懂，应该是与git子项目有关，因为这个项目是clone自别人的项目，没有push到GitHub上的权限。

