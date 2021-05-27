DNF即Dandified YUM，基于RPM， Linux发行版下一代软件包管理工具。它首先在Fedora 18中出现，并且在最近发行的Fedora 22中替代YUM工具集。CentOS 和 OpenSUSE等Linux发行版也可以使用。

yum已经有些老旧，不堪大用。

下文将介绍怎么安装DNF软件包管理器，默认已经切换到root 用户，如果没有切换，请加上 sudo命令。（本文安装最新版本centos 7.2并已经更新最新版本补丁。）

```
 yum install epel-release
```

或者

```
 yum install epel-release -y
```

-y命令可以不显示详细安装过程。

安装进度条完成后就可以使用命令安装DNF。

```
yum install dnf
```

首先检查DNF软件版本。

```
dnf --version
```

列举dnf 软件库。

```
dnf repolist
```

列举过程可能需要稍等一下，取决于计算机或网络配置。dnf repolist all是列举所有安装源，其中包括未能使用安装源。

```
dnf list 
```

这个命令可以显示所有软件列表，包括未安装软件包。显示已安装软件可用命令

```
dnf list installed
```

搜索软件包可用

```
dnf search vim
```

vim是示例软件名称，可以替换为其他软件名称。

安装软件包和yum很相似。

```
dnf install vim
```

卸载软件可用

```
dnf remove vim 
```

升级系统和软件分别为

```
dnf update systemd
dnf update 或 # dnf upgrade
```

删除未使用软件包请用

```
dnf autoremove
```

这一点和apt有些类似。Ubuntu系列只需要将dnf修改为apt即可。

当然还有安装软件组等等功能，可以查看

```
man dnf
```

帮助。

提示：安装软件可能会报错（类似）。

```
Extra Packages for Enterprise Linux 7 - x86_64  668 kB/s | 9.4 MB     00:14
Failed to open: /var/cache/dnf/x86_64/7/x86_64/7/epel/repodata/e8...19-updateinfo.xml.bz2.
```

需要执行命令。

```
yum remove epel-release
```

再执行命令

```
dnf repolist
dnf list installed
```

即可解决报错问题。