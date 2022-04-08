# Git基本操作

###Github等平台建立仓库

SSH Key与Github平台连接

在Github上建立仓库，复制仓库地址 git@github.com:XXX

默认分支：main（之前是master）

### 本地文件处理

1.将文件夹初始化：`git init`

2.添加文件：`git add . `（空格，提交全部文件/文件名）

3.提交本地文件：`git commit -m "注释/标识"`

### 远程仓库操作

1.连接远程仓库：`git remote add origin git@github.com:XXX`

2.克隆远程仓库：`git clone git@github.com:XXX`

3.提交到远程仓库，先切换到本地分支：（远程分支：remote_branch，本地分支：local_branch）

​	a.远程已有分支并且已经关联本地分支：`git push` 

​	b.远程已有分支但未关联本地分支：` git push -u origin remote_branch`

​	c.远程没有分支:` git push origin local_branch:remote_branch`

### 分支管理

1.创建并切换到 branch_1 分支：`git checkout -b branch_1`

2.删除分支：`git checkout -d branch_1`

3.查看分支：`git branch`

4.切换分支：`git checkout branch_name`

5.合并分支：`git merge branch_name`  （把branch_name分支上的内容合并到当前分支）

### 其他命令

1.查看仓库状态：`git status `

2.查看文件修改：`git diff file_name`

### 常用工具

- lazygit（https://github.com/jesseduffield/lazygit）
- sourcetree（https://www.sourcetreeapp.com/）
