---
tags: [Notebooks/Study]
title: git初次提交错误，github token权限不足
created: '2022-05-07T06:32:22.858Z'
modified: '2022-05-07T07:05:28.207Z'
---

# git初次提交错误，github token权限不足

创建了需要使用到yml的工程，在初次上传的时候报错
> refusing to allow an OAuth App to create or update workflow

**原因是access token没有workflow权限**
去Settings-》Developer-》Access Token重新Update一下token，同时记得勾上workflow权限
记录好新生成的token

git的操作：
重新生成完token后，git要么是OpenSSL SSL_read: Connection was reset, errno 10054要么是Authentication failed（time out忽略不计），此时应该重新用新token和远程仓库建立下连接，找到了两种方法，二者均可：
1. git命令操作
```git
$ git remote rm origin
$ git remote add origin https://<name>:<token>@github.com/<name>/<repo>.git
$ git status
On branch master
nothing to commit, working tree clean
$ git push -u origin master
```
2. 基于windows的凭据管理器
凭据管理器（credentials manager）-》windows credentials-》普通凭据，添加新凭据
> Internet地址或网络地址：git:https://github.com
用户名：\<name>
密码：\<token>
我用的第一种方法，同样也在凭据管理器中生成了凭证，所以ok

试错：之前尝试撤回push，但说是撤回，其实是回滚到前面的某一状态，本次的工程是第一次push，所以方法没成功
首先`git reflog`查看headID，在reset即可
```git
$ git reset --soft HEAD  # HEAD~1是前一commit

$ git reflog
69d375b (HEAD -> master) HEAD@{0}: reset: moving to HEAD
69d375b (HEAD -> master) HEAD@{1}: reset: moving to HEAD
69d375b (HEAD -> master) HEAD@{2}: commit (initial): first-commit
```
这里发现没法撤回，所以此方法被pass（所以感觉初次只能无脑push -u欸，，，）

新问题：update token且依照上述修改后，cmd命令行仍出现OpenSSL SSL_read: Connection was reset, errno 10054错误？




参考：
https://www.jianshu.com/p/be31d1ab30d0
https://segmentfault.com/a/1190000040544939
https://stackoverflow.com/questions/64059610/how-to-resolve-refusing-to-allow-an-oauth-app-to-create-or-update-workflow-on
推荐：
https://git-scm.com/book/en/v2
【git真的是，，没问题的时候以为就记那几条命令就够了，一有问题真的抓瞎qwq】

