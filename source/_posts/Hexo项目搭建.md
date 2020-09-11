---
title: Hexo项目搭建
date: 2020/09/10
categories: Web
tags: [Hexo,Git]
---

## 前言

本文将记录我在搭建Hexo项目时的一个大体步骤以及遇到的一些问题，总结一下，哪天若是需要重新弄一遍我也好有个参考。
由于能力有限涉及的知识不会很深，甚至会有些错误，可不能全信。



## 环境准备

简要的写一下需要哪些环境，为何需要，以及基本的安装步骤，不会说太详细。

如果遇到问题，搜索引擎是坠吼滴朋友。



### Git

[Git](https://git-scm.com/) 是一个开源的分布式版本管理工具。之后许多需要安装的软件都需要使用到git。git的基础使用在此不再赘述。

本着“能用就行”的理念，在Linux上直接使用了yum安装。

~~~shell
yum install git.x86_64
~~~



### Nginx

[Nginx](http://nginx.org/)  (engine x) 是一个高性能的HTTP和反向代理Web服务器。

#### 安装

我也不求什么最新版本，同样直接使用了yum安装。

~~~shell
yum install nginx.x86_64
~~~

#### 配置

默认的配置文件位置在

~~~shell
$NGINX_HOME/nginx.conf
~~~

配置文件的结构可参考[官方教程](http://nginx.org/en/docs/beginners_guide.html)或[官方文档](https://docs.nginx.com/nginx/admin-guide/web-server/web-server/#virtual-server)。

主要需要修改的配置如下：

~~~shell
server {
	# 服务器监听的端口
	listen       80 default_server;
	listen       [::]:80 default_server;
	# 服务器名称
	server_name  _;
	# 设置服务器根目录的位置
	root         /data/www/hexo_site/public;
	# Load configuration files for the default server block.
	include /etc/nginx/default.d/*.conf;

	# NGINX Plus can send traffic to different proxies or serve different files based on the request URIs by using location block。
	location / {
	}
	
	# 服务器错误的跳转页面
	error_page 404 /404.html;
		location = /40x.html {
	}

	error_page 500 502 503 504 /50x.html;
		location = /50x.html {
	}
}

~~~

#### 启动与关闭

使用

~~~shell
nginx
~~~

启动服务器，启动后，可以通过使用-s参数调用可执行文件来控制

~~~shell
nginx -s [signal]
~~~

signal可取：

- `stop` — fast shutdown
- `quit` — graceful shutdown
- `reload` — reloading the configuration file
- `reopen` — reopening the log files



### NVM

#### NodeJS

[NodeJS](http://nodejs.cn/) 是一个基于Chrome V8引擎的JavaScript运行时。简单来说，服务器安装了NodeJS之后，能够让在网页上运行的JavaScript也能在服务器端运行。即安装完NodeJS后就能在服务器使用JavaScript写脚本了。

而hexo是依赖于NodeJS的，所以需要安装NodeJS。

#### NVM

如果说手上有好几个项目，每个项目的需求不同，进而不同项目必须依赖不同版的NodeJS运行环境。如果没有一个合适的工具，这个问题将非常棘手。

[NVM](https://github.com/nvm-sh/nvm) （Node Version Manager）是一个独立于NodeJS的外部shell脚本，能够帮助管理NodeJS的版本。项目使用了git管理，故可以选择一个合适的位置使用

~~~shell
git clone https://github.com/nvm-sh/nvm
~~~

来拷贝它。

之后需要执行`nvm`文件夹下的`nvm.sh`脚本：`source nvm.sh`来初始化，之后便可使用nvm或npm相关命令了。

建议把此初始化命令加到`~/.bashrc`文件里，这样每次登录后就能自动初始化了。

使用`nvm ls`查看已安装的NodeJS版本，使用`nvm install [node_version]`来安装版本号为[node_version]的NodeJS。

#### NPM

NPM（Node Package Management）是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：

- 允许用户从NPM服务器下载别人编写的第三方包到本地使用，hexo就是这样一个第三方包。
- 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

详细介绍可见：[NPM 使用介绍](https://www.runoob.com/nodejs/nodejs-npm.html)



### Hexo

[Hexo](https://hexo.bootcss.com/)是一个快速、简洁且高效的前端博客框架，用于生成静态页面。

使用

~~~shell
npm install hexo-cli -g 
~~~

安装。



## 搭建流程

创建一个hexo项目的文件夹如`hexo_site`，使用命令`hexo init`来初始化一个hexo项目。

再使用`hexo generate`命令，静态页面便生成好了，位置在该项目的`.../hexo_site/public`文件夹中。

将Nginx配置文件中的`root`变量配置为上述的`.../hexo_site/public`文件夹位置，就大功告成了。

如果需要发布/删除博文，则在hexo项目的`.../hexo_site/source/_post`文件夹中进行管理即可。



## 配置与问题

### 站点基本信息配置

hexo项目的站点配置在项目根目录的`.../hexo_site/_config.yml`文件中，具体配置可参考[官方文档](https://hexo.io/zh-cn/docs/index.html)。

### 使用其他主题

以使用NexT主题为例：

定位到hexo项目的`.../hexo_site/theme`文件夹下，使用

~~~shell
git clone https://github.com/theme-next/hexo-theme-next
~~~

来拷贝主题，并在`.../hexo_site/_config`文件中设置`theme`字段的值为要使用的主题文件夹名称即可，如`theme: next`。

需要注意的是：通过 git 拷贝的主题中会存在属于主题的`.git`文件夹，这会使得hexo项目的git管理出现一些异常，比如某些文件add不上之类的，所以需要将其删除或重命名。

每个主题都有属于自己的主题配置文件，NexT主题的配置文件在`.../hexo_site/theme/next/_config.yml`，具体配置可参考[官方文档](http://theme-next.iissnan.com/)。

### 设置阅读全文

hexo官方推荐的方法是：在博文的markdown文件中添加`<!--more-->`标签来标识，我觉得太不优雅了，我不想每一篇文章都得这样单独加一笔。

早期版本的NexT可以通过在主题配置文件中配置

~~~yaml
auto_excerpt:
  # 是否启用
  enable: true
  # 截取的长度，默认为150字
  length: 150
~~~

但在较新一些的NexT版本中，这项功能貌似被删去了，而且官方文档也没进行说明，主题配置文件中

~~~yaml
# Automatically excerpt description in homepage as preamble text.
excerpt_description: true
~~~

如同摆设。

若需要实现则需要另外安装`hexo-excerpt`插件

~~~shell
npm install hexo-excerpt --save
~~~

在站点配置文件中进行配置

~~~yaml
excerpt:
# 这个depth与截取的字数成正相关，但也不是正比，具体如何对应的也不清楚，且只能取整数。
depth: 10
excerpt_excludes: []
more_excludes: []
hideWholePostExcerpts: true
~~~

### 使用git hook进行自动发布

首先使用git管理项目，在`.../hexo_site/`下使用`git init`初始化git仓库进行管理，git还会在`.gitignore`中自动添加一些忽略规则。由于hexo项目的初始化需要文件夹是空的，所以git的初始化需要放在后面，为便于说明，将此git仓库称为git客户端。

再在服务器的另一个位置使用`git init --bare`建立一个git的服务端，我们要实现的是：使用本地电脑提交新内容到服务器的git服务端，然后通过git服务端的hook脚本，同步git客户端的内容并即时生成最新静态页面并发布。

关于git hook，详细内容可查看[官方文档](https://git-scm.com/book/zh/v2/%E8%87%AA%E5%AE%9A%E4%B9%89-Git-Git-%E9%92%A9%E5%AD%90) ，此处使用的是`post-receive` 的hook 。

原理大致就是：git服务端在收发推送时，在此时间节点之前或之后能够自动执行一些脚本，此处使用的`post-receive` 脚本就是在git服务端接收推送之后会自动执行的脚本。

脚本的位置在git服务端下的`.../.git/hook/post-receive`，如果没有便新建并给予可执行权限，脚本内容可参考以下：

~~~shell
# 读取分支名称
while read oldrev newrev ref; do
	# 若接收的推送在master分支上时执行
	if [[ $ref =~ .*/master$ ]]; then
		# 设置为git客户端的位置
		DEPLOY_PATH="/data/www/hexo_site"
		# git的hooks里面默认有一些环境变量,会导致无论在哪个语句之后执行git命令都会有一个默认的环境路径，既然这样unset 掉默认的GIT环境变量就可以了。
		unset GIT_DIR #这条命令很重要
		cd $DEPLOY_PATH
		# 将git客户端重置一下，以防工作区或暂存区存在未保存的内容，导致后续操作失败
		git reset --hard
		# 将git服务端的内容同步到git客户端
		git pull origin master
		export NVM_DIR="/opt/nvm"
		# 初始化 nvm
		[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
		[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" # This loads nvm bash_completion
		nvm use stable # 设置使用的nvm版本
		# 更新hexo项目中使用的一些插件，没用上的话可以省略，cnpm是淘宝镜像版的npm，速度会快一些
		cnpm install hexo-excerpt --save
		# 生成静态页面
		hexo generate
	else
        echo "Ref $ref successfully received.  Doing nothing: only the master branch may be deployed on this server."
	fi
done
~~~

可以将这些命令的输出重定向至文件中，如果同步失败可以方便地查找原因。

#### 权限问题

如果整个过程全部都使用`root`用户进行操作的话，估计不会有啥问题，但我也没试。

这里我创建了一个git用户，并将git客户端与服务端的属主全设置为git，即`chown -R git:git [dir]` ，全程使用`git`用户进行操作。

并使用`ssh-keygen -t rsa` 创建一个用户签名，将这个签名公匙`id_rsa.pub`的内容放到`/home/git/.ssh/`文件夹下的`authorized_keys`文件中。此文件相当于白名单，对于文件中的所有签名，签名所属用户进行ssh连接的时候能够免密登录（在此若不这样做，脚本中进行git相关操作的时候仍会要求输入用户名与密码，这样相关操作就没办法完成了）。

最后在`/etc/passwd`设置用户git登录脚本为git-shell，妄图保证一点点安全。

### 在markdown格式的博文中引用图片

最简单的一个办法：资源（Asset）代表 source 文件夹中除了文章以外的所有文件，例如图片、CSS、JS 文件等。比方说，如果你的hexo项目中只有少量图片，那最简单的方法就是将它们放在 source/images 文件夹中。然后通过类似\!\[](./images/image.jpg) 的方法访问它们。