```
mkdir GinWebTest
cd GinWebTest
git init
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin https://gitee.com/subowen_admin/GinWebTest.git
git push -u origin master


cd existing_git_repo
git remote add origin https://gitee.com/subowen_admin/GinWebTest.git
git push -u origin master
```


git忽略已经被提交的文件  
```
git rm --cached stupid.txt //从版本库中rm 文件，working dicrectory中仍然保留，如果要删除目录下所有文件包括子目录中的 git rm -r --cached directory_name
在.gitignore中添加要忽略的文件
commit
push
其他成员pull，working directory中对应的文件会删除，所以如果文件重要，要提前备份。
 
 
git rm --cached IrisWeather
```

#### 常用命令
![常用命令](http://alicdn.epg.huan.tv/epg/tech/git-command.jpg)
