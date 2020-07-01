# CentOS 笔记

## 常见名词/工具解析
1. nvm 
> nvm是node版本管理工具,nvm是让你在同一台机器上安装和切换不同版本的node的工具[安装参考](https://www.jianshu.com/p/ea2b6a742e04)
2. wget
> wget是一个下载文件的工具
3. yum 
> Yum（全称为 Yellow dog Updater, Modified）是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理器
4. rpm
> rpm命令是RPM软件包的管理工具。rpm原本是Red Hat Linux发行版专门用来管理Linux各项套件的程序，由于它遵循GPL规则且功能强大方便，因而广受欢迎。逐渐受到其他发行版的采用。RPM套件管理方式的出现，让Linux易于安装，升级，间接提升了Linux的适用度。
5. vim
> Unix及类Unix系统文本编辑器

## 通用命令

1. 切换至root用户: `su`
2. 查看Linux系统内核版本: `cat /proc/version`

## 修改yum镜像源[【参考阿里云】](https://developer.aliyun.com/mirror/centos)

1. 首先备份系统自带yum源配置文件/etc/yum.repos.d/CentOS-Base.repo
`mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup`

2. 下载新的 CentOS-Base.repo (注意版本，本版本为8)
`wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-8.repo`

3. 生成缓存
`yum makecache`

## 安装node环境

1. 下载node（最新地址至node官网查看）
`wget https://npm.taobao.org/mirrors/node/v14.4.0/node-v14.4.0-linux-x64.tar.xz`

## 安装java环境

1. 查看yum源的java包
`yum list java*`

2. 安装java jdk包
`yum -y install java-1.8.0-openjdk`

3. 检查java是否安装成功
`java -version`

## 安装Jenkins[【参考】](https://www.jianshu.com/p/180fb11a5b96/)

1. 下载jenkins包
`wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo`

2. 导入秘钥
`rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key`

3. 下载（需要较长时间）
`yum install jenkins`

4. 查找jenkins安装路径
`rpm -ql jenkins`
> jenkins相关目录释义：
> 1) /usr/lib/jenkins/：jenkins安装目录，war包会放在这里。
> 2) /etc/sysconfig/jenkins：jenkins配置文件，“端口”，“JENKINS_HOME”等都可以在这里配置。
> 3) /var/lib/jenkins/：默认的JENKINS_HOME。
> 4) /var/log/jenkins/jenkins.log：jenkins日志文件。

5. 配置jenkins

  * 查看端口占用情况
  `netstat -ntlp`
  * 启动jenkins
  `java -jar /usr/lib/jenkins/jenkins.war --httpPort=8080`
  * 查看服务器是否开启
  > 打开浏览器，输入服务器IP+端口
  * 创建jenkins任务


## 配置jenkins任务

1. 服务器安装git
`yum install git`

2. 添加Generic Webhook Trigger Plugin插件（后简称GWT）
> 作用：git 每次提交都是发送到jenkins，都会触发构建功能，而GWT则可以区分不同分支来执行

3. Jenkins 构建项目，并开启（GWT插件）
  * 源码管理-> [√]git 项目关联git仓库（配置git项目地址[	Repository URL] 配置证书/用户[Credentials]）
  * 构建触发器->[√]Generic Webhook Trigger

4. git仓库(或许是码云)添加 Webhooks [参考链接](https://juejin.im/post/5ad1980e6fb9a028c42ea1be)
url格式:
`http://<User ID>:<API Token>@<Jenkins IP地址>:端口/generic-webhook-trigger/invoke`
`http://<Jenkins用户名>:<密码>@<Jenkins地址>/generic-webhook-trigger/invoke`
（eg：`http://admin:123456@192.168.1.2:8080/generic-webhook-trigger/invoke`）

5. 构建Jenkins项目自动化流程
  演示：

