title: OpenGrok搭建Android代码搜索服务器笔记
date: 2019-05-05 10:30:11
tags: OpenGrok
---


### 目录

1. OpenGrok介绍

2. 安装OpenGrok

    2.1 安装JAVA运行环境

    2.2 安装Web服务器-Tomcat

    2.3 安装OpenGrok

    2.4 配置OpenGrok

    2.5 安装 universal-ctags

    2.6 建立源码索引

    2.6 更新源码索引


### 1.OpenGrok介绍
OpenGrok is a fast and usable source code search and cross reference engine.  It helps you search, cross-reference and navigate your source tree

Requirements:

- Latest Java 1.8
- A servlet container like GlassFish or Tomcat (8.x or later) also running with Java at least 1.8
- Universal ctags (Exuberant ctags work too)

OpenGrok是一个快速, 方便使用的源码搜索引擎与对照引擎, 它能够帮助我们快速的搜索、定位、对照代码树. 

系统需求:

- Java 1.8
- Web服务器  GlassFish or Tomcat (8.x or later, 此处用Tomcat8)
- Universal ctags (Exuberant ctags也行, 此处有坑,实际上Exuberant ctags不行,最新版的OpenGrok在建立索引的时候会检测是否有Universal ctags, 没有直接的话报错)

简单来说OpenGrok就是一个毫秒级代码搜索的工具, 它最大的优点就是搜索速度贼快
     

### 2. 安装OpenGrok
#### 2.1 安装JAVA运行环境
OpenGrok 和Tomcat都依赖于 JAVA , 因此我们首先需要 JDK 来支持其运行

```
sudo apt-get update
sudo apt-get install openjdk-8-jdk
java -version   //查看java是否正确安装
```

具体安装请参照 Ubuntu安装JDK详解



#### 2.2 安装Web服务器-Tomcat
OpenGrok 是基于网页访问, 需要一个 Web 系统来支撑,所以必须安装Web服务器.

Ubuntu18.04 的源中已经提供了Tomcat8的包,  直接从源中安装 Tomcat8.
```
sudo apt-get install tomcat8
启动 Tomcat8

sudo service tomcat8 start   //启动Tomcat服务
或者

sudo /etc/init.d/tomcat8 start
启动 Tomcat 服务后, 在浏览器中输入网址, 看到页面显示"It works !"说明Tomcat服务OK了
```
http://localhost:8080



Tomcat8常用命令
```
sudo service tomcat8 start   //启动Tomcat服务
sudo service tomcat8 stop    //关闭Tomcat服务
sudo service tomcat8 status  //查看Tomcat服务状态
sudo service tomcat8 restart //重启Tomcat服务
```
更加详细的安装, 请参照Ubunt安装和配置tomcat8服务



#### 2.3 安装OpenGrok
安装好 Tomcat 后, 接下来就是配置 OpenGrok 了.

OpenGrok 下载地址 :

http://opengrok.github.io/OpenGrok

https://github.com/oracle/opengrok/releases

