## 常用操作

### git clone

### git config 配置开发者用户和邮箱

```bash
git config user.name xxxx
git config user.email xxxx
```

### git branch 项目分支

```bash
git branch xxx //创建一个名为xxx的分支
git branch -m xxx1 xxx2 //将xxx1分支重命名为xxx2
git branch //查看当前项目分支表
git branch -d xxx //删除xxx分支
git branch -a 查看本地和远程仓库的分支列表
git branch -r 查看远程仓库的分支列表
git branch -vv //查看最后提交id，最近提交原因等信息的本地仓库分支列表
```

### git checkout 切换分支

```bash
git checkout xxx //切换到xxx分支
git checkout -b xxx //创建xxx分钟同时切换到这个新建分支
```

### git status 查看文件变动状态

### git add 添加文件变动到暂存区

```bash
//patch
git add -p //显示当前内容和本地版本库的差异，然后决定操作方式
```

+ y 接受修改
+ n 忽略修改
+ q 退出当前命令
+ a 添加修改
+ d 放弃修改
+ / 通过正则表达式匹配修改的内容
+ ？查看帮助

```bash
//update
git add -u //
```

### git commit 提交文件变动到版本库

```bash
git commit -m "这里填写log"
```

### git push 将本地代码改动推送至服务器

```bash
git push origin xxxx
```

> origin指代的是当前的git服务器地址

### git pull 将服务器上最新的代码拉取到本地

```bash
git pull origin xxx
```

### git log 查看版本提交记录

> J往下翻页，K往上翻页，Q退出查看

### git tag 为项目标记里程碑

```bash
git tag 标记名
```

### .gitignore 设置哪些内容不需要推送至服务器

不是指令，是个文件

### git merge 将其他分支合并到当前分支

### git stash 栈的操作

```bash
git stash 将未提交的文件保存到栈中
git stash list 查看栈中保存的列表

```

### git reset 将当前分支重设到指定的`commit`或者`HEAD`

