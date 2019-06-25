## git config

git config --system:系统中对所有用户都普遍适用的配置。

git config --global:用户目录下的配置文件只适用于该用户。

## 用户信息

git config --global user.name "John Doe"

git config --global user.email johndoe@example.com

要检查已有的配置信息，可以使用 `git config --list` 命令。

## git 切换远程分支

git checkout -b dev origin/dev

## 取消已经暂存的文件

git reset HEAD <file>

## 合并分支：

分支提交更多：

![git-br-on-master](https://www.liaoxuefeng.com/files/attachments/919022533080576/0)

切换到master；

git merge dev;

## 解决冲突

分别提交后：

![git-br-feature1](https://www.liaoxuefeng.com/files/attachments/919023000423040/0)

切换到master ：git merge feature1;

修改冲突；

提交后：

![git-br-conflict-merged](https://www.liaoxuefeng.com/files/attachments/919023031831104/0)

切换到feature1：git merge master；

## 多人协作

git push失败，因为别人先push过一个新版本。

因此先git pull，但提示错误，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接。

git branch --set-upstream-to=origin/dev dev

然后pull成功，但是合并有冲突，需要手动解决。

解决后commit，然后push。


## Stash

git stash list

git stash pop

git stash apply stash@{0}

小结:  修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场。




git log --graph  分支合并图

git checkout 切换分支

git branch vv

git add -A

git config --global --edit

git push 和git push origin master(是否checkout到对应分支)；

