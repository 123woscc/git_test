# Git命令 

### 1.创建版本库

```git
> git init 
Initialized empty Git repository in C:/Users/12845/Desktop/my_github/git_test/.git/
```

###  2.添加到仓库

```
> git add README.md
```

> LF will be replaced by CRLF 解决方案
>
> ```
> > git config --gobal core.autocrlf        #输出ture
> > git config --gobal core.autocrlf false
> ```

### 3.提交到版本库

```
> git commit -m "message"
```

> -m 'message' 添加提交说明
>
> 可以执行多次`git add`后 再一次性执行`git commit` 提交所有操作到版本库

### 4. 版本信息查看

```
> git log 
```

> 输出 `date`  `author`  `commit message`  `commit_id`
>
> commit_id == 版本号
>
> HEAD == 当前版本标记

### 5.版本回退

* 方案一

```
> git reset --hard HEAD^
```

>用`HEAD`表示当前版本，也就是最新的提交，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`

* 方案二

```
> git reset --hard commit_id
```

> 版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。

### 6.版本回退失误恢复操作

> 回退过头了想反悔怎么办

```
> git log
commit 8edf4d9f64a977ea573e6b6bcecdc35a95d4b97c (HEAD -> master)
Author: 123woscc <1284511710@qq.com>
Date:   Fri Jul 21 11:36:13 2017 +0800

    first
```

> 使用` git log`已无法查看当前版本之后的 `commit_id` 

```
> git reflog
8edf4d9 (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
83ee005 HEAD@{1}: commit: add some
8edf4d9 (HEAD -> master) HEAD@{2}: commit (initial): first
```

> 可使用`git reflog`查看历史操作,找到最后一次操作的`commit_id` ,然后执行回退操作

### 7.工作区状态查看

````
> git status
````

> Untracked: 未被 `git add` 提交的文件 

<<<<<<< HEAD
```&amp;amp;amp;amp;gt; 
=======
​```&amp;amp;amp;gt; 
>>>>>>> error1
> git diff HEAD -- readme.md
```

> 查看当前工作区文件和已`git add` 后的文件的区别,`git commit ` 只会提交`git add` 后的文件,所有每次修改
>
> 文件后都得执行`git add` 后再` git commit`  .
>
> 正确执行步骤: 第一次修改 -> `git add` -> 第二次修改 -> `git commit`

### 8.撤销修改

* 状态一:只修改了内容未执行任何git提交操作

  ```
  > git checkout -- file
  ```

  > 让文件回到最近一次`git commit`或`git add`时的状态。

* 状态二:已经执行了`git add` 操作

  ```
  > git reset HEAD file
  > git checkout -- file
  ```

  > `git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区

* 状态三: 已执行了`git commit` 操作

  ```
  > git reset --hard HEAD^
  ```

  或者

  ```
  > git reset --hard commit_id
  ```

  > 直接回退到上一个提交的版本

### 9.删除操作

* 正确删除

  ```
  > rm file #删除文件命令
  > git rm file
  > git commit -m 'msg'
  ```

* 误删撤回

  ```
  > git checkout -- file
  ```

> `git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

### 10.远程仓库

1. 创建SSH KEY

   ```
   > ssh-keygen -t rsa -C "youremail@example.com"
   ```

   > 如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

2. 关联远程仓库

   陆GitHub，打开“Settings >> SSH and GPG keys >> New SSH key” , title随便填,key填写`id_rsa.pub`文件的内容,然后点击Add SSH key.

3. 添加远程仓库

   登陆GitHub，然后，点击右上角的加号找到“New repository”按钮，创建一个新的仓库,Repository name可以随便取,Description (optional)为描述,可以不填,然后点击Create repository创建.

4. 推送文件到远程仓库

   按照GitHub的提示操作:

   ```
   > git init
   > git add README.md                         #添加文件
   > git commit -m "first commit"              #提交
   > git remote add origin https://github.com/username/test.git        #关联本地仓库与远程仓库
   > git push -u origin master                 #推送到远程仓库
   ```

   > `username  ` 修改为自己的GitHub Id
   >
   > `test  `  修改为自己添加远程仓库时的 Repository name
   >
   > `origin`这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。
   >
   > `master `由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

5. 克隆远程仓库

   ```
   > git clone https://github.com/username/repository_name.git
   ```

### 11.分支管理

1. 创建分支

   ```
   > git branch dev     #创建分支
   > git checkout dev   #切换到dev分支
   ```

   合并命令

   ```
   > git checkout -b dev       # git checkout命令加上-b参数表示创建并切换，相当于以下两条命令
   ```

2. 查看所有分支

   ```
   > git branch
   ```

3. 切换分支

   ```
   > git checkout master
   ```

4. 合并到当前分支

   ```
   > git merge dev
   ```

5. 删除分支

   ```
   git branch -d dev
   ```

### 12.合并冲突解决

```
> git status  #查看发生冲突的文件,然后打开
```

Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们修改如下后保存,再提交：

> 就是人肉修改冲突文件后（其实不改也行就是确认一下），再次add and commit，系统就确认那个是最新(merge后）版本了。

```
> git add readme.md 
> git commit -m "conflict fixed"
[master 59bc1cb] conflict fixed
```

用带参数的`git log`也可以看到分支的合并情况：

```
$ git log --graph --pretty=oneline --abbrev-commit
*   59bc1cb conflict fixed
|\
| * 75a857c AND simple
* | 400b400 & simple
|/
* fec145a branch test
...
```

