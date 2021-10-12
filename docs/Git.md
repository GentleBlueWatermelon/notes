# Git 笔记

# 基本操作

- 查看状态

```shell
git status
```

- 添加文件

```shell
git add fileName1 fileName2
```

- 删除文件

```shell
git rm 
```

- 提交文件
    + -m 提交消息

```shell
git commit -m "commit_message" fileName
```

- 查看历史记录操作

```shell
git log --pretty=oneline
git log --oneline
git reflog
```

- 回退/前进版本

```shell
git reset --hard 提交ID
git reset --hard~回退步数
```

- 比较文件差异

```shell
git diff 
```

# 分支操作

- 查看分支

```shell
git branch -v
```

- 新建分支

```shell
git branch 分支名
```

- 切换分支

```shell
git checkout 分支名
```

- 合并分支(切换到合并分支的分支)

```shell
git merge 分支名
```

- 删除分支(不能在要删除的分支上删除自己)

```shell
git branch -d 分支名
```
