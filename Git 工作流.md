# 				**Git 工作流**

> ## 前言
>
> 本工作流适合大部分的情况，但绝不是唯一，请根据自己的喜好来决定是否使用
>
> 内容灵感来源于b站up：码农高天
>
> 若有侵权 告知即删

​	自己创建仓库的时候可以看到：

![image-20240812084349055](/home/lewis/Documents/Typora/Git 工作流.assets/image-20240812084349055.png)

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

## 更新、添加、提交和推送变更

​	当我们写好Bug以后，我们可以通过一些列的指令来查看我们的更改，提交到Local区，然后推送到Remote端

```shell
git diff
git add <some-file>
git commit
git push origin new-branch
```



​	以上就是我们最常接触到的内容，但是当你和别人协作开发的时候，这些就不够用了，你总不能别人上传他的代码后，你还去`clone`下来，然后再把它的东西复制到你的代码里面，那就完全不需要使用Git就能完成这些工作了

## 拉取请求

​	当我们的任务开发完毕后，或者别人的`feature`写完了你需要放在你自己的代码上跑，这个时候，我们就需要去`Github`上看看有没有最新的提交，首先我们要切换到`main`分支

```shell
git checkout main
```

​	此时由于我们还没有拉取Remote端的内容，所以不会有任何更改在main上，所以我们拉取main分支

```shell
git pull origin main
```

​	我们再切换到`new-branch`分支（为了保证`main`分支永远都是可以直接运行的状态，所以我们得在`new-branch`中检查新增内容会不会对我们的代码造成什么Bug）在使用`rebase`合并`main`分支和`new-branch`分支

```shell
git checkout <new-branch>
git rebase main
```

![image-20240812225732363](/home/lewis/Documents/Typora/Git 工作流.assets/image-20240812225732363.png)

​	`rebase` ：git会从`new-branch`和`main`的共同祖先开始提取`new-branch`分支上的修改，然后将`new-branch`分支指向`main`分支的最新提交，进行冲突处理

## 提交新请求

​	由于我们本地已经合并好了`new-branch`和`main`分支，但是Remote端还未进行同步，所以我们需要将合并好的推送给Remote端

```shell
# -f 表示强制推送，和rebase配合使用，由于这个branch是你自己开发的，所以不会和别人的提交起冲突。请注意，如何你和别人配合使用一个branch的时候一定要保证他没有进行任何推送代码
git push -f origin <new-branch>
```

## 合并分支

​	当我们的`new-branch`已经完成他的功能的时候，我们就可以将其和`main`分支合并了，这里我演示的是在`Github`端进行的操作，本地类似，但是很多情况下，你都不是项目的管理员，无法直接推送到`main`

​	在Remote仓库中找到**Pull requests**，点击**New pull request**，选择对应的分支后，**Create pull request** 等待仓库管理员的同意

![image-20240812233422941](/home/lewis/Documents/Typora/Git 工作流.assets/image-20240812233422941.png)

​	此时我们的合并任务就完成了！

## 更新本地分支

​	当仓库管理员同意该合并请求后，我们需要将合并后的`main`分支更新到Local端，如果该bug已经写完，我们可以选择删除`new-branch`(当然我觉得删不删都无所谓了，毕竟远程仓库那段你可以不删)

```shell
git checkout main
git pull origin main
# 删除该分支
git branch -D <new-branch>
```

## 结束

到此为止Git工作流就已经介绍完毕了，我这再介绍一些常用的branch命名规范

### 分支命名规范

1. 主分支（Master/Main Branch）: 主分支是代码库的主要分支，通常用于部署到生产环境。主分支的命名可以使用”master”或”main”。

2. 开发分支（Develop Branch）: 开发分支是从主分支分离出来的，用于进行日常开发工作的分支。开发分支的命名可以使用”develop”。

3. 功能分支（Feature Branch）: 功能分支是用于开发某个特定功能的分支。功能分支应该从开发分支分离出来，并且在功能开发完成后合并回开发分支。功能分支的命名可以使用”feature/”前缀，后面跟上具体的功能名称。例如，”feature/login-page”表示用于开发登录页面的功能分支。

4. 修复分支（Bugfix Branch）: 修复分支是用于修复bug的分支。修复分支应该从主分支分离出来，并且在修复完成后合并回主分支。修复分支的命名可以使用”bugfix/”前缀，后面跟上具体的修复内容。例如，”bugfix/fix-login-bug”表示用于修复登录bug的修复分支。

5. 发布分支（Release Branch）: 发布分支是用于发布软件版本的分支。发布分支应该从开发分支分离出来，并且经过测试后合并回主分支。发布分支的命名可以使用”release/”前缀，后面跟上版本号。例如，”release/v1.0″表示用于发布1.0版本的发布分支。

需要注意的是，分支的命名应该尽量简洁明了，能够清楚地表示分支的用途和内容。另外，为了避免冲突，分支的命名应该使用小写字母和短划线（-），避免使用空格和特殊字符。

### 提交信息规范

**commit message格式：**

```shell
<type>(<scope>): <subject>
```

type(必须)：用于说明git commit的类别，只允许使用下面的标识

- feat：新功能（feature）
- fix/to：修复bug，可以是QA发现的BUG，也可以是研发自己发现的BUG。
- fix：产生diff并自动修复此问题。适合于一次提交直接修复问题
- to：只产生diff不自动修复此问题。适合于多次提交。最终修复问题提交时使用fix
- docs：文档（documentation）
- style：格式（不影响代码运行的变动）
- refactor：重构（即不是新增功能，也不是修改bug的代码变动）
- perf：优化相关，比如提升性能、体验
- test：增加测试
- chore：构建过程或辅助工具的变动
- revert：回滚到上一个版本
- merge：代码合并
- sync：同步主线或分支的Bug

scope(可选)：scope用于说明 commit 影响的范围

subject(必须)：subject是commit目的的简短描述，不超过50个字符。

