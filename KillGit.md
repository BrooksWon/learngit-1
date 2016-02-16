# Git使用说明
#### 初始化一个仓库
- `git init`

#### 添加一个文件到Git仓库
- `git add`

#### 把文件提交到Git仓库
- `git commit -m "提交信息"`

#### 查看仓库状态
- `git status`

#### 查看文件修改内容
- `git diff`

#### (版本回退)在历史版本中穿梭，HEAD指向的版本就是当前版本
- `git reset --hard coommit_id`

#### 查看提交历史
- `git log`

#### 查看命令历史
- `git reflog`

#### 工作区和暂存区
- 我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

#### 管理修改
- Git跟踪并管理的是修改，而非文件。
- 每次修改，如果不 `add` 到暂存区，那就不会加入到 `commmit` 中。

#### 撤销修改
- 场景1：当你改乱了工作区的某个文件的内容，想直接丢弃工作的修改时，用命令 `git checkout -- file`.
- 场景2：当你不但改乱了工作某个文件的内容，还添加到了暂存区时，修丢弃修改，分两步，第一步用命令 `git reset HEAD file` ，就会到了场景1，第二部按场景1操作，
- 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考 `版本回退` 一节，不过前提是没有推送到远程库。

#### 删除文件
- `git rm`


#### 创建SSH KEY
- `ssh-keygen -t rsa -C "youremail@example.com"`

#### 添加远程库
- `git remote add origin git@github.com:BrooksWon/TestGitDemo.git`

#### 从远程库克隆
- `git clone git@github.com:BrooksWon/TestGitDemo.git`


## 分支管理


#### 查看分支
- `git branch`

#### 创建分支
- `git branch <name>`

#### 切换分支
- `git checkout <name>`

#### 创建＋切换分支
- `git checkout -b <name>`

#### 合并某分支到当前分支
- `git merge <name>`

#### 删除分支
- `git branch -d <name>`


#### 解决冲突
- 当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。用 `git log --graph` 命令可以看到分支合并图。

#### 分支管理策略
- 合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward 合并久看不出来曾经做过合并。

#### Bug分支
- 当修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；当熟透工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场。

#### feature分支
- 开发一个新feature，最好新建一个分支；如果要丢弃一个没有被合并过的分支，可以通过`git branch －D <name>`强行删除。

#### 多人协作
##### 推送分支
- master分支是主分支，因此要时刻与远程同步；
- dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，没必要推送到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推送到远程，取决于你是否和你的小伙伴合作在上面开发。

##### 抓取分支
多人协作时，大家都会往`master` 和 `dev`分支上推送各自的修改。
- 使用`git clone <项目地址>`克隆远程项目到本地；
- 使用`git checkout -b dev origin/dev`创建远程origin的dev分支到本地;
- 使用`git branch --set-upstream dev origin/dev`设置本地分支`dev`和远程分支`origin/dev`的链接

###### 因此，多人协作的工作模式通常是这样：
1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 如果没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！
注意：如果`git pull`提示“no tracking infomation”，则说明本地分支和远程分支的连接关系没有创建，用命令`git branch --set-upstream <branch-name> origin/<branch-name>`。

###### 小结
- 查看远程库信息，使用`git remote -v`；
- 本地新建分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin <branch-name>`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b <branch-name> origin/<branch-name>`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream <branch-name> origin/<branch-name>`;
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。


## 标签管理
发布一个版本时，我们通常现在版本库中打一个标签，这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。
<br>
Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像，但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

#### 创建标签
- 命令`git tag <name>`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；
- `git tag -a <tagname> -m "blablabla..."`可以指定标签信息；
- 命令`git tag`可以查看所有标签；
- `git show <tagname>`查看标签说明信息。

#### 操作标签
- 命令`git push origin <tagname>`可以推送一个本地标签；
- 命令`git push origin --tags`可以推送全部为推送过的本地标签；
- 命令`git tag -d <tagname>`可以删除一个本地标签；
- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。