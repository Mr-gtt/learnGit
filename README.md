

# Git 的使用

*仅用于个人学习使用，如果侵犯到您的权益，请联系我删除*

来源：方糖

## Git的安装

教程点这里[Git的安装](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)



## Git 的使用

### 使用终端

打开终端

![tem](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4d8eca87c.png)

### 创建目录

首先，我们需要运行几个命令来创建一个目录，这个目录将作为我们的代码目录。

``` bash
mkdir Code && cd Code
```

![mkdir](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4d8fe623c.png)

***



### 初始化 (git init)

然后运行 git 初始化命令。

``` bash
git init
```

这时候 git 告诉我们它创建了一个空的 git 仓库。

![init](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4d9274023.png)

### 新建文件

让我们新建一个文件，然后把它添加到 git 里边。

``` bash
echo "make git great " > a.txt && git add a.txt
```

![mkdir](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4d93a2c07.png)

### 新建文件

让我们新建一个文件，然后把它添加到 git 里边。

``` bash
echo "make git great " > a.txt && git add a.txt
```

![mkdir](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4d93a2c07.png)

### 添加当前目录到暂存区(git add)

`git add` 命令可以将对应的文件放到 git 暂存区里边。

相反，我们可以运行 `git reset` 命令来将暂存区里边的某个文件移出来。我们来看下实际操作。

![](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee3464eba96.png)

***



让我们通过 cd 命令进到 git 仓库里去看看都有些啥。

``` bash
cd .git && ls -la
```
注意其中 objects 目录就是用来存放文件的全量历史数据的。

![](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4d9703087.png)

进入 objects 目录，我们可以发现下边有一个 61 目录，里边有一个以 ac 开头的文件。

``` bash
cd objects && ls -la
```

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4d9802d42.png)

这里将 61 和这个文件名拼接起来就可以得到我们刚才 add 的文件的 SHA1 。把前两位拿出来作为目录名，这是为了避免在同一个目录里存放太多文件，导致文件索引过大而降低性能。



我们来确认下文件内容，通过 git 的 cat-file 命令，我们可以查看存储到 objects 目录下的所有对象。

``` bash
git cat-file -p 61ac375fc5565c8a763d942dc599fd34128bdb37
```
可以看到「 make git great 」字样，这的确是我们刚才的文件。

![](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4d98f0b87.png)

而这个「 index 」文件就是我们之前提到的「暂存区」。我们可以通过「 git ls-files --stage 」命令来查看它的内容。

``` bash
git ls-files --stage
```
![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4d9b76eb1.png)

可以看到， a.txt 这个文件名和它当前对应的 SHA1 值都被记录在了暂存区中。我们通过动图来看一下。

![](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee3759bae11.png)

***

「 HEAD 」文件描述了当前分支上 Head 的位置。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4d9c82ec8.png)

通过「 cat 」命令我们可以查看到 HEAD 文件里边其实放的是一个引用地址。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4d9d904a3.png)

而这个引用指向的地方是不存在的。这是因为，我们还没有进行过 commit 。运行 「 cd .. 」命令返回上级目录，然后我们来将暂存区的文件提交到本地仓库。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4d9e96f03.png)

***

### 提交(git commit)

这里我们会用到 git commit 命令。它附带一个参数 m ，用来描述这一次的提交。

运行命令后，我们会得到一个错误。这是因为我们前边讲过， commit 里边还会包含提交人的信息。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4da09a420.png)

这里的错误就是因为 git 命令行没有能成功的自动获取到 email 地址。按照提示，我们通过 git config 来设置提交人名字和 email 地址。

``` bash
git config --global user.email "easychen@gmail.com"
git config --global user.name "Easy"
```

补全信息后，再提交，我们就可以看到成功信息了。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4da1ef895.png)

这个时候我们再来看一下 HEAD 里边的那个引用

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4da2e3a9e.png)

可以看到，现在这个引用就指向了一个提交。而这个提交里边，正是我们刚才的内容。

**有时候我们修改的文件比较多，可能会忘了哪些需要 `add `。这时候，我们可以通过 `git status` 命令来查看。**

比如我们修改了 b.txt ，但是并没有 add 它。

```bash
echo "b" > /data1/Code/b.txt
```

那么我们运行 git status 时就能看到下图的提示。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4da589a44.png)

