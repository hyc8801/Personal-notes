# CentOS 笔记
<!-- TOC -->

- [CentOS 笔记](#centos-笔记)
  - [常见名词/工具解析](#常见名词工具解析)
  - [通用命令](#通用命令)
  - [修改 yum 镜像源[【参考阿里云】](https://developer.aliyun.com/mirror/centos)](#修改-yum-镜像源参考阿里云httpsdeveloperaliyuncommirrorcentos)
  - [安装 node 环境](#安装-node-环境)
  - [安装 java 环境](#安装-java-环境)
  - [安装 Jenkins[【参考】](https://www.jianshu.com/p/180fb11a5b96/)](#安装-jenkins参考httpswwwjianshucomp180fb11a5b96)
  - [配置 jenkins 任务](#配置-jenkins-任务)
  - [配置中遇到的问题](#配置中遇到的问题)
    - [一、 npm: command not found解决（环境变量无法获取）](#一-npm-command-not-found解决环境变量无法获取)
    - [二、配置多个地址](#二配置多个地址)

<!-- /TOC -->
## 常见名词/工具解析

1. nvm
   > nvm 是 node 版本管理工具,nvm 是让你在同一台机器上安装和切换不同版本的 node 的工具[安装参考](https://www.jianshu.com/p/ea2b6a742e04)
2. wget
   > wget 是一个下载文件的工具
3. yum
   > Yum（全称为 Yellow dog Updater, Modified）是一个在 Fedora 和 RedHat 以及 CentOS 中的 Shell 前端软件包管理器
4. rpm
   > rpm 命令是 RPM 软件包的管理工具。rpm 原本是 Red Hat Linux 发行版专门用来管理 Linux 各项套件的程序，由于它遵循 GPL 规则且功能强大方便，因而广受欢迎。逐渐受到其他发行版的采用。RPM 套件管理方式的出现，让 Linux 易于安装，升级，间接提升了 Linux 的适用度。
5. vim
   > Unix 及类 Unix 系统文本编辑器

## 通用命令

1. 切换至 root 用户: `su`
2. 查看 Linux 系统内核版本: `cat /proc/version`

## 修改 yum 镜像源[【参考阿里云】](https://developer.aliyun.com/mirror/centos)

1. 首先备份系统自带 yum 源配置文件/etc/yum.repos.d/CentOS-Base.repo
   `mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup`

2. 下载新的 CentOS-Base.repo (注意版本，本版本为 8)
   `wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-8.repo`

3. 生成缓存
   `yum makecache`

## 安装 node 环境

1. 下载 node（最新地址至 node 官网查看）
   `wget https://npm.taobao.org/mirrors/node/v14.4.0/node-v14.4.0-linux-x64.tar.xz`

## 安装 java 环境

1. 查看 yum 源的 java 包
   `yum list java*`

2. 安装 java jdk 包
   `yum -y install java-1.8.0-openjdk`

3. 检查 java 是否安装成功
   `java -version`

## 安装 Jenkins[【参考】](https://www.jianshu.com/p/180fb11a5b96/)

1. 下载 jenkins 包
   `wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo`

2. 导入秘钥
   `rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key`

3. 下载（需要较长时间）
   `yum install jenkins`

4. 查找 jenkins 安装路径
   `rpm -ql jenkins`

   > jenkins 相关目录释义：
   >
   > 1. /usr/lib/jenkins/：jenkins 安装目录，war 包会放在这里。
   > 2. /etc/sysconfig/jenkins：jenkins 配置文件，“端口”，“JENKINS_HOME”等都可以在这里配置。
   > 3. /var/lib/jenkins/：默认的 JENKINS_HOME。
   > 4. /var/log/jenkins/jenkins.log：jenkins 日志文件。

5. 配置 jenkins

- 查看端口占用情况
  `netstat -ntlp`
- 启动 jenkins
  `java -jar /usr/lib/jenkins/jenkins.war --httpPort=8080`
- 查看服务器是否开启
  > 打开浏览器，输入服务器 IP+端口
- 创建 jenkins 任务

## 配置 jenkins 任务

1. 服务器安装 git
   `yum install git`

2. 添加 Generic Webhook Trigger Plugin 插件（后简称 GWT）

   > 作用：git 每次提交都是发送到 jenkins，都会触发构建功能，而 GWT 则可以区分不同分支来执行

3. Jenkins 构建项目，并开启（GWT 插件）

- 源码管理-> [√]git 项目关联 git 仓库（配置 git 项目地址[ Repository URL] 配置证书/用户[Credentials]）
- 构建触发器->[√]Generic Webhook Trigger

4. git 仓库(或许是码云)添加 Webhooks [参考链接](https://juejin.im/post/5ad1980e6fb9a028c42ea1be)
   url 格式:
   `http://<User ID>:<API Token>@<Jenkins IP地址>:端口/generic-webhook-trigger/invoke`
   `http://<Jenkins用户名>:<密码>@<Jenkins地址>/generic-webhook-trigger/invoke`
   （eg：`http://admin:123456@192.168.1.2:8080/generic-webhook-trigger/invoke`）

5. 构建 Jenkins 项目自动化流程
   演示：

## 配置中遇到的问题

### 一、 npm: command not found解决（环境变量无法获取）

1. 确保服务器有安装node，执行`node --version`进行检查
2. 查看系统环境变量 `echo $PATH`
3. 将服务器的环境变量添加至jenkins中
   jekins -> 系统管理 -> 系统配置 -> 全局属性 -> 环境变量 -> 新增
   键： PATH     值：<第二步中查看的结果>

### 二、配置多个地址

1. 
