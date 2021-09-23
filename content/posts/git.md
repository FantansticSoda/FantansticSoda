---
title: "git"
date: 2020-11-18T16:03:53+08:00
draft: false
---

# git 基础


# git 操作

## 初始化本地仓库

- 使用 git init 初始化当前目录为本地仓库，在当前目录下新建一个.git目录，存放git版本信息。

```
learn$ mkdir TestGit
learn$ cd TestGit/
TestGit$ ll
total 0
drwxrwxrwx 1 root root 4096 Nov 17 22:43 ./
drwxrwxrwx 1 root root 4096 Nov 17 22:43 ../

# 初始化仓库
TestGit$ git init
Initialized empty Git repository in /mnt/d/learn/TestGit/.git/

TestGit$ ll
total 0
drwxrwxrwx 1 root root 4096 Nov 17 22:43 ./
drwxrwxrwx 1 root root 4096 Nov 17 22:43 ../
drwxrwxrwx 1 root root 4096 Nov 17 22:43 .git/

# 查看自动创建的.git目录的结构
TestGit$ tree .git/
.git/
├── HEAD
├── branches
├── config
├── description
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── prepare-commit-msg.sample
│   └── update.sample
├── info
│   └── exclude
├── objects
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags

9 directories, 16 files
```

## 提交修改

- 使用 git add testfile  将工作区文件提交至git暂存区

- 使用 git commit -m "message"  将git暂存区文件提交至本地仓库

```
TestGit$ touch testfile1
TestGit$ echo "hello, git" >> testfile1
TestGit$ cat testfile1
hello, git
TestGit$ echo "hello, me again" >> testfile1
TestGit$ cat testfile1
hello, git
hello, me again

TestGit$ git add testfile1

TestGit$ git commit -m "Add testfile1"
[master (root-commit) bb3b0a1] Add testfile1
 1 file changed, 2 insertions(+)
 create mode 100644 testfile1
 
TestGit$ git status
On branch master
nothing to commit, working tree clean
```

Tips：可以使用git commit -a -m "message" 来合并git add和git commit两步操作。

但是加的-a参数可以将**所有已跟踪文件中的执行修改或删除操作的文件**都提交到本地仓库，即使它们没有经过git add添加到暂存区（新加的文件（即没有被git系统管理的文件）是不能被提交到本地仓库的。）

建议一般不要使用-a参数，正常的提交还是使用git add先将要改动的文件添加到暂存区，再用git commit 提交到本地版本库。

```
TestGit$ git status
On branch master
nothing to commit, working tree clean
TestGit$
TestGit$ ll
total 0
drwxrwxrwx 1 root root 4096 Nov 17 22:50 ./
drwxrwxrwx 1 root root 4096 Nov 17 22:43 ../
drwxrwxrwx 1 root root 4096 Nov 17 22:51 .git/
-rwxrwxrwx 1 root root   42 Nov 17 22:49 testfile1*
-rwxrwxrwx 1 root root    6 Nov 17 22:50 testfile2*
TestGit$
TestGit$ echo "change to testfile1" >> testfile1
TestGit$ echo "change to testfile2" >> testfile2
TestGit$
TestGit$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   testfile1
        modified:   testfile2

no changes added to commit (use "git add" and/or "git commit -a")
TestGit$
TestGit$ git commit -am "update testfile1 and testfile2"
[master 27b45ec] update testfile1 and testfile2
 2 files changed, 2 insertions(+)
TestGit$
TestGit$ git status
On branch master
nothing to commit, working tree clean
```

## 版本回退

- 使用git log查看提交操作的历史记录
- 使用git reglog查看所有的历史操作记录
- 使用git reset --hard commit id来回退至指定的版本

**git log**

使用git log可以查看到每次commit的author和date，--pretty=oneline则仅展示commit id和message。

```
TestGit$ git log --pretty=oneline
27b45eca24afb6c7828bfef94b27fed12ed58a00 (HEAD -> master) update testfile1 and testfile2
d643e5c4235eb55d32bbe5a51a16e17aacee52b4 Add testfile2
f70d613610dd1906426550b573a3ef76361e8ecd Update testfile1
bb3b0a1a26d561cacc9b227e0b68f059beafb9a1 Add testfile1
```

前面的一串数字为sha1算法计算得来的commit id，是每一个版本的唯一标识，若需要回退时则需要使用，后面的则为commit时的message信息。**HEAD代表为当前版本。**

**git reflog**

git reflog将记录所有的操作记录，所有版本提交的commit id可以通过此找回。

