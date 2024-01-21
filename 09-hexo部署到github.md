## 1. 安装环境

1. git
2. nodejs

## 2. 连接github

右键 -> Git Bash Here，**设置用户名和邮箱**

```
git config --global user.name "GitHub 用户名"
git config --global user.email "GitHub 邮箱"
```

**创建 SSH 密匙**

```
ssh-keygen -t rsa -C "GitHub 邮箱"
```

在家目录下有.ssh目录中有两个文件，id_rsa，id_rsa.pub。用记事本打开id_rsa.pub公钥文件，或者用命令：`cat ~/.ssh/id_rsa.pub`，然后复制密钥内容到GitHub账号的设置下的`SSH and GPG keys`

**验证连接：**

```
ssh -T git@github.com
```

## 3. 创建 Github Pages 仓库

GitHub 主页右上角加号 -> New repository

+ Repository name 中输入 `用户名.github.io`
+ 勾选 “Initialize this repository with a README”
+ Description 选填

## 4. 本地安装 Hexo 博客程序

新建一个文件夹用来存放 Hexo 的程序文件。打开该文件夹，右键 -> Git Bash Here。

使用 npm 一键安装 Hexo 博客程序：

```
npm install -g hexo-cli
```

Hexo 初始化和本地预览

```
hexo init    # 初始化
hexo g  	 # 生成页面
hexo s  	 # 启动预览
```

**访问** `http://localhost:4000`**，出现 Hexo 默认页面，本地博客安装成功！**

## 5. 部署 Hexo 到 GitHub Pages

本地博客测试成功后，就是上传到 GitHub 进行部署，使其能够在网络上访问。

首先**安装 hexo-deployer-git**：

```
npm install hexo-deployer-git --save
```

然后**修改 _config.yml** 文件末尾的 Deployment 部分，修改成如下：

```
deploy:
  type: git
  repository: git@github.com:用户名/用户名.github.io.git   # 使用ssh访问
  branch: main	#分支为main
```

成后运行 `hexo d` 将网站上传部署到 GitHub Pages。

完成！这时访问我们的 GitHub 域名 `https://用户名.github.io` 就可以看到 Hexo 网站了。

## 6. 修改主题

butterfly是个不错的主题，在hexo目录下执行以下命令安装该主题：

```
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

应用主题，修改 Hexo 根目录下的 _config.yml，把主题改为 butterfly

```
theme: butterfly
```

安装插件，如果你沒有 pug 以及 stylus 的渲染器，请下载安裝：

```
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

## 7. 实现步骤

1. 将github的hexo_demo仓库文件拷贝到本地电脑

```
git clone git@github.com:helloliyilin/hexo_demo.git			# ssh连接方式
git clone https://github.com/helloliyilin/hexo_demo.git		# https连接方式
```

2. 在本地电脑桌面或其他工作区创建一个hexo目录，在目录下右键 -> Git Bash Here，在终端输入命令：

```
hexo init
```

3. 把git clone后的文件和文件夹复制粘贴到本地工作区的hexo目录

![hexo工程必要目录](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20231127171310966.png "hexo工程必要目录")

hexo项目完整的工程树形结构如下

![hexo工程完整目录](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20231127171420492.png "hexo工程完整目录")

4. 新建一个.md文件，执行命令后会存放该工程下的`source/_posts`

```
hexo new "文件名字"
```

5. 可以使用typora或其他软件打开并编写`"文件名字".md`这个文件。

6. 一键部署到你的博客网站`helloliyilin.github.io`

```
npm install hexo-renderer-pug hexo-renderer-stylus --save  #安装pug以及stylus的渲染器
npm install hexo-deployer-git --save  						# 安装该依赖项，才能使用hexo d
hexo clean && hexo g && hexo d
```