在该网址中可以下载 OpenGrok 的编译文件, 也可以下载源文件. 在此我们直接下载编译文件(对OpenGrok感兴趣的同学可以下载源码查看究竟）, 下载后通过以下命令进行解压：

tar xvzf opengrok-1.1-rc41.tar.gz -C /opt


#### 2.4 配置OpenGrok
如果希望 OpenGrok 能够正常运行, 则需要很多环境变量, 如果我们按照 OpenGrok的要求安装 jdk , Tomcat 和 OpenGrok, 并建立好目录结构的话, 这些环境变量在运行 OpenGrok 脚本的时候会被正确设置, 但是如果我们希望配置更加灵活的话, 还是自定义目录结构, 然后手动的配置这些环境变量吧,

环境变量的配置如下opengrokSetEnv.sh
```
#-------------------------------------------------------------------------------
#   - JAVA_HOME                   Full Path to Java Installation Root
#   - JAVA                        Full Path to java binary (to enable 64bit JDK)
#   - JAVA_OPTS                   Java options (e.g. for JVM memory increase
#-------------------------------------------------------------------------------
JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
JRE_HOME=$JAVA_HOME/jre
JAVA_BIN=$JAVA_HOME/bin
CLASSPATH=$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
export JAVA_HOME JRE_HOME PATH CLASSPATH

#-------------------------------------------------------------------------------
#   - OPENGROK_DISTRIBUTION_BASE  Base Directory of the OpenGrok Distribution
#   - OPENGROK_INSTANCE_BASE      Base Directory of the OpenGrok User Data Area
#   - EXUBERANT_CTAGS             Full Path to Exuberant CTags
#   - OPENGROK_CTAGS_OPTIONS_FILE Full path to file with extra command line
#                                 options for CTags program (for its --options
#-------------------------------------------------------------------------------
#  opengrok home directory
export OPENGROK_INSTANCE_BASE=/opt/opengrok
export SCRIPT_DIRECTORY=$OPENGROK_INSTANCE_BASE/bin
export OPENGROK_DISTRIBUTION_BASE=$OPENGROK_INSTANCE_BASE/lib


#  source code root
export SRC_ROOT=$OPENGROK_INSTANCE_BASE/database/src
#  generated data root
export DATA_ROOT=$OPENGROK_INSTANCE_BASE/database/data
#
EXUB_CTAGS=/usr/bin/ctags


#-------------------------------------------------------------------------------
#   - OPENGROK_APP_SERVER         Application Server ("Tomcat" or "Glassfish")
#   - OPENGROK_WAR_TARGET_TOMCAT  Tomcat Specific WAR Target Directory
#   - OPENGROK_WAR_TARGET_GLASSFISH Glassfish Specific WAR Target Directory
#   - OPENGROK_WAR_TARGET         Fallback WAR Target Directory
#   - OPENGROK_TOMCAT_BASE        Base Directory for Tomcat (contains webapps)
#   - OPENGROK_GLASSFISH_BASE     Base Directory for Glassfish
#                                 (contains domains)
#-------------------------------------------------------------------------------
export OPENGROK_APP_SERVER=Tomcat
export OPENGROK_TOMCAT_BASE=/var/lib/tomcat8
export OPENGROK_WAR_TARGET_TOMCAT=$OPENGROK_TOMCAT_BASE/webapps
export OPENGROK_WAR_TARGET=$OPENGROK_TOMCAT_BASE/webapps
export CATALINA_HOME=$OPENGROK_TOMCAT_BASE
```
完成后, 每次在运行 OpenGrok 之前就可以使用opengrokSetEnv.sh 脚本来设置环境变量了
```
source opengrokSetEnv.sh
```
OpenGrok脚本的信息请参见https://github.com/OpenGrok/OpenGrok/blob/master/OpenGrok

如果运行出错请查看源代码自行设置环境变量的信息

解压完成后进入到解压目录, 将 /opt/opengrok/lib 目录下的 source.war 包拷贝到apache-tomcat/webapps 目录下

在浏览器中输入

http://localhost:8080/source/

如果看到OpenGrok页面就证明OpenGrok运行成功.



接下需要更改 apache-tomcat/webapps/source/WEB-INF 目录下的web.xml配置文件
```
<context-param>
   <param-name>CONFIGURATION</param-name>
   <param-value>/opt/opengrok/etc/configuration.xml</param-value>
   <description>Full path to the configuration file where OpenGrok can read it's configuration</description>  
 </context-param>
 ```
配置 configuration.xml 的路径为 /opt/opengrok/etc/ 配置目录, 这个configuration.xm会在下面的建立源码索引步骤自动生成, 这里预先填上

#### 2.5 安装 universal-ctags

```
sudo apt install autoconf
cd /tmp
git clone https://github.com/universal-ctags/ctags
cd ctags
./autogen.sh
./configure --prefix=/opt/universal-ctags  # 我的安装路径。你按自己的情况调整。
make -j8
sudo make install
把ctags可执行文件更新到系统PATH上？No,我选择创建链接的方式：

# 如果你装了Exuberant ctags，要先删除链接文件：
# sudo mv /usr/bin/ctags /usr/bin/ctags.bak

# 然后，把新编译安装的universal-ctags链接过来：
sudo ln -s /opt/universal-ctags/bin/ctags /usr/bin/ctags
```

#### 2.6 建立源码索引
下面我们就需要为我们的源码配置索引了, OpenGrok 生成源代码的索引信息, 貌似是建立相关数据库,以便达到快速搜索的目的

设置的话需要如下环境变量

环境变量	描述	默认值
SRC_ROOT	待生成索引的源代码路径	${OPENGROK_INSTANCE_BASE}/src
DATA_ROOT	存放生成的索引的路径	${OPENGROK_INSTANCE_BASE}/data
那么我们直接在 opengrok 的安装目录 /opt/opengrok 下创建 src 和 data目录即可

接着我们将需要索引的源码放在 src 目录下即可, 当然我们其实没必要把源码真的放到这里, 只需要为其创建一个链接即可
```
cd /opt/opengrok/src
ln -s /usr/src/android8.0  android8.0
```
生成索引
```
indexer.py -J=-Djava.util.logging.config.file=/opt/opengrok/logging.properties \
    -a /opt/opengrok/lib/opengrok.jar -- \
    -s /opt/opengrok/src -d /opt/opengrok/data -H -P -S -G \
    -W /opt/opengrok/etc/configuration.xml -U http://localhost:8080
```

接着我们打开

http://localhost:8080/source

就可以看到我们的源代码了

#### 2.6 更新源码索引
当代码改变后就需要更新源码索引, 以便将变化反映到Opengrok上

```
touch /opt/opengrok/src/*

cd /opt/opengrok/bin

indexer.py -J=-Djava.util.logging.config.file=/opt/opengrok/logging.properties \
    -a /opt/opengrok/lib/opengrok.jar -- \
    -s /opt/opengrok/src -d /opt/opengrok/data -H -P -S -G \
    -W /opt/opengrok/etc/configuration.xml -U http://localhost:8080
```