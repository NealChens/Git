# Git
## 参考网站：https://www.liaoxuefeng.com/wiki/896043488029600

## 设置用户名与邮箱地址
--global表示你这台机器上所有的Git仓库都使用这个配置
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
git checkout -- filename
```
- 丢弃暂存区修改
```
git reset HEAD filename
git cheout -- filename
```

## 删除文件
**注意：从来没有被添加到版本库就被删除的文件，是无法恢复的!**
- 已经提交到版本库中，在文件管理器中（工作区）手动删除文件
- 1.确实要从版本库删除该文件
```
git rm filename
git commit -m "remove filename"
```
- 2.删错了，想恢复
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
- 1.当前分支
```
git config branch.master.mergeoptions "--no-ff"
```
- 2.当前版本库所有分支

```
git config --add merge.ff false
```
- 3.所有版本库的所有分支

```
git config --global --add merge.ff false
```

### bug分支
- 保存当前现场
```
git stash
```
- 查看工作现场
```
git stash list
```
- 恢复工作现场
```
//恢复同时把stash内容也删了
git stash pop
```
- 恢复指定的stash
```
git stash apply stash@{0}
```
- 删除stash内容
```
git stash drop
```
- 复制一个分支提交到当前分支
- 当前分支需将修改内容提交后再进行复制
```
//3622897为被删除的临时创建用来修复bug的分支
git cherry-pick 3622897
```
**example**
- 当前位于master分支上
```
//创建并切换到dev分支
git switch -c dev
//修改readme.txt中...，收到master的bug需立即解决，先保存现场
git stash
//切换到master分支
git switch master
//创建临时分支修改bug
git switch -c bug-101
//修改完readme.txt提交
git add readme.txt
git commit -m "fix bug-101"
//切换到master分支，进行合并
git switch master
git merge --no-ff -m "merge fix bug-101" bug-101
//删除bug-101分支,删除后会得到一个值(**bb061fb**),用于下面的复制
git branch -d bug-101
//回到dev分支继续修改
git switch dev
//查看工作现场
git stash list
//恢复的同时把stash内容删除
git stash pop
//因为dev分支是从master分支分出来的，这个bug在dev分支上也存在
//先将dev分支的修改提交，再将bug-101提交所做的修改复制到dev分支
git add readme.txt
git commit -m "learn fix bug through branch"
git cherry-pick bb061fb
```
### Feature分支
- 每添加一个功能，新建一个feature进行开发,完成后合并，最后再删除掉该分支
- 强行删除未合并的分支
```
//feature-1为新开发的功能分支
git branch -D feature-1
```

### 多人合作
**从远程库克隆时，Git自动把本地的master分支和远程的master分支对应起来,默认远程仓库名称为origin**
- 查看远程库
- -v显示更详细的信息，显示可以抓取和推送的origin的地址，如果没有推送权限，就看不到push的地址
```
git remote
```
-推送分支
```
//指定master分支上的所有本地提交推送到远程库origin
git push origin master
```
- 多人协作工作模式
- 1.试图推送自己的修改**git push origin branchname**
- 2.如果失败，先试图合并**git pull**
- 3.如果合并有冲突，则解决冲突，并在本地提交
- 4.没有冲突或者解决掉冲突后，再推送就能提交成功**git push origin branchname**
- 如果**git pull**提示no tracking information,说明本地分支与远程分支没有创建链接关系
- 使用**git branch --set-upstream-to=origin/branchname branchname**创建链接，本地和远程分支的名称最好一致

### Rebase
**rebase操作的特点：把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了**
- rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比
```
git rebase
```

## 标签管理
### 创建标签
- 打标签
```
//切换到打标签的分支上
git switch master
//打标签
git tag versionname
```
- 默认标签是打在最新提交的commit上的，可以找到历史提交commit id打标签
```
//-a指定标签名，-m指定说明文字,5756418是历史提交的commit id
git tag -a -v1.0 -m "version 1.0 released" 5756418
```
### 查看所有标签
```
git tag
```
### 查看标签信息
```
git show tagname
```
### 使用Github
- 在GitHub上，可以任意Fork开源仓库
- 自己拥有Fork后的仓库的读写权限
- 可以推送pull request给官方仓库来贡献代码

## 使用Gitee
**https://www.liaoxuefeng.com/wiki/896043488029600/1163625339727712**

## 自定义Git
### 显示颜色
```
git config --global color.ui true
```
### 忽略特殊文件
**https://www.liaoxuefeng.com/wiki/896043488029600/900004590234208**

### 配置别名
- --global是全局参数，针对当前用户，表示这些命令在本机上的所有Git仓库下都有用
```
//用st表示status，br表示branch
git config --global alias.st status
git config --global alias.br branch
//撤销修改
git config --global alias.unstage 'reset HEAD'
//配置最后一次提交信息
git config --global alias.last 'log -1'
//配置日志
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
#### 配置文件
**当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中**
```
//从分支返回主目录
cd
//查看.gitconfig文件
cat .gitconfig
```
- 每个仓库的Git配置文件都放在.git/config文件中
- 删除别名，在[alias]中直接把对应的行删掉就行

## 搭建Git服务器
**https://www.liaoxuefeng.com/wiki/896043488029600/899998870925664**

## GUI工具：SourceTree
