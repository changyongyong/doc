# git常用命令

## 克隆远程分支
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

	该命令会监控工作区的状态树，使用它会把工作时的所有变化提交到暂存区，包括文件内容修改(modified)以及新文件(new)，但不包括被删除的文件。
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
	git commit -am 'log'
	```
	
## 查看本地分支
	``` bash
	git branch
	```
	
	
	
	