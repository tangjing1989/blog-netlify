---
title: git开发规范
tags:
  - git
categories: []
date: 2018-01-01 12:11:48
---

 最近小伙伴常常在提交代码时出现各种问题,先整理一份 git 开发规范来避免大家在提交代码时出现各种灵异事件~
<!-- more -->

# 第一步：新建分支

-- 获取主干最新代码 

    $ git checkout master
    $ git pull

-- 新建一个开发分支develop

    $ git checkout -b develop 

ps: 

    1.git branch -a 命令可以查看所有分支
    2. git checkout branchName 切换本地分支
    3. git checkout -b  branchName 表示创建并切换分支上。( git branch branchName ,git checkout branchName )
    4. git branch -d branchName 删除分支

# 第二步：提交分支commit
--分支修改后，就可以提交commit了。

    $ git add --all
    $ git status
    $ git commit --verbose

# 第三步：撰写提交信息
提交commit时，必须给出完整扼要的提交信息，下面是一个范本。

    Present-tense summary under 50 characters
    * More information about commit (under 72 characters).
    * More information about commit (under 72 characters).
    http://project.management-system.com/ticket/123

第一行是不超过50个字的提要，然后空一行，罗列出改动原因、主要变动、以及需要注意的问题。最后，提供对应的网址（比如Bug ticket）。

# 遇到冲突时的分支合并
有时候合并操作并不会如此顺利。如果在不同的分支中都修改了同一个文件的同一部分，Git 就无法干净地把两者合到一起（译注：逻辑上说，这种问题只能由人来裁决。）

    git merge branchName
    Auto-merging index.html
    CONFLICT (content): Merge conflict in index.html
    Automatic merge failed; fix conflicts and then commit the result

Git 作了合并，但没有提交，它会停下来等你解决冲突。要看看哪些文件在合并时发生冲突，可以用 git status 查阅：

    git status
    On branch master
    You have unmerged paths.
    (fix conflicts and run "git commit")
    Unmerged paths:
    (use "git add <file>..." to mark resolution)
            both modified:      index.html
    no changes added to commit (use "git add" and/or "git commit -a")

任何包含未解决冲突的文件都会以未合并（unmerged）的状态列出。Git 会在有冲突的文件里加入标准的冲突解决标记，可以通过它们来手工定位并解决这些冲突。可以看到此文件包含类似下面这样的部分：

        <<<<<<< HEAD
        <div id="footer">contact : email.support@github.com</div>
        =======
        <div id="footer">
        please contact us at support@github.com
        </div>
        >>>>>>> iss53

删除了 <<<<<<<，======= 和 >>>>>>> 这些行。在解决了所有文件里的所有冲突后，运行 git add 将把它们标记为已解决状态;

因为一旦暂存，就表示冲突已经解决。如果你想用一个有图形界面的工具来解决这些问题，不妨运行 git mergetool，它会调用一个可视化的合并工具并引导你解决所有冲突：

    $ git mergetool
    This message is displayed because 'merge.tool' is not configured.
    See 'git mergetool --tool-help' or 'git help config' for more details.
    'git mergetool' will now attempt to use one of the following tools:
    opendiff kdiff3 tkdiff xxdiff meld tortoisemerge gvimdiff diffuse diffmerge ecmerge p4merge araxis bc3 codecompare vimdiff emerge
    Merging:
    index.html
    Normal merge conflict for 'index.html':
    {local}: modified file
    {remote}: modified file
    Hit return to start merge resolution tool (opendiff):

如果不想用默认的合并工具（Git 为我默认选择了 opendiff，因为我在 Mac 上运行了该命令），你可以在上方"merge tool candidates"里找到可用的合并工具列表，输入你想用的工具名。我们将在第七章讨论怎样改变环境中的默认值。

退出合并工具以后，Git 会询问你合并是否成功。如果回答是，它会为你把相关文件暂存起来，以表明状态为已解决。

再运行一次 git status 来确认所有冲突都已解决：

    $ git status
    On branch master
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)
            modified:   index.html

如果觉得满意了，并且确认所有冲突都已解决，也就是进入了暂存区，就可以用 git commit 来完成这次合并提交。提交的记录差不多是这样：

    Merge branch 'iss53'

    Conflicts:
    index.html
    #
    # It looks like you may be committing a merge.
    # If this is not correct, please remove the file
    #       .git/MERGE_HEAD
    # and try again.
    #
如果想给将来看这次合并的人一些方便，可以修改该信息，提供更多合并细节。比如你都作了哪些改动，以及这么做的原因。有时候裁决冲突的理由并不直接或明显，有必要略加注解。

