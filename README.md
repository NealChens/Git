# Git

## 设置用户名与邮箱地址
global表示你这台机器上所有的Git仓库都使用这个配置
```
git config --global user.name "Your Name"
git config --global user.email "email@exp.com"
```

## 显示当前目录
```
pwd
```

## 查询目录下的文件
- -ah查看隐藏目录
```
ls
```

## 创建版本库
先创建空目录，切换到创建目录文件下，通过git init命令变成Git可以管理的仓库
```
mkdir learngit
cd learngit
git init
```

## 添加文件到Git仓库
- 文件一定要放在仓库目录下
- git add可以添加多个文件，最后一起提交
- -m 后面输入的是本次提交的改动说明
```
git add filename
git commit -m "message"
```

## 查询工作区状态
```
git status
```

## 查看改动内容
```
git diff filename
```

## 查看提交历史
- 回退版本
- 单行显示，加上--pretty=oneline
```
git log
```

## 查看命令历史
- 回到未来版本
```
git reflog
```

## 版本回退命令
- HEAD指向的版本就是当前版本
```
git reset --hard commit_id
```

## 撤销修改
- 丢弃工作区修改
```
git checkout -- file
```
- 丢弃暂存区修改
```
git reset HEAD file
git cheout -- file
```

## 删除文件
**注意：从来没有被添加到版本库就被删除的文件，是无法恢复的!**
- 已经提交到版本库中，在文件管理器中（工作区）手动删除文件
- 确实要从版本库删除该文件
```
git rm filename
git commit -m "remove filename"
```
- 删错了，想恢复
- git checkout其实是用版本库的版本去替换掉工作区的版本
```
git checkout -- filename
```

## 远程仓库
### 添加远程库
- 创建SSH Key
```
ssh-keygen -t rsa -C "Youremail@exp.com"
```
- 将生成的公钥(id_rsa.pub)粘贴到远程仓库的SSH key中
- github:右上角头像下拉框-->Settings-->SSH and GPG keys

- 关联远程库
- origin是默认指定远程库的习惯命名，后面是在Github上创建新仓库后ssh链接
```
git remote add origin git@github.com:NealChens/learngit.git
```
- 关联远程库后，就可以把本地库的所有内容推送到远程库中
```
//把当前分支master推送到远程，-u把本地的master分支和远程的master分支关联起来
git push -u origin master
//以后只要本地提交了，就可以通过以下命令将本地master分支的最新修改推送至Github
git push origin master
```

### 删除远程库
**删除只是解除本地与远程的绑定关系，真正删除远程库需在Github上所要删除的仓库-->Settings最下面的"Delete this repository"进行删除**
- 查看远程库信息
```
git remote -v
```
- 根据名字删除
```
//删除远程库origin
git remote rm origin
```

### 从远程库克隆
```
git clone git@github.com:NealChens/gitskills.git
```

## 分支管理
### 创建与合并分支
- 查看分支
```
git branch
```
- 创建分支
```
git branch branchname
```
- 切换分支
```
git switch branchname
```
- 创建+切换分支
```
git switch -c branchname
```
- 合并某分支到当前分支
```
git merge branchname
```
- 删除分支
```
git branch -d branchname
```

### 解决冲突
**当有多个分支修改后，Git无法自动合并分支时，必须先解决冲突后，再提交，最后再合并**
- 查看分支合并图
```
git log --graph --pretty=oneline --abbrev-commit
```

### 分支管理策略
- 合并分支时，加上--no-ff参数可以看合并后的历史所有分支，默认fast forward合并就看不出来曾经做过合并
```
git merge --no-ff -m "merge with no-ff" branchname
```
**设置省略--no-ff**
- 当前分支
```
git config branch.master.mergeoptions "--no-ff"
```
- 当前版本库所有分支

```
git config --add merge.ff false
```
- 所有版本库的所有分支

```
git config --global --add merge.ff false
```
