---
layout:     post
title:      "Linux下安装Shadowsocks"
subtitle:   ""
date:       2017-06-03
author:     "Will"
header-img: "img/linux/linux.png"
catalog: true
tags:
    - Linux
    
---

> “walk beside you ”


## 前言
   高阁客竟去，小园花乱飞。
   参差连曲陌，迢递送斜晖。
   肠断未忍扫，眼穿仍欲归。
   芳心向春尽，所得是沾衣。
    李商隐 《落花》

---

## 正文
Shadowsocks，中文名影梭，使用socks5代理。
本次安装使用的系统为 CentOS 7.4

#### 安装 pip
Shadowsocks 安装需要先安装 Python的包管理工具 pip ，最新的系统里已经预装了 pip 。

查看是否已安装 使用命令   ``` pydoc modules ``` 来显示所有Python安装的模块 。

如需安装pip 使用命令 ``` python get-pip.py  ```

#### 安装 Shadowsocks

直接使用命令 ``` pip install shadowsocks ``` 进行安装

#### 修改 Shadowsocks 配置文件
首先需创建配置文件：``` vi /etc/shadowsocks.json ```
添加内容：

 ```
{
    "server":"your_server_ip",
    "server_port":14000,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"password",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": true,
}
  ```
属性的含义：

* server：服务器 IP 地址（如果公网地址不行，换成内网地址）
* server_port：监听的服务器端口
* local_address：本地监听的 IP 地址，直接写127.0.0.1就可以了,默认就是127.0.0.1,不用更改
* local_port：本地端端口
* password：密码
* timeout：超时时间（秒）
* method：加密方法，可选择 “bf-cfb”, “aes-256-cfb”, “des-cfb”, “rc4”, 等等。推荐使用:aes-256-cfb
* fast_open：true 或 false。如果你的服务器 Linux 内核在3.7+，可以开启 fast_open 以降低延迟。
* wokers :worker数量,默认为 1,(这个只在Unix和Linux下有用，可不设置)

Shadowsocks 也支持多用户配置 如下：

 ```
{
    "server": "0.0.0.0",
    "port_password": {
        "port1": "password1",
        "port2": "password2",
        "port3": "password3"
    },
    "timeout": 300,
    "method": "aes-256-cfb"
}
 ```
#### 启动/停止

* 启动：  ``` ssserver -c  shadowsocks.json -d start  ```

* 停止：  ``` ssserver -c  shadowsocks.json -d stop ```

* 查看日志： ``` tail -f /var/log/shadowsocks.log ```

#### 客户端

* 客户端直接到 [官网地址](https://shadowsocks.org/en/index.html) 下载就行，把对应的ip、密码填上就行

* 浏览器插件，Proxy SwitchySharp（用于Chrome）
