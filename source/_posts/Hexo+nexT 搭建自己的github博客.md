---
title: 如何使用hexo+next搭建属于自己的github博客
日期: 2017-09-27 20:28:46
标签:
---

环境安装就省略了...总体步骤为：
> * 搭建环境准备 （包括nodejs git gitHub账号配置）
> * 安装hexo
> * 配置hexo
> * 联系Hexo 和 github page
> * 怎样发布文章
> * 主题配置（NXET）
> * 插件sitemap 和feed 安装
> * 添加404页面

** `只是创建repo时后缀为github.io`**

## 安装Hexo

建立一个项目文件夹，进入该目录，命令行输入：
```
npm install -g hexo-cli 

```
执行hexo -v查看结果，验证是否安装成功
接下来初始化项目操作 输入：
```
hexo init 
```
执行完此条命令，会创建一个基于hexo的项目
执行  `npm install` 安装依赖

执行hexo g  hexo s 启动本地服务 如此本地服务站点搭建完成

## 如何*联系*Hexo和github page 
### 配置个人信息
  1. 在项目文件夹下执行git init
  2. 执行git remote add origin yougithub repo地址

### 配置 Deployment
在项目的根目录下的_config.yml 找到Deploy 修改：
```
deploy:
  type: git
  repo: git@github.com:yourname/yourname.github.io.git
  branch : master
```
### 接下来就可以写文章发布了
命令：
`hexo new post "article-name"`
此时会在你的source目录下生成一个.md的文件
用markdown编辑保存后，执行命令：
```
hexo g
hexo d  //部署到github服务器上
```
访问你的地址 https://yourName.github.io就可以看到了

## `注意需要安装一个扩展 npm install hexo-deployer-git --save`##

# 补充git
在管理Git项目上，很多时候都是直接使用https url克隆到本地，当然也有有些人使用SSH url克隆到本地。

这两种方式的主要区别在于：使用https url克隆对初学者来说会比较方便，复制https url然后到git Bash里面直接用clone命令克隆到本地就好了，但是每次fetch和push代码都需要输入账号和密码，这也是https方式的麻烦之处。

而使用SSH url克隆却需要在克隆之前先配置和添加好SSH key，因此，如果你想要使用SSH url克隆的话，你必须是这个项目的拥有者。否则你是无法添加SSH key的，另外ssh默认是每次fetch和push代码都不需要输入账号和密码，如果你想要每次都输入账号密码才能进行fetch和push也可以另外进行设置。前面的几篇介绍Git的博客里面采用的都是https的方式作为案例

1、设置Git的user name和email：(如果是第一次的话)
```
# 这里的“xujun" 可以替换成自己的用户名
git config --global user.name "xujun"
# 这里的邮箱 gdutxiaoxu@163.com  替换成自己的邮箱
git config --global user.email  "gdutxiaoxu@163.com"
```
2、检查是否已经有SSH Key 没有生成。
```
# 这里的邮箱 gdutxiaoxu@163.com  替换成自己的邮箱
ssh-keygen -t rsa -C "gdutxiaoxu@163.com"
```
默认的存储路径是：
`C:\Users\Administrator\.ssh`
3、添加密钥到ssh-agent
确保 ssh-agent 是可用的。ssh-agent是一种控制用来保存公钥身份验证所使用的私钥的程序，其实ssh-agent就是一个密钥管理器，运行ssh-agent以后，使用ssh-add将私钥交给ssh-agent保管，其他程序需要身份验证的时候可以将验证申请交给ssh-agent来完成整个认证过程```
# start the ssh-agent in the background
eval "$(ssh-agent -s)"
```
添加生成的 SSH key 到 ssh-agent。
`ssh-add ~/.ssh/id_rsa`

4、登陆Github, 添加 刚才本地生成的（id_rsa.pub）ssh 
5、测试：
`ssh -T git@github.com`


[百度](https:www.baidu.com)
![描述](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=3138365389,851751545&fm=27&gp=0.jpg)
[TOC]

作者 [@tuwei]    
2017 年 09月 27日  