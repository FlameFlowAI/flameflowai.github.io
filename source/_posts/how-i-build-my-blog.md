title: 怎么搭建自己的博客？
date: 2015-06-15 09:31:11
categories:
 - hexo
tags: hexo
---
前面写了一篇文章“[为什么写博客？](https://johnwhite-leaf.github.io/2015/06/07/why-i-blog/)”，梳理了自己写博客的动机和目的，而这篇文章主要将自己搭建博客的过程记录下来。

在我搭建这个博客之前，我有时会在QQ空间上写日志，但QQ空间的分享只限在QQ空间好友之间，偶尔也发微博，但微博所能承载的信息非常有限，也尝试过在CSDN写博客，但自主控制权非常有限，也特意在本地电脑上搭建[WordPress](http://cn.wordpress.org/)博客系统，但WordPress是动态网页部署的条件比较苛刻，而且也很臃肿，我只是想简单地写博客而已，不想搞得那么复杂，总之这些方式都有这样那样的局限性，都不能让自己完全掌控自己的博客。

我的经历大致上正如[阮一峰](http://www.ruanyifeng.com/)在[《搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门》](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)中提到的喜欢写Blog的人会经历三个阶段：
>第一阶段，刚接触Blog，觉得很新鲜，试着选择一个免费空间来写。  
>第二阶段，发现免费空间限制太多，就自己购买域名和空间，搭建独立博客。  
>第三阶段，觉得独立博客的管理太麻烦，最好在保留控制权的前提下，让别人来管，自己只负责写文章。

机缘巧合下读到了CnFeat的一篇文章[《如何搭建一个独立博客——简明Github Pages与Hexo教程》](http://www.jianshu.com/p/05289a4bc8b2)。感谢CnFeat这篇教程让我知道了一个很酷的Hexo。基于hexo+Github Pages构建的博客系统正好可以满足我的需求，几乎100%的控制权，让Github Pages来托管博客网站，自己专注于写文章。
## [Hexo](http://hexo.io)是啥玩意？
[Hexo](http://hexo.io)就是一个用来生成静态博客网站的一个工具
>A fast, simple & powerful blog framework  
>快速、简洁且高效的博客框架

## [Githut pages](https://pages.github.com/)又是啥玩意？
[Githut pages](https://pages.github.com/)可以用来向全世界展示你的博客网站的地方
>Websites for you and your projects.  
>一个服务于你或你的项目的网站

上面提到CnFeat搭建独立博客的教程很长很全面，但其实有些步骤并不一定需要立刻要做，放在一起做让人感觉很复杂让人望而生畏，下面是我在win 7上基于hexo+githut pages搭建博客最基本的几个必要步骤：
## 搭建博客
[Hexo](http://hexo.io)的安装和使用依赖两个工具[Node.js](http://nodejs.org/)和 [Git](http://git-scm.com/) ：
### 安装hexo的依赖工具
- [Node.js](http://nodejs.org/) is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications.  
Node.js是一个基于Chrome的JavaScript运行时的构建快速稳定网络应用的平台  
适合Windowds系统64位版本的 [Node.js下载](http://nodejs.org/dist/v0.12.4/x64/node-v0.12.4-x64.msi)
- [Git](http://git-scm.com/) is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.  
Git是一个免费开源的分布式版本控制系统，为快速有效地管理各种大大小小项目的源码而生  
适合Windowds系统的[Git下载](https://github.com/msysgit/msysgit/releases/download/Git-1.9.5-preview20150319/Git-1.9.5-preview20150319.exe)

可用下面两个命令验证这两个工具是否安装好，如果OK再下一步
```
node --version
git --version
```
### 安装hexo

```
npm install hexo-cli -g
```

同样，用类似命令验证hexo是否安装好。
```
hexo --version
```

刚开始按照CnFeat的教程到这一步不成功，因为教程里用的命令是npm install hexo -g，而在[hexo官网](http://hexo.io)发现hexo版本升级后安装命令变成了npm install hexo-cli -g，所以要学会从官网获取最新信息，还要提高英语水平，很多第一手的资料都是英文的

### hexo初始化
新建一个专门保存hexo的目录（例如我的是在E:\blog），在目录下运行以下命令  
```
  hexo init  
  npm install
```

hexo init 是作用是生成了hexo的一些基本目录，npm install的作用是安装一些依赖的模块

### hexo server
在Hexo 3.0 后server被单独出来了，需要安装server，安装的命令如下
  
```
npm install hexo-server –-save 
```

### 通过hexo生成博客
```
hexo g
```

hexo g 的作用是生成静态博客网站。hexo自带一个博客例子，这里先利用自带例子走完整个流程，后续再详细介绍怎么写博客和更换自己喜欢的博客主题，hexo g生成的博客在hexo的所在目录下的public目录下，我的博客就在E:\blog\public目录生成。hexo g 是简写命令，完整命令是hexo generate
### 本地预览刚才生成的博客
```
hexo s
```

hexo s 的作用是在本地部署hexo g命令生成的博客，然后就可以很方便在浏览器中输入http://localhost:4000/ 预览博客效果，hexo s 的完整命令是hexo server。如果你喜欢命令行还可用如下命令调用默认浏览器打开博客。预览博客效果时请千万记住一点：珍爱生命 远离IE
```
start http://localhost:4000/
```


### 如果在浏览器输入本地地址访问，看到白板和Cannot GET / 几个字
原因是由于2.6以后就更新了，我们还需要手动配置些东西，输入下面三行命令：

```
npm install hexo-renderer-ejs --save

npm install hexo-renderer-stylus --save

npm install hexo-renderer-marked --save
```

此时网站在本地电脑上建好了，不过现在只有自己才能访问，要想全世界的人都能访问就必须把生成的博客网站部署到[Githut pages](https://pages.github.com/)，即[githut](https://github.com/)免费向githut用户提供的部署静态网页的服务的地方

### 开通githut pages服务
#### 1. 注册[Githut pages](https://pages.github.com/)账号
记得到注册githut账号的邮箱激活github账号，不然githut pages服务可能不生效
#### 2. 在github上新建博客仓库
仓库名必须是"githut账户名.github.io"，比如我的githut账号名为Johnwhite-leaf，那我建的仓库名则为Johnwhite-leaf.github.io

### 建立hexo和githut pages之间的关联：
#### 1. 生成SSH key并提交到github
```
ssh-keygen -t rsa -C "邮件地址@youremail.com"
```
输入邮箱和密码等信息后，一般会在在目录C:\Users\win7用户名\\.ssh\生成SSH key验证文件，将文件id_rsa.pub里面的内容拷贝提交到githut网站的Settings->SSH Public keys。SSH key用来验证你的提交是合法的，只有将生成SSH key提交到github上的电脑才有权限修改你的github仓库，这里就是你的博客仓库

#### 2. 配置git邮件地址和名字
git提交必须配置的信息，这一步骤在安装git之后和hexo d 之前任何时候做都可以，配置的信息将会在git的每个提交上显示。hexo d调用了git将修改提交到github，后续更换主题和管理hexo代码还会用到git
```
git config --global user.email "邮件地址@youremail.com  
git config --global user.name "your name"
```

#### 3. 配置hexo管理github pages
在hexo根目录的_config.yml配置文件中修改如下配置，没有就在文件末尾添加，特别地我的是在E:\blog\\_config.yml。其中我的githut账号名为Johnwhite-leaf，对应的配置值为 git@github.com:Johnwhite-leaf/Johnwhite-leaf.github.io.git

    deploy:  
        type: git  
	repository:  https://github.com/github账号名/github账号名.github.io.git
        branch: master
    
    # 例子	
    deploy:
        type:  git
        repository:  https://github.com/Johnwhite-leaf/Johnwhite-leaf.github.io.git
        branch:  master

      

### 部署博客到githut pages
```
hexo d
```

### 在Hexo 3.0版本后deploy git 被分开的，所以需要安装，安装命令如下
```
npm install hexo-deployer-git --save
```

不出问题的话就可以在浏览器中通过http://githut用户名.github.io 访问刚刚部署的博客，比如我的githut账号名为Johnwhite-leaf，对应网址 http://johnwhite-leaf.github.io 。hexo d 的完整命令是hexo deploy

## 简略的发表博文流程
搭建好hexo博客环境后,发表文章的的流程就简单多了

### 1. hexo new "title"，
   会在hexo所在目录的E:\blog\\source\\_post目录生成title.md文件
### 2. 编辑步骤1生成的文件
   用markdown格式编写title.md文件，windows系统下可用[markdownpad](http://markdownpad.com/)编辑
### 3. hexo g
   修改后再次生成博客
### 4. hexo s
   本地预览博客效果。一般的流程，步骤2，3和4会重复多遍，预览博客满意后，最后到步骤5
### 5. hexo d

搭建好这个博客后，就可以很方便地发表文章了。每天都抽一部分时间来做自己喜欢做的事--写博客吧，Follow my heart！
