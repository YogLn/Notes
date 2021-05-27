### 1.Github

仓库(Repository):项目，一个项目对应一个仓库,通过git管理

收藏(Star):

复制克隆项目(Fork):该fork项目是独立存在

发起请求(PullRequest):

关注(Watch):

事务卡片(Lssue):

### 2.添加仓库文件

- 向仓库中添加文件流程

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200902221929214.png" alt="image-20200902221929214" style="zoom:67%; margin-left:20px" />

- 设置用户名和邮箱

  $ git config --global user.name '用户名'

  $ git config --global user.email '邮箱'

  该设置在github仓库显示谁提交了该文件

- 初始化一个新的Git仓库

  - 新建一个文件夹

    $ mkdir 文件名

    `$ mkdir test`

  - 在文件内初始化Git(创建Git仓库)

    `$ cd test`   进入文件夹

    `$ git init`   初始化文件

- 向仓库中添加文件

  `$ touch a1.php`   创建一个文件

  `$ git status`   查看状态

- 将文件从工作区提交到暂存区

  `$ git add a1.php`    将文件从工作区提交到暂存区

  `$ git status`   查看状态

- 把暂存区的文件提交到仓库

  `$ git commit -m 'add a1.php'`

  `$ git status`   查看状态

### 3.修改仓库文件

- 修改文件

  `$ vi a1.php`

  输入添加的东西

  Esc+:wq退出

  `$ cat a1.php`    查看内容

- 添加到暂存区

  `$ git add a1.php`    将文件从工作区添加到暂存区

- 把暂存区的文件提交到仓库

  `$ git commit -m '提交描述'`

### 4.删除仓库文件

- 删除文件

  `$ rm -rf a1.php`

- 从Git中删除文件

  `$ git rm a1.php`

- 提交操作

  `git commit -m '提交描述‘`

### 5.git远程管理仓库

`git push`:将本地仓库提交到远端。

`git clone 仓库地址`

### 6.Github Pages搭建网站

###### 1.个人站点 

访问：http://用户名.github.io

**搭建步骤**

1. 创建个人站点 -> 新建仓库（注：仓库名必须是【用户名.githun.io】)
2. 在仓库下新建index.html的文件即可

注意：github pages 仅支持劲太网页

​			仓库里面的只能是.html文件

###### 2.Project Pages 项目站点

http://用户名.github.io/仓库名

**搭建步骤**

1. 进入项目主页，点击settings
2. 点击[Launch automatic page gennerator] 来自动生成主题页面



### 7.实操

将文件上传到github[教程](https://blog.csdn.net/geerniya/article/details/79552247)

**windows下上传文件夹**

接下来我们回到git bash的命令窗口，由于我们是在文件夹上右键打开的，因此已经定位到该文

件夹目录了。可以通过`pwd`命令查看文件夹位置：

```
pwd #查看文件夹位置
```

然后，将该文件夹变成Git可以管理的仓库：

```
git init
```

我们可以通过ls命令查看文件夹中的内容：

```
ls #查看文件夹中的内容
```

然后通过`git add`将所有文件提交到暂存区：

```
git add .
```

由于是第一次提交，需要将所有文件都进行提交，如果一个一个的提交太麻烦，通过`.` 命令可以将所有文件都进行提交。

再然后，`git commit -m '说明'`提交到版本库中即可。

```
git commit -m 'the initial edition'
```

这样我们便在本地建立好了仓库，接下来需要将本地仓库与GitHub网站的仓库进行关联。

```
git remote add origin https://github.com/YogLn/ESNote.git
```

出现错误可先移除再进行添加

```
git remote rm origin
```

后面的网址是我们刚才在GitHub网站上建立的仓库位置，可以从网站上进行复制

在将本地仓库与GitHub网站上的仓库进行关联后，便可进行推送了，但是在第一次进行推送时，需要注意的是，GitHub网站上的仓库并非是空的，我们在创建时创建了一个README文档，因此需要将两者进行合并才行。

```
git pull --rebase origin master
```

最后，在进行推送即可。

```
git push -u origin master
```

这个带有-u这个参数是指，将master分支的所有内容都提交，第一次关联之后后边你再提交就可以不用这个参数了，之后你的每一次修改，你就可以只将你修改push就好了。

```
git push origin master
```

回到GitHub网站刷新下我们的MxShop2仓库，便可看到已经将windows上文件夹的内容全部同步过来了。

**4、定期维护**

在完成第一次上传后，之后在本地做的修改，都可以通过如下命令进行同步。

```git
git add -A               #将文件的修改上传到暂存区
git commit -m '说明'      #提交到本地仓库
git push origin master   #推送到GitHub网站上
```

