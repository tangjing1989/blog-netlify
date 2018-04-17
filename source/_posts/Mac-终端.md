---
title: Mac_终端改造
tags:
  - Mac
  - 终端
categories: []
date: 2016-10-09 15:26:00
---

**一：终端切换bash **
   所需材料：
          <!-- more-->
  1. Solarized Dark，配色方案大集合，下载后找到iTerm的配色方案双击安装；
  2. oh-my-zsh，前面所说的Z shell的现成配置方案，方便你管理自己的zsh，可使用curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh进行安装；
  3. Menlo-Powerline or Monaco-Powerline，两种字体补丁任选其一下载安装即可，不安装会有乱码.
  注：
1.调整选中时颜色为明黄色，
2.安装好oh my zsh后，在~/.zshrc中添加如下内容，能让你用的更愉快，
* ZSH_THEME="agnoster"  #使用 agnoster 主题，很好很强大
* DEFAULT_USER="你的用户名"     #增加这一项，可以隐藏掉路径前面那串用户名
* plugins=(git brew node npm)   #自己按需把要用的 plugin 写上

**二：自动补全终端代码 **
　打开终端，输入： nano .inputrc
　在里面粘贴上以下语句：

     set completion-ignore-case on
     set show-all-if-ambiguous on
     TAB： menu-complete   
 Control+O，保存，重启终端，OK！
　　  这就是开启Terminal自动补全功能的方法啦，经常需要使用Terminal或者输入命令句的用户们，快把这个神功能打开吧。