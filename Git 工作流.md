# 				**Git 工作流**

> ## 前言

​	自己创建仓库的时候可以看到：

![image-20240812084349055](/home/lewis/.config/Typora/typora-user-images/image-20240812084349055.png)

​	当我们第一次push git上远程仓库的时候我们一般会执行：

```shell
echo "# git" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:dioff/git.git
git push -u origin main
```

​	若第一次上传以后就可以直接使用：

```shell
git remote add origin git@github.com:dioff/git.git
git branch -M main
git push -u origin main
```



​	但是一般来说，我们的工作流程是Fork别人的代码进自己的仓库，然后`clone`下来，所以今天我们来介绍一下Git工作流

​	（Git如何下载，怎么配置等基础概念这里不会介绍）

## 拉取仓库

这个操作想必大家都不陌生 

```shell
git clone git@github.com:xxx/xxx.git
```

​	它会将远程仓库的内容下载到我们本地，由于是别人`git init`的项目，所以在我们下载到本地的时候，git本地仓库也就已经存在了，我称之为Local态

## 开发新功能

​	当我们要开发项目的新功能，对代码进行改进的时候。我们首先要遵循不破坏之前写好的功能，所以我们要将main分支切换到我们自己的新分支

```shell
# -b 参数，该参数可作为一种便捷的方法，用于创建新分支并立即切换到该分支
# 新版本中不需要-b 也可以执行相应的操作
git checkout -b ＜new-branch＞
```

### 更新、添加、提交和推送变更

​	当我们写好Bug以后，我们可以通过一些列的指令来查看我们的更改，提交到Local区，然后推送到Remote端

```shell
git diff
git add <some-file>
git commit
git push origin new-branch
```



​	以上就是我们最常接触到的内容，但是当你和别人协作开发的时候，这些就不够用了，你总不能别人上传他的代码后，你还去`clone`下来，然后再把它的东西复制到你的代码里面，那就完全不需要使用Git就能完成这些工作了

