---
title: git第一周笔记
date: 2023-03-17 17:19:48
tags:
cover: https://picshack.net/ib/WvLmrf8xXq.jpg
---

## 1. git版本库配置（init）

1. 创建一个**gitTUT**的文件夹，在文件夹内打开git bash命令窗口；

2. 用管理员身份去管理：

   ```git cofig --global user.name "liyilin"```

   `git cofig --global user.email "li@email.com"`

3. 确认身份象征：

   `git config user.neme`

   `git config user.email`

4. 清理命令：`clear`；

5. 在gitTUT文件夹内创建一个隐藏文件夹**.git**：

   `git init`

## 2. 添加文件管理（add）

1. **ls** 就能看到文件夹中的所有文件, 不过 git 创建的管理库文件 **.git** 是被隐藏起来的. 所以我们要执行`ls -a`能才能看到被隐藏的文件；

2. 建立一个新的 **1.py** 文件，然后就可以用 **status** 来查看版本库的状态：

   `git status`

   `git status -oneline`

3. 文件 **1.py** 并没有被放入版本库中 (unstaged), 所以我们要使用 **add**把它添加进版本库 (staged):

   `git add 1.py`

   `git add .` (一次性添加文件夹中所有未被添加的文件)

## 3.提交改变 (commit)

我们已经添加好了 **1.py** 文件, 最后一步就是提交这次的改变, 并在 **-m** 自定义这次改变的信息:

`git commit -m "create 1.py"`

**流程图**

![git流程图.png](https://s2.loli.net/2023/03/17/A2zHKZ8c7dBvuYO.png)

