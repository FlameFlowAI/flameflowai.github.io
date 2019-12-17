---
title: How I build my blog？
date: 2015-06-15 09:31:11
categories: hexo

tags: hexo

---

I wrote an article "[Why I blog?](Https://johnwhite-leaf.github.io/2015/06/07/why-i-blog/)", combing my motivation and purpose of blogging , And this article mainly records the process of setting up your own blog.

Before I set up this blog, I sometimes write logs in the QQ space, but the sharing of the QQ space is limited to friends in the QQ space, and occasionally Weibo, but the information that Weibo can carry is very limited, I also tried I have written a blog on CSDN, but I have very limited independent control. I also purposely set up a [WordPress](http://cn.wordpress.org/) blog system on my local computer. It is also very bloated. I just want to write a blog simply, and I don't want to make it so complicated. In short, these methods have all kinds of limitations, and I can't let myself control the blog completely.

My experience is roughly as [Ruan Yifeng](http://www.ruanyifeng.com/) mentioned in [Building a Free, Unlimited Blog-GitHub Pages and Getting Started with Jekyll](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html) will go through three stages:

> In the first stage, I just came into contact with the blog and felt very fresh. Try to choose a free space to write.
> In the second stage, I found that the free space is too restrictive, so I bought a domain name and space and set up an independent blog.
> In the third stage, I feel that the management of the independent blog is too cumbersome. It is best to let others take care of it under the premise of retaining control and only write articles.

By chance, I read an article by CnFeat [How to Build an Independent Blog-Concise Github Pages and Hexo Tutorial](http://www.jianshu.com/p/05289a4bc8b2). Thanks to CnFeat for this tutorial that let me know a cool Hexo. The blog system based on hexo + Github Pages can meet my needs, with almost 100% control. Let Github Pages host the blog site and focus on writing articles.

## [Hexo](http://hexo.io) What is it?

[Hexo](http://hexo.io) is a tool for generating static blog sites

> A fast, simple & powerful blog framework

## [Githut pages](https://pages.github.com/) What is it?

[Githut pages](https://pages.github.com/) can be used to show your blog site to the world

> Websites for you and your projects.

The above mentioned CnFeat's tutorial for setting up an independent blog is very long and comprehensive, but in fact some steps do not necessarily need to be done immediately. It makes people feel complicated and daunting to put them together. The following is based on hexo + githut on win 7 The most basic steps required to build a blog page:

## Build a blog

The installation and use of [Hexo](http://hexo.io) depends on two tools [Node.js](http://nodejs.org/) and [Git](http://git-scm.com/ ):

### Install hexo's dependencies

- [Node.js](http://nodejs.org/) is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications.
  Node.js is a platform for building fast and stable web applications based on the JavaScript runtime of Chrome
  [Node.js download] for Windowds 64-bit version(http://nodejs.org/dist/v0.12.4/x64/node-v0.12.4-x64.msi)
- [Git](http://git-scm.com/) is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.
  Git is a free and open source distributed version control system, which was created to quickly and efficiently manage the source code of various large and small projects
  [Git download] for Windowds system(https://github.com/msysgit/msysgit/releases/download/Git-1.9.5-preview20150319/Git-1.9.5-preview20150319.exe)

The following two commands can be used to verify that these two tools are installed. If OK, go to the next step.

```
node --version
git --version
```

### Install hexo

```
npm install hexo-cli -g
```

Also, verify that hexo is installed with a similar command.

```
hexo --version
```

At the beginning, following this CnFeat tutorial to this step was unsuccessful, because the command used in the tutorial was npm install hexo -g, and the [hexo official website](http://hexo.io) found that after the hexo version upgrade, the installation command became npm install hexo-cli -g, so you must learn to get the latest information from the official website and improve your English. Many first-hand materials are in English.

### hexo initialization

Create a new directory to save hexo(for example, mine is in E:\blog), run the following command in the directory

```
  hexo init
  npm install
```

hexo init is used to generate some basic directories of hexo, npm install is used to install some dependent modules

### hexo server

After Hexo 3.0, the server has been separated. You need to install the server. The installation command is as follows

```
npm install hexo-server --save
```

### Blogging with hexo

```
hexo g
```

The role of hexo g is to generate a static blog website. hexo comes with a blog example. Here I will use the built-in example to go through the entire process, and then I will introduce in detail how to write a blog and change your favorite blog theme. The blog generated by hexo g is in the public directory under the directory where hexo is located. The blog is generated in the E: \ blog \ public directory. hexo g is a shorthand command, the full command is hexo generate

### Local preview of the blog just generated

```
hexo s
```

The role of hexo s is to locally deploy the blog generated by the hexo g command, and then you can easily enter http://localhost:4000/in the browser to preview the blog effect. The complete command of hexo s is hexo server. If you like the command line, you can use the following command to call your default browser to open the blog. When previewing the blog effect, please remember one thing: cherish life, stay away from IE

```
start http://localhost: 4000/
```


### If you enter the local address in your browser, you will see a whiteboard and Cannot GET /

The reason is that since it was updated after 2.6, we need to manually configure some things. Enter the following three lines of commands:

```
npm install hexo-renderer-ejs --save

npm install hexo-renderer-stylus --save

npm install hexo-renderer-marked --save
```

At this time, the website is built on the local computer, but now it can only be accessed by itself. If the whole world can access it, the generated blog site must be deployed to [Githut pages](https://pages.github.com/), Which is the place where [githut](https://github.com/) provides free services to githut users to deploy static web pages

### Open githut pages service

#### 1. Sign up for [Githut pages](https://pages.github.com/)

Remember to activate the github account at the email address of the registered githut account, otherwise the githut pages service may not take effect

#### 2. Create a new blog repository on github

The warehouse name must be "githut account name.github.io". For example, my githut account name is Johnwhite-leaf, then the name of the warehouse I built is Johnwhite-leaf.github.io

### Establish the association between hexo and githut pages:

#### 1. Generate SSH key and submit to github

```
ssh-keygen -t rsa -C "mail address@youremail.com"
```

After entering the information such as email address and password, the SSH key verification file is generally generated in the directory C:\Users\win7 user name\\.Ssh\, and the content in the file id_rsa.pub is submitted to Settings-> SSH in the githut website. Public keys. The SSH key is used to verify that your submission is valid. Only the computer that generates the SSH key and submits it to GitHub can modify your GitHub repository. This is your blog repository.

#### 2. Configure git email address and name

git commit must be configured information, this step can be done at any time after installing git and before hexo d, the configuration information will be displayed on each commit of git. hexo d called git to submit the changes to github, and subsequent changes to the theme and management of hexo code will also use git

```
git config --global user.email "mail address@youremail.com
git config --global user.name "your name"
```

#### 3. Configure hexo to manage github pages

Modify the following configuration in the _config.yml configuration file in the root directory of hexo. If not, add it at the end of the file, especially mine is in E: \ blog \\ _ config.yml. The name of my githut account is Johnwhite-leaf, and the corresponding configuration value is git@github.com: Johnwhite-leaf/Johnwhite-leaf.github.io.git

    deploy:
        type: git
    repository: https://github.com/github account name/github account name.github.io.git
        branch: master
    
    # Examples
    deploy:
        type: git
        repository: https://github.com/Johnwhite-leaf/Johnwhite-leaf.github.io.git
        branch: master




### Deploy blog to githut pages

```
hexo d
```

### After Hexo 3.0, deploy git is separated, so it needs to be installed. The installation command is as follows

```
npm install hexo-deployer-git --save
```

If there is no problem, you can access the blog just deployed in the browser through http://githutusername.github.io. For example, my githut account name is Johnwhite-leaf, and the corresponding URL is http://johnwhite-leaf.github .io. The complete command for hexo d is hexo deploy

## Simple post process

After setting up the hexo blog environment, the process of publishing articles is much simpler.

### 1. hexo new "title"
The title.md file will be generated in the E: \blog\source\_post directory of the directory where hexo is located

### 2. Edit the file generated in step 1
Write title.md file in markdown format, which can be edited by [markdownpad](http://markdownpad.com/) under windows

### 3. hexo g
Generate blog again after modification

### 4. hexo s
Preview blog effect locally. The general process, steps 2, 3 and 4 will be repeated many times, after the preview blog is satisfied, finally to step 5

### 5. hexo d

After setting up this blog, you can easily publish articles. Take part of your time every day to do what you like to do-write a blog, Follow my heart!

