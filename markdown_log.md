t是什么？

- Git是`分布式版本控制系统`。而集中式的版本控制系统不但速度慢，而且必须联网才能使用。
- 2008年，GitHub网站上线（程序员社区网站，这网站上可以托管自己的网站库，基于Git完成的）

> ###Git的作用

- 备份文件
- 记录历史
- 回到过去
- 多端共享
- 团队协作

> ###Git的诞生

Linus创建了Linux（1991年），由于很多人提交代码，都是最后都是通过手工方式合并的（1991年-2002年）。最后很多人都有意见，于是Linus选择了（2002年）一个商业的版本控制系统BitKeeper，但是开发Samba的Andrew`试图破解`BitKeeper的协议（2005年4月），于是BitMover公司怒了，要收回Linux社区的免费使用权。于是Linus花了两周时间自己用C写了一个`分布式版本控制系统`，这就是Git！（2005年4月初发布）

> ###分布式和集中式的区别

- 集中式版本控制系统最大的毛病就是必须联网才能工作，如果`网速慢`就很悲剧、如果不联网就没有办法提交，查看记录。
- 分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库，就不需要联网了
- 你的同事也在他的电脑上改了文件A，这时，你们俩之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。
- 分布式版本控制系统的安全性要高很多，因为每个人电脑里都有完整的版本库，某一个人的电脑坏掉了不要紧
- 而集中式版本控制系统的中央服务器要是出了问题，所有人都没法干活了。
- Git极其强大的分支管理

在实际使用分布式版本控制系统的时候，其实很少在两人之间的电脑上推送版本库的修改，因为可能你们俩不在一个局域网内，两台电脑互相访问不了，也可能今天你的同事病了，他的电脑压根没有开机。因此，分布式版本控制系统通常也有一台充当“中央服务器”的电脑，但这个服务器的作用仅仅是用来方便“交换”大家的修改，没有它大家也一样干活，只是交换修改不方便而已。

> ###安装完成后（安装过程自行查阅），设置用户名和密码

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

如果要查看全局参数就输入 `git config --global --list`
查看Git版本`git --version`


---


> ###创建版本库

1. 进入某个目录
2. 使用 `git init`把这个目录变成版本库，这时目录下会多出一个.git的隐藏目录，千万不要修改这个目录的东西，所有Git的记录都在里面

> ###把文件添加到版本库

1. 先修改一个文件a.php
2. 使用`git add a.php`（把这个文件添加到暂存区）
3. 使用`git commit -m '增加了xxx函数'`（把文件提交到当前分支）

每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为commit。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个`commit`恢复

> ###状态查看

- `git status`查看当前状态
- `git diff a.php` 查看没有add的a.php和已add的a.php的区别（如果文件已经add到暂存区则diff无法查看）

> ###版本退回

在退回到某个commit快照版本之前，我们必须要知道那个版本的ID号

在Git中，用`HEAD`表示当前版本，也就是最新的提交`3628164...882e1e0`，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个^比较容易数不过来，所以写成`HEAD~100`

- 如果我们commit（快照、提交到当前分支）很多次文件，那么肯定记不住哪次提交修改了哪些内容，这时候就可以通过`git log`命令来查看（倒序显示，最新的修改在最前面），如果嫌输出的信息太多，我们可以使用`git log --pretty=oneline`来简洁查看，输出的一大串字符串是`commit id`也就是`版本号`
- 既然我们知道了版本号，那么如何退回到指定的版本呢？我们可以通过`git reset --hard 3628164`命令来退回到指定的版本（注意：这里的`--hard`的意思是版本号`指针`)，版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。`OK，我们现在已经可以回到过去！`
- 既然我们回到了过去，但是又后悔了，想回到原来的地方，但是，这时候使用`git log`发现，这时候已经看不到未来的版本号了，当前的版本号是最新的！那么这样就意味着，找不到未来的版本号了？找不到未来的版本号也就意味着回不去了！那么怎么办呢？Git提供了一个命令`git reflog`用来记录你的每一次命令，也就是说：你可以通过`git reflog`命令找到未来的版本号，用于回到未来！

```
也就是说

现在有3个版本

版本号：333

版本号：222

版本号：111

我现在用git reset —hard 222 退回到222版本，

git log 后 就看不到333版本了

必须要用git relog才能看到222之前的333版本号

然后在通过git reset —hard 333 恢复到333

```



> ###撤销修改

我在文件a.php添加了一行“我的傻逼老板任然喜欢SVN”。突然发现，如果这么写本老板看见了，就完蛋啦。

这时候查看`git status` 说明可以用`git checkout -- a.php`来放弃工作区修改。



```

这里有两种情况：
一种是a.php自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
一种是a.php已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
总之，就是让这个文件回到最近一次git commit或git add时的状态。
```

如果你已经把不该说的话添加到了暂存区，那么查看`git status` 说明可以用`git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重新放回工作区

如果已经提交了，则只能用版本退回！



> ###删除文件



我现在添加一了个conf.php文件，但是写完这个文件并提交commit后，发现不需要了，就把它在工作区删掉。这时候版本库的文件和工作区的文件就不一样了（工作区多了一个conf.php文件，而版本库还没删掉），那么就要把它从版本库删掉，用：`git rm conf.php`，并且把这一次的删除，提交到版本库，这样工作区和版本库就一致了。

如果还没有commit之前（但是已经提交到暂存区），发现，这时候我又需要这个文件了怎么办呢？那么就要用：`git reset HEAD <file>`（从暂存区恢复）在用`git checkout -- <file>`（用版本库里的版本替换工作区的版本）恢复



> ###创建、合并分支



1. 创建debug分支`git branch debug`在切换到debug分支`git checkout debug` ，也可以一句话简写`git checkout -b debug`，-b参数表示创建并切换

2. 用`git branch`查看当前分支，命令会列出所有分支，当前分支前面会标一个*号

3. 修改当前分支文件，并commit

4. 在切换到master分支：`git checkout master`，这时候，查看debug分支修改的内容，是查不到的，因为不是在当前的master分支下修改的，那么我们就要合并分支

5. 合并分支：`git merge debug` 注意！！！这里合并分支方式是`Fast forward`模式，但这种模式下，`删除分支后，会丢掉分支信息！`我们要禁用这`Fast forward`模式，加上`--no-ff`参数就可以用`普通模式`合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。必须要用：`git merge --no-ff -m "merge with no-ff" debug` 这样来合并分支，因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。

6. 合并完成后，我们就可以删掉debug分支了：`git branch -d debug`

7. 然后我们在查看分支`git branch`，debug分支果然被删除了

8. 用`git log --graph --pretty=oneline`分支合并图



> ###修复BUG的stash

- 如果你正在dev分支干活，这时候接到一个紧急修复BUG的任务，但是你的dev分支工作只进行到一半，还没法提交，幸好，Git还提供了一个`git stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作。

- 用`git stash list`命令查看储存的工作区，一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除； 另一种方式是用`git stash pop`，恢复的同时把stash内容也删了：









