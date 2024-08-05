#### GIT

##### 常用指令

初始化一个Git仓库，使用`git init`命令。

添加文件到Git仓库，分两步：

1. 使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；
2. 使用命令`git commit -m <message>`，完成。

- 要随时掌握工作区的状态，使用`git status`命令。

- 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。

- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。

- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

  

- `git checkout -- file`可以**丢弃工作区的修改**

  `git restore <file>  to discard changes in working directory`

- `git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区

  `use "git restore --staged <file>..." to unstage`

- `git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。

  

- 要**关联一个远程库**，使用命令`git remote add origin git@server-name:path/repo-name.git`；

  关联一个远程库时必须给远程库指定一个名字，`origin`是默认习惯命名；

  关联后，使用命令`git push -u origin master`第一次**推送**master分支的所有内容；

  此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改。

- 克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

  Git支持多种协议，包括`https`，但`ssh`协议速度最快。

  

- **删除远程库**

  如果添加的时候地址写错了，或者就是想删除远程库，可以用`git remote rm <name>`命令。使用前，建议先用`git remote -v`查看远程库信息：

  ```sh
  $ git remote -v
  origin  git@github.com:michaelliao/learn-git.git (fetch)
  origin  git@github.com:michaelliao/learn-git.git (push)
  ```

  然后，根据名字删除，比如删除`origin`：

  ```sh
  $ git remote rm origin
  ```

  此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。

  

- Git鼓励大量**使用分支**：

  查看分支：`git branch`

  创建分支：`git branch <name>`

  切换分支：`git checkout <name>`或者`git switch <name>`

  创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

  **合并某分支到当前分支**：`git merge <name>`

  ```sh
  $ git merge --no-ff -m "merge with no-ff" dev
  ```

  删除分支：`git branch -d <name>`

  

- 当Git无法自动合并分支时，就必须首先**解决冲突**。解决冲突后，再提交，合并完成。

  解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

  用`git log --graph`命令可以看到**分支合并图**。

  ```sh
  $ git log --graph --pretty=oneline --abbrev-commit
  ```

  

- Git分支十分强大，在团队开发中应该充分应用。

  **合并分支时**，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

  ```sh
  $ git merge --no-ff -m "merge with no-ff" dev
  ```

  **修改工作留痕**有修改记录。

  

- 修复bug时，我们会通过创建新的**bug**分支进行修复，然后合并，最后删除；

  只能保存tracked 的文件，新增文件不会被stash

  当手头工作没有完成时，先把**工作现场**`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；完成一个才能继续恢复其他stash。

  在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

  

- 开发一个新feature，最好**新建一个分支**；

  如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

- rebase操作可以把本地未push的分叉提交历史整理成直线；

  ```sh
  $ git rebase
  ```

  rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

  

- **多人协作**的工作模式通常是这样：

  1. 首先，可以试图用`git push origin <branch-name>`**推送自己的修改**；
  2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
  3. 如果合并有冲突，则解决冲突，并在本地提交；
  4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

  如果`git pull`**提示**`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

  这就是多人协作的工作模式，一旦熟悉了，就非常简单。

- 查看远程库信息，使用`git remote -v`；

- 本地新建的分支如果不推送到远程，对其他人就是不可见的；

- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；

- **在本地创建和远程分支对应的分支**(clone之后，本地没有，远程有dev，建立好后再**push**)，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

- **建立本地分支和远程分支的关联**(**git pull**拉不回的情况)，使用`git branch --set-upstream branch-name origin/branch-name`；相当于git merge 其他分支到当前分支。

- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

  

- 命令`git tag <tagname>`用于新建一个**标签**，默认为`HEAD`，也可以指定一个commit id；

  ```sh
  $ git tag v0.9 f52c633
  ```

- 命令`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；

- 命令`git tag`可以查看所有标签。

- 命令`git push origin <tagname>`可以推送一个本地标签；

- 命令`git push origin --tags`可以推送全部未推送过的本地标签；

- 命令`git tag -d <tagname>`可以删除一个本地标签；

- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

  

- 忽略某些文件时，需要编写`.gitignore`；

- `.gitignore`文件本身要放到版本库里，并且可以对`.gitignore`做版本管理！

###### 创建版本库

通过`git init`命令把这个目录变成Git可以管理的仓库：

```
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
解释
```

瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看见。

###### 将文件添加到版本库

第一步，用命令`git add`告诉Git，把文件添加到仓库：

```shell
$ git add readme.txt
```

执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。

第二步，用命令`git commit`告诉Git，把文件提交到仓库：

```shell
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

简单解释一下`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

`git commit`命令执行成功后会告诉你，`1 file changed`：1个文件被改动（我们新添加的readme.txt文件）；`2 insertions`：插入了两行内容（readme.txt有两行内容）。

###### 撤销修改

**丢弃工作区内容**

> `git checkout -- file`可以**丢弃工作区的修改**：
>
> ```sh
> $ git checkout -- readme.txt
> 解释
> ```
>
> 命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：
>
> ​	一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库	一模一样的状态；
>
> ​	一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加	到暂存区后的状态。
>
> 
>
> **等同于**`git restore <file>  to discard changes in working directory`
>
> `git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。

**暂存区回退到工作区**

> `git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区：
>
> ```sh
> $ git reset HEAD readme.txt
> Unstaged changes after reset:
> M	readme.txt
> 解释
> ```
>
> `git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。
>
> 
>
> **等同于**
>
> `use "git restore --staged <file>..." to unstage`



关于git: [Git简介 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/896043488029600/896067008724000)