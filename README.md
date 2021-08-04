# Git

## 设置用户名与邮箱地址
global表示你这台机器上所以的Git仓库都使用这个配置
```
git config --global user.name "Your Name"
git config --global user.email "email@exp.com"
```

## 显示当前目录
```
pwd
```

## 查询目录下的文件
```
ls -ah
```

## 创建版本库
先创建空目录，切换到创建目录文件下，通过git init命令变成Git可以管理的仓库
```
mkdir learngit
cd learngit
git init
```

## 添加文件到Git仓库
文件一定要放在仓库目录下
git add可以添加多个文件，最后一起提交
```
git add <filename>
git commit -m <message>
```

