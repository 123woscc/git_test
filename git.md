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

### 3.提交到版本库

```
> git commit -m "message"
```

> -m 'message' 添加提交说明

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

```
> git reset --hard HEAD^
```



