# git常用命令

## 克隆
	根据用户配置，这里url做了修改，@后边少了.com      
	git@github.com:changyongyong/doc.git修改为git@github:changyongyong/doc.git     
``` bash
git clone git@github:changyongyong/doc.git
```

## 查询工程修改状态
``` bash
git status
```
	该命令提示下一步操作，以及一些内容修改，或者出现的问题      

##	增加文件

	该命令会监控工作区的状态树，使用它会把工作时的所有变化提交到暂存区，包括文件内容修改(modified)以及新文件(new)，但不包括被删除的文件，注意add后边英文句号    
``` bash
git add .
```
	该命令仅监控已经被add的文件（即tracked file），他会将被修改的文件提交到暂存区。add -u 不会提交新文件（untracked file）。（git add --update的缩写）    
``` bash
git add -u
```
	该命令是上面两个功能的合集（git add --all的缩写）     
``` bash
git add -A
```
	
## 提交修改
``` bash
git commit -am '日志描述'
```

## 拉取
从远程获取最新版本到本地 git pull = git fetch + git merge    
``` bash
git pull
```

## 获取
从远程获取最新版本到本地，但不会自动merge，跟本地对比之后需要merge才能到开发空间   
``` bash
git fetch
```

## 查看本地分支
``` bash
git branch
```
	
## 新建分支
``` bash
git branch newBranch
```

## 切换到你的新分支
``` bash
git checkout newBranch
```

## 创建并切换到新分支
``` bash
git checkout -b newBranch
```

## 合并分支
把otherBranch合并到当前分支，一般采用merge
``` bash
git merge otherBranch
```
有另一种合并，也是把otherBranch合并到当前分支
``` bash
git rebase otherBranch
```
merge和rebase的区别，以本地分支和私服分支为例：   
1、merge合并后结果是一个新的提交，（merge commit）是一个新版本，相当于把私服的内容给本地修改了一次     
2、rebase合并后结果不是新的提交，是把自己分支修改的部分接续到私服分支上，相当于本地修改的记录是在私服最新版本上迭代的，比merge少了一个（merge commit）版本，而且和merge的log日志顺序不一样，不过该情况下也造成rebase冲突概率较大     

## 拉取远程分支并创建本地
这种情况会自动关联远程和本地分支   
``` bash
git checkout -b newBranch origin remoteBranch
```

## 将新分支发布到远程
``` bash
git push origin newBranch:newBranch
```

``` bash
git push -u origin newBranch
```

## 关联本地分支和remote分支
``` bash
git branch --set-upstream-to=origin/dev dev
```
``` bash
git branch --set-upstream dev origin/dev
```

## 删除本地分支
``` bash
git branch -D BranchName
```	

其中-D也可以是--delete，如：    
``` bash
git branch --delete BranchName
```	

## 删除本地的远程分支
``` bash
git branch -r -D origin/BranchName
```	

## 远程删除git服务器上的分支
``` bash
git push origin -d BranchName
```	

其中-d也可以是--delete，如：    
``` bash
git push origin --delete BranchName
```		

也可以是置空远程分支(origin 后面有空格)：   
``` bash
git push origin :br  
```	

## 本地代码库回滚
回滚到commit-id，将commit-id之后提交的commit都去除，不带commit-id即回滚到最近一次提交到远端版本，
所以一般要用服务器覆盖本地需要先fetch命令  
``` bash
git reset --hard commit-id
```	

将最近3次的提交回滚    
``` bash
git reset --hard HEAD~3
```	