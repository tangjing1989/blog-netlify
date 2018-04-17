title: MAC 启动原生的NTFS支持
author: John Doe
tags:
  - MAC NTFS
categories: []
date: 2017-02-27 16:41:00
---

打开“实用工具”-“终端“里面进行输入命令。
可以从finder或者使用以下命令查看到磁盘的Volume Name:
<!--more-->
	
    diskutil list

可以看到，我的 Volume Name 是DISK。

紧接着更新 /etc/fstab文件

	sudo vi /etc/fstab

出现让你输入自己电脑的密码（没有密码的会跳过去），输入电脑密码后出现以下内容：
接着把以下内容写入进去

	LABEL=DISK none ntfs rw,auto,nobrowse

下面来依次解释一下，如果你的名字里面有空格键，就需要用\040的意思是代替空格键，如：DISK/040CAMP。
后面的Ntfs rw表示把这个分区挂载为可读写的ntfs格式，最后nobrowse非常重要，因为这个代表了在finder里不显示这个分区，这个选项非常重要，如果不打开的话挂载是不会成功的。
这个时候可以重启了。

当然还有个缺陷需要去掉：因为这个分区在finder里不显示了，那么我们要怎么找到它呢，总不能一直用命令行。
解决办法其实很简单，因为这个DISK分区是挂/Volumes下的，我们把这个目录在桌面做一个快捷方式就行了。

	sudo ln -s /Volumes/DISK ~/Desktop/DISK
抱歉一下，由于在Desktop/后面多输入个volumes，请最后一步没法能创建快捷方式的同学，用这个上面最新的公式。
如果想以后都能看到除DISK以外其他隐藏的驱动器的话，可以多创建一个这个文件夹快捷方式：如下：
	
    sudo ln -s /Volumes ~/Desktop/Volumes
