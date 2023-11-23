---
title: Git常用命令
cover: ''
tags:
  - Git
  - 学习笔记
categories: 笔记
abbrlink: caff8000
date: 2023-01-24 10:38:11
---

# Git常用命令

|                 命令                 |                             说明                             |
| :----------------------------------: | :----------------------------------------------------------: |
|              `git init`              |                        初始化本地仓库                        |
|    `git commit -am “第一次提交”`     |                      添加并提交到本地库                      |
|             `git status`             |                       查看当前文件状态                       |
|             `git reflog`             |                         查看简洁日志                         |
|              `git log`               |                         查看完整日志                         |
|             `git remote`             |                       查看添加的远程库                       |
| `git remote add origin [远程库地址]` |                添加一个别名为`origin`的远程库                |
|      `git push -u origin main`       |      将本地库`main`分支推送到`origin`远程库的`main`分支      |
|   `git push -u [远程库地址] main`    | 将本地库`main`分支推送到`[远程库地址]`所在的远程库的`main`分支 |
|   `git push -u origin master:main`   |     将本地库`master`分支推送到`origin`远程库的`main`分支     |
|        `git remote rm origin`        |                      删除`origin`远程库                      |
|                 分支                 |                                                              |
|           `git branch -v`            |                         查看所有分支                         |
|           `git branch dev`           |                   创建一个名为`dev`的分支                    |
|          `git checkout dev`          |                       切换到`dev`分支                        |
|        `git checkout -b dev`         |        创建一个名为`dev`的分支,  同时切换到`dev`分支         |
|         `git branch -M test`         |                  将当前所在分支更名为`test`                  |
|           `git merge main`           |    将`main`分支合并到当前分支(可能会产生冲突, 需手动处理)    |
|      `git reset –hard [版本号]`      |         切换到某个版本，可通过`git reflog`查看版本号         |
|      `git branch --delete dev`       |                        删除`dev`分支                         |
|       ` git reset --hard HEAD`       |                   恢复到最后一次提交的状态                   |
|       `git clone [远程库地址]`       |                    克隆远程库的项目到本地                    |

# 扩展

- 官网:  https://git-scm.com/book/zh/v2

- 常用命令:  https://yelog.org/2016/10/02/git-command/
- git - 简易指南:   https://www.bootcss.com/p/git-guide/
- 菜鸟Git教程:  https://www.runoob.com/git/git-tutorial.html
- Git大全:  https://gitee.com/all-about-git



# 基本结构

![Git](http://qiniu.yujing.fit/typora_img/Git.png)

# Pull Request

1. `fork`项目到本地仓库
2. 对项目更改后提交到本地库
3. 点击`Pull request`创建一个拉取请求
4. 写入日志提交
5. 仓库主点击Pull Request 查看更改的代码
6. 选择是否合并请求



# 常见问题

- `push`之前应先`pull`保持版本最新

## Github网络问题

- 使用`ssh`提交
- 更换使用`Gitee`
- 多尝试几次
- 尝试下面两种方法

**修改设置，解除ssl验证**

```
git config --global http.sslVerify "false"
```

取消代理

```cmd
git config --global --unset http.proxy
git config --global --unset https.proxy
```

当两个仓库历史不一致时, `fatal: refusing to merge unrelated histories`

```bash
git pull https://gitee.com/yu-jing-Tiam/gitee_-web.git master --allow-unrelated-histories
```





