title: hexo生成静态博客
date: 2015-11-17 11:51:01
tags: [hexo]
---

###先屡一下思路 
1. 首先hexo是基于nodejs的，所以必须安装nodejs 
2. 安装nodejs方法很多，我选择homebrew安装方式，所以需要安装它 
3. 安装homebrew就很简单了，mac自带ruby脚本功能，一句话搞定 
4. hexo提交部署github需要使用git工具，所以需要安装git，用homebrew的话也是一句话搞定 
5. OK整理一下安装顺序（homebrew-nodejs-hexo-git） 

<!-- more -->
 
思路屡清楚了，下面安装方法整理一下 

###1.安装brewhome，一句话搞定

```
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)”
```

###2.0 安装nodejs 
####2.1 第一种方式，brewhome安装，一句话搞定

```
brew install node
```

####2.2 第二种方式，前提是已经安装好Xcode和git，安装git方法在下面介绍

```
git clone git://github.com/joyent/node.git
cd node
./configure
make
sudo make install
```

####2.3 第三种方式，下载源码( http://nodejs.org/download/ )，编译执行同上 

###3.0 安装hexo 
####3.1 第一种方式，用nodejs自带npm安装

```
npm install -g hexo
hexo init
npm install
```

####3.2 第二种方式，下载源码( http://www.nodejs.org/download/ )，编译执行 
###4.0 安装git 
####4.1 第一种方式，homebrew安装，一句话搞定 

```
sudo brew install git 
```

####4.2 第二种方式，前提是已经安装好Xcode

```
curl -O http://kernel.org/pub/software/scm/git/git-1.7.5.tar.bz2
tar xjvf git-1.7.4.1.tar.bz2
cd git-1.7.4.1
./configure --prefix=/usr/local
make
sudo make install
which git
```

####4.3 第三种方式，下载源码( https://www.kernel.org/pub/software/scm/git/ )，编译执行同上 
####4.4 第四种方式：图形界面安装OpenInGitGui( https://code.google.com/p/git-osx-installer )，但是天朝被墙 
####4.5 配置 
#####4.5.1 检查SSH key

```
cd ~/.ssh
```

#####4.5.2 备份已有的key，（如果有的话）

```
mkdir key_backup
mv id_rsa* key_backup
```

#####4.5.3 生成SSH key

```
ssh-keygen -t rsa -C "xxxx@xxxx.com”
```

#####4.5.4 将SSH key添加到Github 
登录到GitHub页面，Account Settings->SSH Public Keys->Add another key将生成的key(id_rsa.pub文件）内容copy到输入框中，save。 
#####4.5.5 测试连接
```
ssh git@github.com
```

#####4.5.6 设置个人信息

```
git config --global user.name "treason258”
git config --global user.email ma.tengfei2008@163.com
```
###5 修改hexo根目录下_config.yml文件（xxxx为你的github账户名称）

```
deploy:
  type: github
  repo: git@github.com:xxxx/xxxx.github.io.git
  branch: master
 ```

###6 注册github账号，新建名为xxxx.github.io的repository 
到这，Hexo博客搭建已经完成了，并且可以git提交到github上，通过访问xxxx.github.io就可以访问本博客，关于hexo的一些操作以后有时间再整理吧