***

### 分支(git branch )

下边我们来看看分支命令。 首先我们从 master 里边分支出 dev 分支和 ops 分支，分别为它们添加两句不同的话。最后我们再把这两个分支合并到主干上。

首先我们通过 `git branch `来创建新分支。

然后再通过` git checkout` 来切换到这个新分支上。

```bash
git branch dev && git checkout dev
```

切换过来以后，我们就可以进行编辑了。这里我们给 a.txt 原文之上加上了「 make dev great again 」。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4daa87e0e.png)

编辑完成以后，我们先通过 add 将其加入暂存区，再通过 commit 放入仓库。

```bash
git add a.txt && git commit -m 'dev'
```

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4daba1f0f.png)

然后，我们 checkout 回 master 。再按之前的操作，创建一个 ops 分支。

```bash
git checkout master && git branch ops && git checkout ops
```

然后我们对 a.txt 进行修改，在原文后边加上「 make ops great again 」。

```bash
git add a.txt && git commit -m 'ops'
```

现在我们两个分支就准备好了。接下来我们就可以来合并分支了。

***

### 合并(git merge)

合并分支是通过 `git merge` 来完成的。

先 checkout 回 master ，然后运行 merge 命令。

```bash
git checkout master && git merge dev
```

可以看到，这次合并很成功，它是一次「快速合并」，它在我们的 a.txt 中添加了一行。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4daf8f003.png)

再次运行 merge 命令，将 ops 分支合并进来。

```bash
git merge ops
```

可以看到，这是一次三方合并。而且很幸运的， a.txt 完成了自动合并。 终端进入了注释补全，按 `alt+o` 存盘，回车以后再按 `alt+x` 退出即可。

之所以能自动合并，这是因为 a.txt 在 dev 分支和 ops 分支的新增内容分别在「 make git great 」的上边和下边。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4db1bf088.png)

通过简单的算法就可以进行合并。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4db35bd10.png)

那如果 a.txt 在 dev 和 ops 分支的新增内容，都在「 make git great 」之下会怎么样呢。 我们来合并试试，首先 checkout 回 dev 。

```bash
git checkout dev
```

我们修改下 a.txt ，将 make dev great again 从第一行换到最后一行。 然后提交到 Git 。

```bash
git add a.txt && git commit -m 'update a.txt'
```

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4db579480.png)

然后我们再来合并试试。

```bash
git checkout master && git merge dev && git merge ops
```

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4db66af81.png)

可以看到，这次自动合并就失败了。这时候我们有两个选择。一是放弃合并，这时候我们运行「 git merge --abort 」，就可以恢复到 merge 之前的状态。另一个选择就是我们手工来处理冲突。

在编辑器中， VSCode 已经帮我们用箭头把两个版本的变动标记出来了。这时候，我们可以点击「 Accept Both Changes 」接受两个修改；或者直接手工重新组织内容。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4db7850f0.png)

接下来，我们 `git add` 刚才解决冲突的 a.txt ，然后进行 commit 提交。

```bash
git add a.txt && git commit -m 'merged'
```

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4db934c86.png)

查看日志可以看到，刚才的 merge 提交已经完成了。这就是我们手工解决冲突的过程。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4dbb6aa01.png)

我们通过动图来复习下，同时看看在纯命令行下有何不同。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee44227c72b.png)

***

### 显示提交记录(git log)

刚才我们是通过 `git log` 命令来查看日志的，它可以查到每次提交的记录。运行后会进入互动模式，按上下键可以滚动，按 `q` 键可以退出。

查看结果类似这个样子。在 commit 字样后边，有这次提交的 SHA1 值。运行 git checkout SHA1 值，我们就可以回到那次提交时的历史。你也可以在 checkout 后边加上 -b 参数来将其检出为一个全新的分支。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4dbef3a52.png)

***

### 远程操作

下边我们来看看 git 如何和服务器端的仓库互动。首先我们要把本地的仓库和服务器端的仓库关联起来。 这里有两种情况，一种是本地没有仓库，那么我们直接用 `git clone` 就好了。



### 克隆远程仓库 (git clone repo)

clone 命令后边的参数，是远程仓库的地址。

