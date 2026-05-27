---
layout: post
title: Prisma+Next搭建个人网站
date: 2026-01-04 22:08 +0800
categories: [前端]
tags: [Prisma, Next]
---
# 使用Prisma+Next搭建网站

## 一、首先需要创建Next项目
```shell
pnpm create next-app my-app
```
使用默认的模板即可，自带typescript、eslint、tailwindcss、App Router

## 二、在Next项目中加入Prisma
### 2.1 初始化Prsma
这里使用sqlite，可以按需切换为其他数据库
```shell
pnpm dlx prisma init --datasource-provider sqlite
```
这一步会在项目中创建prisma目录和.env文件

### 2.2 修改prisma下的schema.prisma
举个栗子🌰：
```prisma
// 添加一个Post表格
model Post {
    id String @id @default(cuid())
    title String
    content String
    published Boolean? @default(false)
    updateAt DateTime @updatedAt
    createAt DateTime @default(now())
}
```
### 2.3 推送修改
修改完后，需要运行push命令把修改推送到数据库
#### 2.3.1 直接push
适用于项目初期开发阶段或不考虑数据库版本管理
```shell
pnpm dlx prisma db push
```
> 后面只要对schema.prisma有所修改，都需要重新运行一下`pnpm dlx prisma db push`。<br/>这里第一次执行时，prisma会自动创建dev.db数据库文件，并新建我们在Schema中定义的表
{: .prompt-info}



