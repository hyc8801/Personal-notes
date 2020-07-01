# nginx 笔记

## 安装

`yum install nginx`

## nginx 操作常用命令

Nginx 的命令在控制台中输入 `nginx -h` 就可以看到完整的命令，这里列举几个常用的命令：

```
nginx -s reload # 向主进程发送信号，重新加载配置文件，热重启
nginx -s reopen # 重启 nginx
nginx -s stop   # 快速关闭
nginx -s quit   # 等待工作进程处理完成后关闭
nginx -T        # 查看当前nginx最终的配置
nginx -t -c <配置路径>   # 检查配置是否有问题，如果已经在配置目录，则不需要-c
```

`systemctl` 是Linx系统应用管理工具 `systemd` 的主命令。用于管理系统，我们也可以用它来对nginx进行管理

```
systemctl start nginx   # 启动nginx
systemctl stop nginx    # 停止nginx
systemctl restart nginx # 重启nginx
systemctl reload nginx  # 重新加载nginx，用于修改配置后
systemctl enable nginx  # 设置开机启动nginx
systemctl disable nginx # 关闭开机启动nginx
systemctl status nginx  # 查看nginx 运行状态
```