这个地址是由远程平台提供的。如果我们使用 github ，在仓库页面有一个「 clone or download 」按钮，点开就可以看到一个 URL 。这个 URL 就是仓库地址。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4dc1d483a.png)

如果你用的是 gitlab ，那么在项目页面右侧，也有一个 clone 按钮。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4dc2db789.png)

我们来运行下 git clone 命令。

```bash
cd /data1/Code && git clone https://github.com/easychen/litephp.git
```

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4dc439739.png)

可以看到我们成功的复制了服务器上的仓库。那这个本地仓库是否和服务器端的仓库关联呢，我们来看看。

### 远程仓库列表 (git remote)

通过 `git remote` 命令，我们可以查看到所有关联到当前仓库的远程仓库。

我们就来看看刚才 clone 下来的 repo 的远程仓库是如何设置的。

```bash
cd /data1/Code/litephp && git remote
```

可以看到，本地仓库和一个叫做「 origin 」的远程仓库关联。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4dc872fab.png)



### 显示远程仓库信息(git remote show)

通过 `git remote show` 命令，我们可以查看某一个远程仓库的详细信息。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4dcac741b.png)

可以看到，这些信息包括了 Fetch URL 和 Push URL 。通过 `git push` 和 `git pull` 命令，我们可以从服务器端推送和拉取代码。

### 关联远程仓库 (git remote add)

**还有一种经常遇到的情况是，我们在本地已经有代码仓库了，想要把它发布到 github 之类的平台上。我们会先在 github 上创建一个空的项目，得到远程仓库地址。然后把它和本地仓库关联起来。**

这时候我们会用到 `git remote add` 命令。

这个命令接收两个参数。一个是别名，另一个是远程仓库 URL 。

我们用 `git remote remove` 把 origin 给删掉，然后将其重新添加为 origin2 。

```bash
git remote remove origin && git remote add origin2 https://github.com/easychen/litephp.git && git remote show origin2
```

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4dce39c3c.png)

### 拉取远程仓库到本地(git pull)

然后我们通过 `git pull` 将远程仓库的代码拉取到本地。 pull 命令接收两个参数，一个是我们 remote add 时指定的 name ，一个是远程仓库的 branch 。

于是我们输入 git pull origin2 master ，可以看到，本地的代码是最新的。

```bash
git pull origin2 master
```



![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4dd041915.png)

### 推送本地到远程仓库(git push)

当我们在本地完成修改后，可以通过 git push 命令来推送到远程仓库。注意 push 是写入操作，需要对远程仓库有写入权限，这里我们就不演示了。

### 暂存当前修改到储藏区(git stash)

最后补充一个命令， `git stash` 。这个命令可以将你当前进行到一半的工作保存到一个暂存区域，然后将当前目录回滚到上一次提交。

下边来看一个具体例子，我们往 a.txt 中写入一串字符，然后提交它。

```bash
cd /data1/Code/ && echo "Goooood" > a.txt && git add a.txt && git commit -m 'init2'
```

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4dd5c703f.png)

然后我们假装把 a.txt 改坏了。这时候我们当然可以 checkout 前一个 commit ，但这样当前的修改就丢失了。虽然我们认为当前的修改可能是坏的，但可能我们以后需要从里边提取点数据。安全起见，我们希望当前的修改也能保存下来。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4dd6c4dde.png)

这时候我们运行 git stash ，就会自动切回到上一个提交。

```bash
git stash
```

可以看到 a.txt 的内容已经恢复了。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4dd7b508a.png)

但突然我们又觉得之前那个修改挺好的，想把它找回来。这时候我们运行 git stash apply ， git 就会自动把之前放到储藏区的最新的那个修改切回来。

```bash
git stash apply
```

可以看到 a.txt 的内容已经回来了。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4dd8becba.png)

运行 git stash list 命令，可以把所有放到储藏区的修改都列出来。

```bash
git stash list
```

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4dda1f892.png)

注意这里有个编号，通过指定编号，我们就可以对其中一个修改进行显示（ show ）、删除（ drop ）。也可以用 clear 命令将储藏区全部清除。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4ddb1d827.png)

我们来看下动图。

![img](https://sj-img.ftqq.com/cic/u1/2019.12.09.5dee4aa7372a1.png)

这个命令用熟了以后非常顺手，实在是程序员们对付多变的产品经理、设计师们应付蛋疼的甲方时的好命令。