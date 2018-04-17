---
title: Hexo
tags:
  - 博客
  - 技术折腾
  - Git
categories: []
date: 2016-10-10 11:45:47
---

**一：安装hexo**

	  sudo npm install --unsafe-perm --verbose -g hexo
  	npm install hexo-deployer-git --save
<!-- more -->

**二：创建git远程登录公钥**

 	 ssh-keygen -t rsa -C "邮件地址@youremail.com"
 	 Generating public/private rsa key pair.
  	Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):<回车就好>
注意1: 此处的邮箱地址，你可以输入自己的邮箱地址；注意2: 此处的「-C」的是大写的「C」

然后系统会要你输入密码：
	
    Enter passphrase (empty for no passphrase):<输入加密串>
  	Enter same passphrase again:<再次输入加密串>
在回车中会提示你输入一个密码，这个密码会在你提交项目时使用，如果为空的话提交项目时则不用输入。这个设置是防止别人往你的项目里提交内容。

**三：初始化hexo**

执行init命令初始化hexo,命令：

  	hexo init

好啦，至此，全部安装工作已经完成！blog就是你的博客根目录，所有的操作都在里面进行。

**四：生成静态页面**

 	 hexo generate（hexo g也可以）

**五：本地启动**

启动本地服务，进行文章预览调试，命令：

  	hexo server

浏览器输入http://localhost:4000

然后建立关联，我的blog在本地/Users/leopard/blog，blog是我之前建的东西也全在这里面，有：

    _config.yml    node_modules    public      source

    db.json        package.json    scaffolds  themes

现在我们需要_config.yml文件，来建立关联，命令：

vim _config.yml

翻到最下面，修改成如下格式

  	deploy:
  	type: git
  	repo:https://github.com/用户名/仓库名称.github.io.git
  	branch:master

然后执行命令：

  sudo npm install hexo-deployer-git --save


然后，执行配置命令：

  hexo deploy

然后再浏览器中输入：
  
    http://仓库名.github.io/
    
**六：部署步骤**

每次部署的步骤，可按以下三步来进行。

    hexo clean

    hexo generate

    hexo deploy

一些常用命令：

hexo new"postName" #新建文章

hexo new page"pageName" #新建页面

hexo generate #生成静态页面至public目录

hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）

hexo deploy #将.deploy目录部署到GitHub

hexo help # 查看帮助

hexo version #查看Hexo的版本

**七：绑定域名**

万网买的域名（ http://xiaopingblog.cn/）。 域名买好之后提交实名认证等，这些操作就不在赘述。域名购买地址 

  https://wanwang.aliyun.com/ 

这里需要特别提一下的就是万网如何与 github pages 进行绑定，首先假设你有一个域名并且是可用状态。修改你域名的 DNS 地址为 ：  

 	 f1g1ns1.dnspod.net 
 	 f1g1ns2.dnspod.net
    
然后在你的本地站点目录里的 source 目录下添加一个 CNAME 文件，不带后缀，效果如下以文本编辑器打开 CNAME ，里面添加你的域名信息（不加http://。
下一步注册 DNSpod ，然后添加域名，添加记录即可。

添加域名填写你的域名即可，老规矩不用添加 http:// ，然后在点击你的域名点进去在添加记录即可（其中记录中CHAME的值是你的github pages的地址）。

那么现在把你本地的 Hexo 生成一下在提交到 Github pages 上吧（生成和提交简写命令 hexo d -g ），然后打开你的浏览器输入你购买的域名尝试吧。笔者的博客域名： http://xiaopingblog.cn/ 大家尽情的去蹂躏吧。

ps:万网DNS地址更换貌似需要一段时间才能生效，如果不能访问请晚点或者隔天再访问域名，如果还是不行可能就是出问题了。反正笔者当时运气好，修改了万网DNS之后即时生效。

**八：本地可视化**

本地可视化写文章，然后在使用命令提交上去。笔者使用的是一个 hexo 可视化文章管理的插件（hey），地址： https://github.com/nihgwu/hexo-hey

 	 npm install --save hexo-hey

安装完成后需要在_config.yml中添加如下内容：

    # Admin
    admin:
        name: hexo
        password: hey
        secret: hey hexo
        expire: 60*1
        # cors: http://localhost:3000
当然也有一个 Hexo 的 admin 插件（npm install --save hexo-admin），但是那个插件不支持图片拖拽进来，所以笔者推荐使用 hey 。安装和使用详情，参见笔者给出的github地址。

shell脚本自动化（可忽略，只是一个想法）

开启 Hexo 的本地服务或者提交到github pages这些都是一些终端里的 Hexo 命令，所以笔者写了一些 shell 脚本，来简化这些操作。所以基本就是用 hey 可视化写文章，写好了之后，然后点击 一键部署 的 shell 脚本，然后就自动发布了（当然这也纯属鸡助，看个人。）。由于 shell 脚本比较简单，随意网上搜索资料一大堆。再加上笔者自己写的脚本代码也没上传，在此插入一个一键部署的shell脚本代码

  	hexo clean
  	hexo g
 	 hexo d

cd到自己的站点目录，然后直接使用hexo命令即可。shell脚本自动化操作就是封装了这些命令而已，在此也只是提供这么一个想法，既然我们都是 coder ，何不善用自己已有的知识尼。