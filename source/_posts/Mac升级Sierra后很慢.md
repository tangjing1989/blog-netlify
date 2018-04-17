title: Mac升级Sierra后很慢
author: John Doe
tags:
  - Mac
  - Mysql
  - ''
categories:
  - Mac
  - Mysql
date: 2017-02-07 14:13:00
---
Mac升级Sierra后，关机会很慢，查询后发现是mysql导致的，在系统设置中的mysql关闭时发现关闭不了，到mysql官网发现已经有支持sierra的最新版，备份数据库，卸载当前版本：
<!--more-->   
  
    sudo rm /usr/local/mysql
    sudo rm -rf /usr/local/mysql*
    sudo rm -rf /Library/StartupItems/MySQLCOM
    sudo rm -rf /Library/PreferencePanes/My*
    vim /etc/hostconfig  (and removed the line MYSQLCOM=-YES-)
    rm -rf ~/Library/PreferencePanes/My*
    sudo rm -rf /Library/Receipts/mysql*
    sudo rm -rf /Library/Receipts/MySQL*
    sudo rm -rf /var/db/receipts/com.mysql.*
    
安装从官网下载的最新版本，安装完后：
如果你的 /etc/my.cnf 不存在这个文件，你放一个在那里也行
    
    [client]
    port = 3306
    socket = /tmp/mysql.sock
    default-character-set = utf8

    [mysqld]
    collation-server = utf8_unicode_ci
    character-set-server = utf8
    init-connect ='SET NAMES utf8'
    max_allowed_packet = 64M
    bind-address = 127.0.0.1
    port = 3306
    socket = /tmp/mysql.sock
    
如果你用brew install mysql安装的，那么brew装好以后，把这些配置放到/usr/local/etc/my.cnf，然后执行下面命令就完成了mysql安装了
	
    unset TMPDIR
	mysql_install_db --verbose --user=`whoami` --basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql --tmpdir=/tmp

关于如何启动mysql和设置开机自动启动，可以通过brew info mysql查看。 

安装完毕后使用安全模式修改密码：
	
    mysqld_safe --skip-grant-tables &
    mysql> use mysql;
	mysql> update mysql.user set authentication_string=password('root') where user='root' and Host ='localhost'; 
	mysql> flush privileges;
	mysql> exit;