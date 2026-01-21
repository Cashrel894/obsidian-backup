#mit6102 
主要记录个人不熟悉但实用的 git 命令/技巧

```shell
git config --global alias.lol "log --graph --oneline --decorate --color --all"
git lol # 显示git提交树
git lol hello.txt # 仅查看与hello.txt相关的提交树
```

```shell
git log -p/--patch # 展示每个commit的diff信息
git log -[k] # 显示最后k个提交
git log --stat # 显示每个文件的修改信息（仅增/删行数）
git log --name-status # 显示每个受影响文件的状态（新建/修改/删除）
# More on https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History
```