```
TestGit$ git reflog
27b45ec (HEAD -> master) HEAD@{0}: commit: update testfile1 and testfile2
d643e5c HEAD@{1}: commit: Add testfile2
f70d613 HEAD@{2}: commit: Update testfile1
bb3b0a1 HEAD@{3}: commit (initial): Add testfile1
```

**git reset**

git reset [--hard|soft|mixed|merge|keep] [<commit>或HEAD]：将当前的分支重设(reset)到指定的<commit>或者HEAD(默认，如果不显示指定<commit>，默认是HEAD，即最新的一次提交)，并且根据[mode]有可能更新索引和工作目录。mode的取值可以是hard、soft、mixed、merged、keep。

--hard：重设(reset) 索引和工作目录，自从<commit>以来在工作目录中的任何改变都被丢弃，并把HEAD指向<commit>。

TODO 

**hard和soft的区别**

```
TestGit$ cat testfile1
hello, git
hello, me again
yes, it's me!
change to testfile1
TestGit$
TestGit$ git log --pretty=oneline
27b45eca24afb6c7828bfef94b27fed12ed58a00 (HEAD -> master) update testfile1 and testfile2
d643e5c4235eb55d32bbe5a51a16e17aacee52b4 Add testfile2
f70d613610dd1906426550b573a3ef76361e8ecd Update testfile1
bb3b0a1a26d561cacc9b227e0b68f059beafb9a1 Add testfile1
TestGit$
TestGit$ git reset --hard f70d61
HEAD is now at f70d613 Update testfile1
TestGit$
TestGit$ cat testfile1
hello, git
hello, me again
yes, it's me!
```

## 撤销修改

- 使用git checkout filename，将工作区文件回到最近一次add 或者 commit的状态

撤销修改分为三种情况： 

### 未提交至暂存区

1. 使用git checkout filename 将master的文件替换到工作区

```
TestGit$ echo "new change to testfile1" >> testfile1
TestGit$
TestGit$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   testfile1

no changes added to commit (use "git add" and/or "git commit -a")
TestGit$
TestGit$ git checkout testfile1
Updated 1 path from the index
TestGit$
TestGit$ git status
On branch master
nothing to commit, working tree clean
```

### 已提交至暂存区

1. 使用git reset HEAD filename 撤销暂存区的修改，恢复暂存区的文件与master分支一致
2. 再使用git checkout filename 将暂存区的文件替换到工作区

```
TestGit$ echo "new change to testfile1" >> testfile1
TestGit$
TestGit$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   testfile1

no changes added to commit (use "git add" and/or "git commit -a")
TestGit$
TestGit$ git add testfile1
TestGit$
TestGit$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   testfile1

TestGit$
TestGit$ git reset HEAD testfile1
Unstaged changes after reset:
M       testfile1
TestGit$
TestGit$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   testfile1

no changes added to commit (use "git add" and/or "git commit -a")
TestGit$
TestGit$
TestGit$ git checkout testfile1
Updated 1 path from the index
TestGit$
TestGit$ git status
On branch master
nothing to commit, working tree clean
```

### 已提交至master分支

1. 使用git reset --hard commit_id 回退版本至修改前
2. 使用git checkout filename 将master分支文件替换到工作区

## 删除文件

- 使用git rm filename删除暂存区文件

若想撤销删除，则也可使用git checkout filename 将暂存区中文件替换到工作区，若已提交至暂存区，则先使用reset恢复暂存区，再使用checkout恢复工作区。

```
TestGit$ git rm testfile2
rm 'testfile2'
TestGit$
TestGit$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        deleted:    testfile2

TestGit$
TestGit$ git reset HEAD testfile2
Unstaged changes after reset:
D       testfile2
TestGit$
TestGit$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    testfile2

no changes added to commit (use "git add" and/or "git commit -a")
TestGit$
TestGit$ git checkout testfile2
Updated 1 path from the index
TestGit$
TestGit$ git status
On branch master
nothing to commit, working tree clean
```

## 分支操作

### 创建/切换分支

- 使用git branch branch_name 创建分支
- 使用git checkout branch_name 切换至此分支
- 使用git checkout -b branch_name 创建新分支并切换至此新分支
- 使用git branch 查看所有分支，当前分支前面有有个*

```
TestGit$ git branch
* master
TestGit$ git branch dev
TestGit$ git branch
  dev
* master
TestGit$ git checkout dev
D       testfile2
Switched to branch 'dev'
TestGit$
TestGit$ git branch
* dev
  master
```

切换至dev分支后，对文件进行修改并提交，切换至master分支，可以查看到master分支的文件未被修改。

