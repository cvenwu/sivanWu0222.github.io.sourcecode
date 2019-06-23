---
title: 版本管理工具-Git篇[3]
tags:
  - git
  - GitHub
  - 版本管理工具
share: true
categories:
  - git
  - 入门
reward: true
comment: true
top: 2
date: 2019-06-22 18:35:54
description:
---

![git三棵树](git 三棵树.png)

## git reset 命令

> git reset 默认是 git reset --mixed
> - 移动HEAD的指向，将其指向上一个快照，
> - **将HEAD移动后指向的快照回滚到暂存区域**
> git reset --soft HEAD~
> - 移动HEAD的指向，将其指向上一个快照,
> git reset --hard HEAD~
> - 移动HEAD的指向，将其指向上一个快照
> - **将HEAD移动后指向的快照回滚到暂存区域**
> - 将暂存区域的文件还原到工作目录(**会覆盖掉工作目录的文件，需要注意**)

回滚指定快照：
git reset 快照ID号   -- 回滚到指定的快照
回滚个别文件：
git reset 版本快照 文件名/路径  -- HEAD指针将不会移动

reset 命令回滚快照三部曲
移动HEAD的指向   **--soft**
移动HEAD的指向，并将快照回滚到暂存区  **--mixed**
移动HEAD的指向，并将快照回滚到暂存区，同时将暂存区还原到工作目录  **--hard**


git reset HEAD~        -- 回滚到上一次提交的快照
git reset HEAD~~       -- 回滚到上上次提交的快照
git reset HEAD~10      -- 回滚到前10次提交时的快照


## 关于分支的操作

git branch 分支名       -- 创建分支
git checkout 分支名     -- 切换分支

git checkout -b 分支名  -- 创建并切换分支
git merge 分支名        -- 合并分支名与master分支
git branch -d 分支名    -- 删除分支