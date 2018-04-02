# Git Windows下配置Merge工具DiffMerge

## 下载DiffMerge
下载地址http://sourcegear.com/diffmerge/downloads.php  

## 创建启动DiffMerge脚本   
### 创建以下两个脚本，放到自己认为合适的位置

1.git-difftool-diffmerge-wrapper.sh  
> place this file in the Windows Git installation directory /cmd folder
> be sure to add the ../cmd folder to the Path environment variable
> 
> diff is called by git with 7 parameters:
> path old-file old-hex old-mode new-file new-hex new-mode
> 
> "C:/Program Files (x86)/SourceGear/DiffMerge/DiffMerge.exe" "$1" "$2" | cat

2.git-mergetool-diffmerge-wrapper.sh
> place this file in the Windows Git installation directory /cmd folder
> be sure to add the ../cmd folder to the Path environment variable
> 
> passing the following parameters to mergetool:
> local base remote merge_result
> 
> "C:/Program Files (x86)/SourceGear/DiffMerge/DiffMerge.exe" "$1" "$2" "$3" --result="$4" --title1="Mine" --title2="Merge" --title3="Theirs"

### 将两个脚本设置到环境变量中

### 修改全局.gitconfig文件
该文件一般在~/.ssh路径下
```config
#使用beyond compare来查看文件差异
[diff]
#对比工具名称,必须与difftool项里的名称保持一致
#tool = bc4

[difftool "bc4"]
#beyond compare路径和调用命令
#$REMOTE 表示commit之后的文件
#LOCAL 表示commit到git的文件
#cmd = "\"C:/Program Files/Beyond Compare 4/BComp.exe\" \"$REMOTE\" \"$LOCAL\""
	tool = diffmerge

#合并分支
[merge]
#对比工具名称,必须与mergetool项里的名称保持一致
#tool = bc4
	tool = diffmerge

[mergetool]
#prompt = false
	keepBackup = false

[mergetool "diffmerge"]
#beyond compare路径和调用命令
#cmd = "\"C:/Program Files/Beyond Compare 4/BComp.exe\" \"$LOCAL\" \"$REMOTE\" \"$BASE\" \"$MERGED\""
	cmd = git-mergetool-diffmerge-wrapper.sh "$LOCAL" "$BASE" "$REMOTE" "$MERGED"

[difftool "diffmerge"]
    cmd = git-difftool-diffmerge-wrapper.sh "$LOCAL" "$REMOTE"
```

## 使用
git mergetool为合并比较
git difftool 为对比比较