```
TestGit$ echo "this is a new branch" >> testfile1
TestGit$ git add testfile1
TestGit$ git commit -m "testfile1"
[dev 12497d8] testfile1
 2 files changed, 1 insertion(+), 2 deletions(-)
 delete mode 100644 testfile2
TestGit$
TestGit$ cat testfile1
hello, git
hello, me again
yes, it's me!
change to testfile1
this is a new branch
TestGit$
TestGit$ git checkout master
Switched to branch 'master'
TestGit$
TestGit$ cat testfile1
hello, git
hello, me again
yes, it's me!
change to testfile1
```

### 查看本地分支

- 使用git branch 查看本地分支列表, *表示为当前使用分支

```
TestGit$ git branch
* dev
  master
```

### 合并指定分支

- 使用git merge branch_name 合并分支到当前分支

当合并dev分支至master分支后，在master分支即可看到dev分支进行的修改。

```
TestGit$ git merge dev
Updating 27b45ec..12497d8
Fast-forward
 testfile1 | 1 +
 testfile2 | 2 --
 2 files changed, 1 insertion(+), 2 deletions(-)
 delete mode 100644 testfile2
TestGit$
TestGit$ cat testfile1
hello, git
hello, me again
yes, it's me!
change to testfile1
this is a new branch
```

### 删除分支

- 使用git branch -d branch_name进行删除

```
TestGit$ git branch
  dev
* master
TestGit$ git branch -d dev
Deleted branch dev (was 12497d8).
TestGit$
TestGit$ git branch
* master
```

## 远程仓库操作

### 添加远程库

- 使用git remote add origin命令添加

```
git remote add origin git@github.com:michaelliao/learngit.git
```

origin为远程库的名字，默认命名。

### 提交至远程库

- 使用git push提交

远程仓库的默认名称为origin，-u为初次提交时使用，后续提交使用git push即可。

```
git push -u origin master # 将master分支提交至远程仓库
git push origin dev # 将dev分支提交至远程仓库
```

### 拉取远程库

- 使用git pull抓取对应分支的修改至本地

如果git pull成功，但是有冲突，则需要手动解决后再提交，push至远程仓库。

git pull = git fetch + git merge

### 克隆远程库

- 使用git clone

```
git clone git@github.com:michaelliao/gitskills.git
```

Git支持多种协议，包括`https`，但`ssh`协议速度最快。

Tips：登录github，“Settings“ -> "SSH and GPG keys" -> "New SSH Key”，添加主机的id_rsa.pub，后续使用ssh协议进行repo的相关操作。

### 查看远程库

- 使用git remote -v

```
$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
```

### 创建/建立本地分支与远程库分支关联

- 使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

- 使用`git branch --set-upstream branch-name origin/branch-name`；

### 删除远程分支

- 删除本地分支 git branch -d test
- 删除远程分支 git push origin --delete test

## 子模块

在某一个Git 项目上工作时，需要在其中使用另外一个Git 项目，这种情况下可以用子模块`submodule`来管理这些项目，`submodule`允许你将一个Git 仓库当作另外一个Git 仓库的子目录。这允许你克隆另外一个仓库到你的项目中并且保持你的提交相对独立。

### 添加子模块

- 使用git submodule add 

```
# 将子模块添加至本地的themes/ananke文件夹
Hugo$ git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
Cloning into '/mnt/d/learn/Hugo/themes/ananke'...
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 1873 (delta 0), reused 2 (delta 0), pack-reused 1869
Receiving objects: 100% (1873/1873), 4.36 MiB | 10.00 KiB/s, done.
Resolving deltas: 100% (1046/1046), done.
```

### 查看子模块

- 使用git submodule查看

```
Hugo$ git submodule
 056246eb62b96c66d9ea55d9485053ae2c29bfd5 themes/ananke (v2.5.6-45-g056246e)
```

使用git status可以查看工作区新添了.gitmodules文件

```
Hugo$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   .gitmodules
        new file:   themes/ananke

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        archetypes/
        config.toml
```

### 更新子模块

- 使用git submodule update 

```
Hugo$ git submodule update
```

# 常见应用场景

## 合并冲突

需要手动解决冲突后再提交。

