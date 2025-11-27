---
layout: post
title: Ubuntu安装Mysql
date: 2025-11-27 22:59 +0800
categories: ['安装配置']
tags: ['Ubuntu', 'Mysql']
---
系统：Ununtu 22.04 LTS

安装版本：Mysql 8.4.7 LTS

安装包官方下载地址：[mysql-server_8.4.7-1ubuntu22.04_amd64.deb-bundle.tar](https://dev.mysql.com/get/Downloads/MySQL-8.4/mysql-server_8.4.7-1ubuntu22.04_amd64.deb-bundle.tar)

1）解压：

```bash
tar -xf mysql-server_8.4.7-1ubuntu22.04_amd64.deb-bundle.tar
```


2）安装所有依赖：

```bash
sudo dpkg -i mysql-*.deb
```


如果有报错（正常），执行：

```bash
sudo apt --fix-broken install -y
```


3）MySQL 服务启动：

```bash
sudo systemctl status mysql
```