[TOC]



### git

基本概念：

- 工作区：仓库的目录。工作区是独立于各个分支的。
- 暂存区：数据暂时存放的区域，类似于工作区写入版本库前的缓存区。暂存区是独立于各个分支的。
- 版本库：存放所有已经提交到本地仓库的代码版本
- 版本结构：树结构，树中每个节点代表一个代码版本。

#### git 常用命令

##### 1：初始设置命令

设置全局用户名，信息记录在 `~/.gitconfig` 文件中

```
git config --global user.name xxx
```

设置全局邮箱地址，信息记录在 `~/.gitconfig `文件中

```
git config --global user.email xxx@xxx.com
```

##### 2：git 仓库初始化

将当前目录配置成git仓库，信息记录在隐藏的 `.git` 文件夹中

```
git init
```

##### 3：基本操作

查看仓库状态

```
git status
```

###### 暂存区

将 XX 文件添加到暂存区

```
git add XX
```

将所有待加入暂存区的文件加入暂存区

```
git add .
```

将 XX 文件从  暂存区 移出 (但 git 依然管理这个文件)

```
git restore --staged XX
```



将文件从仓库索引目录中删掉 , 即 git 不再管理 XX 文件

```
git rm --cached XX
```

查看 XX文件 相对于 暂存区 修改了哪些内容

```
git diff XX
```



将 XX文件尚未加入缓存区的修改全部撤销

```
git restore XX
或
git checkout XX
```

```
git restore XX     # 同样也可以将已经删除的文件但是并未加入缓存区的操作 撤销
```



###### 暂存区 与 版本

将暂存区的内容提交到当前分支的一个版本

```
git commit -m "备注"
```

查看当前分支的所有版本

```
git log
```

查看 HEAD指针 的移动历史（包括被回滚的版本）

```
git reflog
```



###### 回滚操作

将代码库回滚到上一个版本

```
git reset --hard HEAD^
或 
git reset --hard HEAD~
```

- `git reset --hard HEAD^^`  : 往上回滚两次，以此类推
- `git reset --hard HEAD~100` : 往上回滚100个版本
- **`git reset --hard 版本号`  : 回滚到某一特定版本**  （可以用于回退操作）



###### 本地仓库关联到远程仓库

查看当前所有的远程地址名

```
git remote -v
```

为远程仓库创建别名

```
git remote add 别名 远程库名
```

将当前分支推送到远程仓库

```
git push -u (第一次需要-u以后不需要)
```

将本地的 `branch_name` 分支推送到远程仓库 `origin`

```
git push origin branch_name
```

将远程仓库 XXX 下载到当前目录下

```
git clone git@git.acwing.com:xxx/XXX.git
```

创建并切换到 `branch_name` 这个分支

```
git checkout -b branch_name
```



























