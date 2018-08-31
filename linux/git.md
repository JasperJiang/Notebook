# Git

初始化git repo

```bash
git init
git add .
git commit -m “xxx”
#远程git库
git remote add origin [url]   
git push origin
```

新建branch

```bash
git checkout -b [name_of_your_new_branch]
git checkout [name_of_your_new_branch]
git push origin [name_of_your_new_branch]
```

还原当前全部操作

```bash
git checkout -f
```

删除远程版本

```bash
git push origin :test
```

解决ignore失效

```bash
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
```

撤销操作

```bash
#git add 添加 多余文件
git status 先看一下add 中的文件
git reset HEAD 如果后面什么都不跟的话 就是上一次add 里面的全部撤销了
git reset HEAD XXX/XXX/XXX.java 就是对某个文件进行撤销了

#git commit 错误
git reset [commit_id]

#如果要是 提交了以后，可以使用 git revert
git reset --hard HASH #返回到某个节点，不保留修改。
git reset --soft HASH #返回到某个节点。保留修改
git push origin master --force
```