```
TestGit$ git switch -c test
Switched to a new branch 'test'
TestGit$
TestGit$ echo "on test branch, make changes to testfile1" >> testfile1
TestGit$ git commit -a -m "update testfile1"
[test c881e8c] update testfile1
 1 file changed, 1 insertion(+)
TestGit$
TestGit$ git switch master
Switched to branch 'master'
TestGit$
TestGit$ echo "on master branch, make changes to testfile1" >> testfile1
TestGit$ git commit -a -m "update testfile1"
[master c8ee491] update testfile1
 1 file changed, 1 insertion(+)
TestGit$
TestGit$ git branch
* master
  test
TestGit$ git merge test
Auto-merging testfile1
CONFLICT (content): Merge conflict in testfile1
Automatic merge failed; fix conflicts and then commit the result.
TestGit$
TestGit$ git diff testfile1
diff --cc testfile1
index 07406a2,3a3894c..0000000
mode 100644,100644..100755
--- a/testfile1
+++ b/testfile1
@@@ -3,4 -3,4 +3,8 @@@ hello, me agai
  yes, it's me!
  change to testfile1
  this is a new branch
++<<<<<<< HEAD
 +on master branch, make changes to testfile1
++=======
+ on test branch, make changes to testfile1
++>>>>>>> test

TestGit$ vim testfile1
TestGit$
TestGit$ git commit -a -m "fix merge conflict"
[master 3d25b74] fix merge conflict
TestGit$
TestGit$ git log --graph --pretty=oneline --abbrev-commit
*   3d25b74 (HEAD -> master) fix merge conflict
|\
| * c881e8c (test) update testfile1
* | c8ee491 update testfile1
|/
* 12497d8 testfile1
* 27b45ec update testfile1 and testfile2
* d643e5c Add testfile2
* f70d613 Update testfile1
* bb3b0a1 Add testfile1
```

## 保留分支合并

TODO 没太明白

- 使用git merge --no-ff -m "message"，在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

```
TestGit$ git checkout master
Switched to branch 'master'
TestGit$
TestGit$ git merge --no-ff -m "merge dev to master branch" dev
Merge made by the 'recursive' strategy.
 testfile2 | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 testfile2
TestGit$
TestGit$ git log --graph --pretty=oneline --abbrev-commit
*   19e0760 (HEAD -> master) merge dev to master branch
|\
| * c9fb039 (dev) Add testfile2
|/
*   fd70cbc fix merge conflict
|\
| * 21d269b update testfile1
* | 2e9f2e5 update testfile1
* | 3d25b74 fix merge conflict
|\|
| * c881e8c update testfile1
* | c8ee491 update testfile1
|/
* 12497d8 testfile1
* 27b45ec update testfile1 and testfile2
* d643e5c Add testfile2
* f70d613 Update testfile1
* bb3b0a1 Add testfile1
```

## 暂存本地工作区文件

- 使用git stash 把当前工作现场“储藏”起来
- 使用git stash pop恢复现场后继续工作
- 使用git stash list 查看stash列表
- 使用git stash apply 恢复指定的stash内容

https://www.liaoxuefeng.com/wiki/896043488029600/900388704535136

## 添加指定提交至当前分支

- 使用git cherry-pick commit_id命令，把指定提交“复制”到当前分支，避免重复劳动。

## Rebase



https://www.liaoxuefeng.com/wiki/896043488029600/1216289527823648

git submodule



# git 命令检索

- git init 初始化仓库

- git add 提交修改至暂存区

- git commit -m 提交修改至仓库
- git log 查看提交记录
- git reflog 查看所有操作历史记录
- git reset 回退至指定版本
- git checkout 撤销修改
- git rm 删除文件
- git add remote 将本地工作区添加至远程分支
- git push 将本地分支提交至remote分支
- git clone 克隆远程分支至本地
- git branch 查看所有分支
- git branch -b 创建分支
- git checkout 切换分支
- git merge 合并分支
- git branch -d 删除分支
- git log --graph --pretty=oneline --abbrev-commit 查看分支合并图



# git cheatsheet

https://training.github.com/downloads/zh_CN/github-git-cheat-sheet/

https://gitee.com/liaoxuefeng/learn-java/raw/master/teach/git-cheatsheet.pdf

# git book

官方：https://git-scm.com/book/zh/v2

# git ignore模板

https://github.com/github/gitignore

# 常见问题

## 手动下载zip包，关联原repo

https://stackoverflow.com/questions/15681643/how-to-clone-git-repository-from-its-zip

## 换行符问题

http://kuanghy.github.io/2017/03/19/git-lf-or-crlf

https://www.jianshu.com/p/a340848a6ec1

## merge和rebase的区别

https://blog.csdn.net/xsj_blog/article/details/79198843

https://github.com/geeeeeeeeek/git-recipes/wiki/5.1-%E4%BB%A3%E7%A0%81%E5%90%88%E5%B9%B6%EF%BC%9AMerge%E3%80%81Rebase-%E7%9A%84%E9%80%89%E6%8B%A9

