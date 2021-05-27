查看当前的分支

`git branch`

查看当前文件状态

`git status`

新建分支

`git checkout -b name`

推送到本地分支

`git add .`

提交信息

`git commit -m message`

将本地分支推动到云端`origin仓库`以`user`分支进行保存

`git push -u origin user`

先切换到主分支，从主分支上把代码合并，`master变成最新的`

`git merge user`

将本地的`master`和云端的`主分支master`进行合并

`git push -u origin 分支名`

从远端拉去代码到本地

`git pull origin 主分支名字`





删除关联的origin的远程库

` git remote rm origin`

>  git remote rm origin 还是报错的话，error: Could not remove config section 'remote.origin'. 我们需要修改gitconfig文件的内容
>
> 1、找到你的github的安装路径，我的是C:\Users\ASUS\AppData\Local\GitHub\PortableGit_ca477551eeb4aea0e4ae9fcd3358bd96720bb5c8\etc
>
> 2、找到一个名为gitconfig的文件，打开它把里面的[remote "origin"]那一行删掉就好了！