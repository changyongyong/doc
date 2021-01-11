# git初始化使用

## Git global setup
``` bash
git config --global user.name "changyong"
git config --global user.email "wenqier@live.cn"
```

## Create a new repository
``` bash
git clone git@gitlab.com:changyongyong/test.git
cd test
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
```

## Existing folder
``` bash
cd existing_folder
git init
git remote add origin git@gitlab.com:changyongyong/test.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

## Existing Git repository
``` bash
cd existing_repo
git remote rename origin old-origin
git remote add origin git@gitlab.com:changyongyong/test.git
git push -u origin --all
git push -u origin --tags
```

# Existing Git repository modify

1.修改命令  
``` bash
git remote origin set-url [url]
```
2.先删后加  
```bash
git remote rm origin
git remote add origin [url]
```
