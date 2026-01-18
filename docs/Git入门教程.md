## 1、什么是Git？

`Git`是一个**分布式版本控制工具**，用于管理代码的版本

### 1.1、什么是版本控制？

举一个简单例子，写过毕业论文的小伙伴都知道

毕业论文往往不是一次撰写就能完成的

每次将word文档发送给指导老师

指导老师如果发现有哪里需要修改

会给出一定的指导意见

那我们就需要修改好，再次发给老师评审

但有些指导老师总是喜欢左右脑互博

可能前几天他说的修改意见，按照这个意见修改之后

过了几天再给他看，他可能又否定这个修改

为了应对这种情况，不让我们的努力付之东流

我们可以在每次发送word文档时，在本地重命名一份

标注好这个版本的特点，比如：

盘子毕业论文_初版.docx

盘子毕业论文_修改摘要版.docx

盘子毕业论文_调整图片格式版.docx

这个过程就是一个简单的版本管理

而`git`这种版本控制工具可以实现更多功能

### 1.2、什么是分布式，什么是集中式？

**集中式版本控制**（Centralized Version Control）是指所有的版本历史和数据都存储在一个 **中央服务器** 上，开发者通过与中央服务器的交互来获取代码、提交更改以及查看版本历史

**分布式版本控制**（Distributed Version Control）是指每个开发者的本地工作副本不仅包含最新的文件，还保存了 **整个项目的完整历史记录**。开发者无需依赖中央服务器就能完成大部分版本控制操作

两种类型版本控制的简单对比

| 特性         | 集中式版本控制               | 分布式版本控制                                 |
| ------------ | ---------------------------- | ---------------------------------------------- |
| **版本存储** | 所有版本保存在中央服务器上   | 每个开发者本地有完整的历史记录                 |
| **操作依赖** | 必须连接到中央服务器进行操作 | 可以在本地完成大部分操作，支持离线工作         |
| **故障恢复** | 中央服务器宕机时无法工作     | 即使没有中央服务器，开发者也可以继续工作       |
| **效率**     | 需要网络连接，操作较慢       | 本地操作速度快，网络操作只有在需要同步时才需要 |
| **协作方式** | 所有操作依赖中央仓库         | 开发者本地工作后，通过推送与拉取进行同步       |

### 1.3、Git工作区域

在 Git 中，有四个主要的工作区域

它们分别是工作目录、暂存区、本地仓库和远程仓库

这些区域共同协作，管理文件的变动和版本历史

**工作目录（Working Directory）**

- 本地计算机上项目的实际文件夹

**暂存区（Staging Area）**

- 暂存区是一个虚拟的区域，它保存着准备提交的文件状态
- 可以选择要提交的修改

**本地仓库（Local Repository）**

- 本地仓库是 Git 存储项目历史和版本信息的地方
- 本地仓库保存了完整的历史记录，包括每次提交的快照、提交信息、分支和标签等

**远程仓库（Remote Repository）**

- 远程仓库通常托管在 Git 服务器或代码托管平台（如 GitHub、GitLab 等）上
- 开发团队成员可以通过远程仓库实现多人协作开发

例如，现在有一个项目叫做blog，其中有一个代码文件index.html，一个.git目录

工作目录就是blog文件夹，暂存区、本地仓库等内容，会存放在.git目录里

### 1.4、Git文件状态

Git 跟踪文件的状态可以分为以下四种：

**未追踪（Untracked）**
 文件刚被添加到 工作目录，但用户没有使用Git追踪，它还没有被加入到暂存区或提交到本地仓库。

**未修改（Unmodified）**
 文件在工作区没有变化，自上次提交以来未做过任何更改，与仓库中的版本相同的状态。

**已修改（Modified）**
 文件被修改，但还没有被添加到暂存区。

**已暂存（Staged）**

文件修改已放入暂存区，等待提交，文件后续修改不影响已暂存内容。

它们之间的转换关系如下图所示：

![](https://Mecamodore.github.io/picx-images-hosting/文件状态.8vnesy2ohw.webp)

### 1.5、Git对象及引用

在 Git 中，所有数据都以对象（Object） 形式存储在 .git/objects 目录中

引用（References）是轻量级的指针，指向具体的commit对象

1. **Blob 对象**  
   - 存储单个文件的内容快照，不含文件名或路径  
   - 通过内容计算 SHA-1 哈希值（40位十六进制字符串）唯一标识，内容变哈希值也会变  
2. **Tree 对象**  
   - 存储目录结构快照，记录文件子目录的组织关系  
   - 包含`文件名`、 `类型(blob/tree)`、 `对应对象的哈希`  
   - 根 Tree 指向 commit，子 Tree 构成目录层级
3. **Commit 对象**  
   - 完整记录某一时刻项目文件的状态，包含：  
     - 指向根 Tree 的指针（完整项目状态）  
     - 父 Commit 哈希（构建历史链）  ，首次提交没有父提交
     - 作者、提交者、时间等元数据  
     - 提交信息
4. **Branch（分支引用）**
   - 本质是一个**指向commit的指针**
   - Git允许创建多个分支，以此来区分一些功能或版本
   - 比如你可以创建修复bug分支，或者测试版分支
   - 它是一个纯文本文件，内容为分支上最新一次提交的哈希
   - 提交时自动前移，使分支指针指向新 commit
5. **HEAD（当前工作区引用）**  
   - 本质是**指向当前位置的特殊指针** ，有两种状态
   - 默认，指向分支
   - 分离状态，直接指向commit哈希 

它们之间的关系如下图所示：

![对象及引用](https://Mecamodore.github.io/picx-images-hosting/对象及引用.64eckvgkg1.webp)

## 2、Git安装

### **2.1、Git 官方网站**  

https://git-scm.com  

### **2.2、Windows 系统安装**  

https://git-scm.com/install/windows下载安装包，在安装过程下一步，记得添加环境变量

### 2.3、MacOS系统安装

https://git-scm.com/install/mac使用Homebrew包管理安装

如果没有安装Homebrew，https://brew.sh.cn/

安装Git 

```bash
brew install git
```

### **2.4、 Linux 系统安装**  

使用包管理器

**Debian/Ubuntu**：  

```bash
sudo apt update && sudo apt install git -y
```

**RHEL/Fedora/CentOS**：  

```bash
sudo dnf install git -y        # Fedora/RHEL 8+
sudo yum install git -y        # CentOS 7（旧版）
```

**Arch/Manjaro**：  

```bash
sudo pacman -S git --noconfirm
```

### 2.5、安装后操作

- 验证是否成功

```bash
git --version
```

- 配置用户名和邮箱

```bash
git config --global user.name "用户名"
git config --global user.email "邮箱"
```

配置有三个等级，优先级从上到下越来越高

- **`--system`**：对这个系统下所有用户的所有仓库都应用配置
- **`--global`**：对当前用户的所有仓库都应用配置
- **`--local(默认)`**：对当前仓库应用配置

## 3、Git基础操作

### 3.1、Git配置（git config）

除了上节课配置的用户名和邮箱外，git config还有许多常用的配置项

- 设置默认编辑器

```bash
# 设置VS Code为默认编辑器
git config --global core.editor "code --wait"
# 设置Vim为默认编辑器
git config --global core.editor "vim"
# 设置Notepad++（Windows）
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -nosession"
```
- 配置命令别名（alias）

```bash
# 常用别名设置
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
# 查看别名配置
git config --global --list | grep alias
```
- 颜色输出设置

```bash
# 启用彩色输出
git config --global color.ui true
git config --global color.status auto
git config --global color.diff auto
git config --global color.branch auto
```
- 代理设置

```bash
# 设置HTTP/HTTPS代理
git config --global http.proxy http://proxy.example.com:8080
git config --global https.proxy http://proxy.example.com:8080
# 取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy
```
- 换行符设置

```bash
# Windows推荐设置
git config --global core.autocrlf true
# macOS/Linux推荐设置  
git config --global core.autocrlf input
# 不进行转换
git config --global core.autocrlf false
```

- 查看配置

```bash
# 查看当前仓库配置
git config --list
# 查看全局配置
git config --global --list
# 查看系统配置
git config --system --list
# 直接编辑配置文件
git config --global -e  # 编辑全局配置文件
```
### 3.2、新建Git仓库（git init、git clone）

有两种方式新建Git仓库

- 本地初始化Git仓库，在项目目录里输入

```bash
git init
```

- 从其它仓库克隆一个仓库，这个仓库可以是远程的也可以是本地的

```bash
git clone https://github.com/pcottle/learnGitBranching.git
```

### 3.3、查看文件状态（git status）

用于显示工作区和暂存区的状态：

```bash
git status
```

- **Untracked files**：未被Git跟踪的文件
- **Changes to be committed**：已添加到暂存区，等待提交的更改
- **Changes not staged for commit**：工作区有修改但未添加到暂存区

为了让输出更简洁，可以使用：
```bash
git status -s  # 简洁模式
```

左边字符是暂存区的状态，右边字符是工作区的状态
- `??` 表示未跟踪的文件
- `AM`表示文件已添加到暂存区，文件又修改了
- `MM`表示暂存区和工作区都有修改
- `A` 表示已添加到暂存区的新文件
- `M` 表示已修改的文件
- `D` 表示已删除的文件

### 3.4、添加文件到暂存区（git add）

git add用于追踪文件，也将文件放到暂存区

```bash
# 添加单个文件
git add README.md
# 添加多个文件
git add file1.txt file2.txt
# 添加整个目录
git add docs/
# 添加所有修改过的文件
git add .
# 交互式添加（可以选择性地暂存部分修改）
git add -p
```

### 3.5、提交到本地仓库（git commit）

将暂存区的文件快照提交到本地仓库，创建一个新的版本

```bash
# 基本提交（会打开编辑器要求输入提交信息）
git commit
# 带提交信息的提交
git commit -m "添加README文件"
# 跳过暂存区直接提交（只适用于已跟踪的文件）
git commit -a -m "更新配置文件"
# 修改最后一次提交
git commit --amend -m "修正提交信息"
# 追加最后一次提交
git add 修改后的文件
git commit --amend -m "更新后的提交信息"
```

### 3.6、查看提交历史（git log）

```bash
# 查看完整日志
git log
# 显示每次提交的文件修改统计
git log --stat
# 简洁日志
git log --oneline
# 图形化日志
git log --graph --oneline --all
# 查看最近3次提交
git log -3
# 按作者查看
git log --author="your-name"
```

### 3.7、查看差异（git diff）

```bash
# 查看工作区与暂存区的差异
git diff
# 查看暂存区与最新版本的差异
git diff --cached
# 比较工作区与最新版本的差异
git diff HEAD
# 查看两次提交之间的差异 变更前 变更后
git diff commit_id1 commit_id2
# 查看特定文件的差异
git diff README.md
```

### 3.8、查看详细信息（git show）

```bash
# 显示最新提交的详细信息
git show
# 显示指定提交的详细信息
git show <commit-hash>
git show HEAD~2
git show feature
# 显示标签信息
git show v1.0.0
```

### 3.9、回退和撤销更改（git reset）

git reset有三种模式

- `--soft` ：撤销提交，但保留暂存区内容
- `--mixed`：默认模式，撤销提交和暂存区内容，保留工作区代码
- `--hard`：彻底删除提交和代码

```bash
# 取消单个文件的暂存
git reset HEAD 文件名
# 取消所有已暂存文件（回退到上个版本)
git reset HEAD~1
# 撤销最近三次提交
git reset HEAD~3
# 回到某个特定版本
git reset <commitID>
# 彻底丢弃最近三次提交和代码
git reset --hard HEAD~3
```

### 3.10、找回彻底删除的提交（git reflog）

reflog记录了本地仓库中 HEAD 和分支引用的所有变化历史，默认保持90天记录，可以通过gc.reflogExpire配置

找到对应提交记录的id，使用git reset回退到该记录

### 3.11、删除与取消追踪文件（git rm）

```bash
# 删除文件,同时从工作区和Git中删除
git rm file.txt
git commit -m "删除file.txt"
# 强制删除文件
git rm -f file.txt
# 删除目录
git rm -r directory/
# 仅从Git中删除(取消跟踪)，保留工作区文件
git rm --cached file.txt
```

### 3.12、移动或重命名文件（git mv）

```bash
# 将文件重命名
git mv oldname newname
# 移动文件到目录
git mv file.txt directory/
# 移动多个文件
git mv file1.txt file2.txt directory/
# 移动目录
git mv old_directory/ new_directory/
```

### 3.13、忽略追踪文件（.gitignore）

通过`.gitignore`文件可以指定不纳入版本控制的文件和目录

如果文件目前已追踪了，先将文件取消追踪

创建一个`.gitignore`文件，常用忽略规则：
```gitignore
# 忽略所有.log文件
*.log

# 忽略build目录
build/

# 忽略node_modules目录
node_modules/

# 忽略特定文件
config/local.conf

# 不忽略docs目录下的README.md
!docs/README.md
```

## 4、Git 分支管理

### 4.1、查看分支（git branch）

```bash
git branch          # 列出本地分支（* 标记当前分支）  
git branch -v       # 显示各分支最新提交摘要  
git branch --merged # 仅列出已合并到当前分支的分支  
```

### 4.2、创建分支（git branch）

```bash
git branch 分支名  # 从当前HEAD创建新分支  
```

### 4.3、重命名分支（git branch）

```bash
git branch -m 旧分支名 新分支名 # 不能改为已存在的分支名
# -m 也可以用来移动分支
```

### 4.4、删除分支（git branch）

```bash
git branch -d 分支名  # 删除已合并分支
git branch -D 分支名  # 强制删除分支
```

### 4.5、切换分支或提交版本（git checkout）

```bash
git checkout 分支名  # 如果本地仓库和远程仓库都没有这个分支，会报错

git checkout abc123 # 切换到某次提交版本
# 此时，处于游离态detached HEAD
# 如果你直接修改提交代码，这次提交将不属于任何一条分支
# git会在一段时间后自动清理这次提交

# 所以，直接基于历史 commit 创建新分支并切换
git checkout -b fix-history abc123 # -b 是创建分支并切换到该分支
# 修改文件...
git add .
git commit -m "修复问题"
```

### 4.6、临时保存工作区（git stash）

git在切换分支时，要求worktree clean，

即任何已修改代码要提交之后，才能切换分支
当项目遇到紧急bug，需要立刻切换到bugfix分支去修复时
当前工作区的代码并没有修改完，不能提交
git stash就解决了这个痛点

```bash
git stash # 保存当前修改（不包括未跟踪文件）
git stash push -m "描述" # 保存并添加注释
git stash list  # 查看所有 stash 记录
git stash apply # 恢复最近一次 stash（不删除记录）
git stash pop   # 恢复最近一次 stash 并删除记录
git stash drop  # 删除最近一次 stash 记录
git stash clear # 清空所有 stash 记录
git stash show -p stash@{1} # 查看指定 stash 的详细 diff
git stash branch new-branch # 从 stash 创建新分支
```

### 4.7、简单合并分支（git merge）

普通合并会将一个分支的完整历史合并到当前分支

找到两个分支的最近共同父提交

创建一个新的提交，包含两个分支的变更

合并后提交历史会有一个叉

```bash
git checkout main # 切换到目标分支
git merge bugfix  # 合并bugfix到main
```

### 4.8、变基合并分支（git rebase）

变基合并会将一个分支的提交直接移动到另一个分支

最新提交之后，就像嫁接一样

合并后提交历史是一条直线

变基合并通常用于在首次推送到远程仓库前

整理分支与提交记录，最好不要对远程仓库拉取的代码进行变基

```bash
git checkout bugfix  # 切换到准备移植的分支
git rebase main      # 将main作为基底，把bugfix嫁接到main上
git checkout main	 # 完成后续合并操作
git merge bugfix
```

### 4.9、优选合并分支（git cherry-pick）

优选合并是选择特定提交，将它移植到当前分支

```bash
git checkout bugfix	    # 切换到目标分支
git cherry-pick abc123  # 将提交 abc123 移植到当前分支
```

### 4.10、标签管理（git tag）

Git标签是用于标记特定提交（commit）的引用

通常用于标记软件发布版本（如v1.0、v2.0等）

Git有两种类型的标签：

- 轻量标签：只是特定提交的一个引用
- 附注标签：存储在Git数据库中的完整对象，包含标签创建者、日期、消息等元数据

```bash
# 创建轻量标签
git tag v1.0
# 对指定提交打标签
git tag v0.9 <commit-id>
# 创建附注标签
git tag -a v1.0 -m "发布1.0版本"

# 列出所有标签
git tag
# 搜索特定标签
git tag -l "v1.*"
# 查看标签详细信息
git show v1.0

# 推送单个标签
git push origin v1.0
# 推送所有标签
git push origin --tags

# 本地删除
git tag -d v1.0
# 删除远程标签
git push origin --delete v1.0

# 基于标签创建新分支
git checkout -b version1.0 v1.0
```

## 5、Git远程协作

### 5.1、添加、查看、重命名、删除远程仓库（git remote）

```bash
# 添加远程仓库（一般origin=主仓库，upstream=上游仓库）
git remote add 名称 仓库URL
# 例如，git remote add origin https://github.com/company/project.git
# 查看所有远程仓库（-v显示URL详情）
git remote -v
# 查看特定远程仓库详情
git remote show 名称 
# 重命名远程仓库
git remote rename 旧名称 新名称
# 删除无效远程仓库
git remote remove 名称
```

### 5.2、远程分支映射

- 查看远程分支（git branch -r）  

  ```bash
  git branch -r         # 列出所有远程分支
  git ls-remote 远程仓库 # 查看远程仓库的完整引用
  ```

- 设置跟踪分支（--set-upstream-to）

  ```bash
  # 推送新分支时设置跟踪
  git push -u 远程仓库 新分支名  
  # 手动设置跟踪关系
  git branch --set-upstream-to=远程仓库/远程分支名
  ```

- 创建/删除远程分支  

  ```bash
  # 推送新本地分支
  git push 远程仓库 新分支名  
  # 删除远程分支
  git push 远程仓库 --delete 远程分支名
  ```

### 5.3、基础协作工作流

**先拉取代码，再开发代码，最后推送代码**  

- 先拉取代码，确保在最新代码的基础上进行开发，避免覆盖他人的代码
- 在开发代码的时候不拉取代码，除非明确知道远程出现bug，或和其他人修改了同一文件
- 适当分散提交，不要将大量修改内容集中到一次提交
- 最后提交推送到特性分支，通过PR审核再合并到主分支或对应分支

```bash
# 协作顺序：
git pull 远程仓库 main   # 1. 先拉取最新主干代码（解决冲突）
# ... 开发/修改代码 ...
git add .              # 2. 添加修改
git commit -m "fix"    # 3. 本地提交
git push 远程仓库 特性分支名 # 4. 推送到自己的特性分支
```

**分支策略**：

```bash
# 从主分支创建特性分支
git checkout -b feature/login
# ... 开发 ...
# 推送到远程特性分支，而不是直接推送到主分支
git push -u origin feature/login
```
常见的分支模型如下

![常见分支模型](https://Mecamodore.github.io/picx-images-hosting/常见分支模型.5q7wu089l2.webp)

### 5.4、拉取代码（git fetch、git pull）

```bash
git fetch 远程仓库        # 仅获取远程更新，不合并
# 查看远程更新内容
git log -p 远程仓库/远程分支名      # 查看具体变更
git diff 远程仓库/远程分支名        # 检查与本地差异
# 拉取并自动合并
git pull origin main    # 相当于git fetch加git merge
git pull --rebase origin main  # 相当于git fetch加git rebase
```

### 5.5、推送代码（git push）

```bash
# 推送当前分支到跟踪的远程分支
git push
# 推送指定分支
git push 远程仓库 分支
# 推送标签，默认标签是不推送的
git push 远程仓库 标签名     # 推送单个标签
git push --tags             # 推送所有本地标签
```

### 5.6、解决代码冲突

在拉取代码合并时，容易出现冲突

Git会提示哪些文件发生冲突

例如下面这样一段输出
```bash
<<<<<<< HEAD
console.log("本地修改");
=======
console.log("远程修改");
>>>>>>> origin/main
```

保留需要的代码，删除其它内容，例如：

```bash
console.log("本地+远程");
```

解决完冲突之后，再添加文件到暂存区，再提交代码即可

## 6、简单使用GitHub

GitHub是一个基于Git的代码托管平台，目前已被微软收购

各大厂商有自己的代码托管平台

这一类的平台功能上大部分是通用的，但各家可能会推出自己独有的功能

或者和自家产品形成联动之类的

GitHub官方使用文档如下

https://docs.github.com/zh

## 7、简单使用Sourcetree

Sourcetree是一个Git图形界面工具，这类工具有很多

使用图形化界面的工具相比于命令行会更直观

在学习分支时，图形化界面也能更好地帮助我们理解

Sourcetree的官方文档如下

https://confluence.atlassian.com/get-started-with-sourcetree/get-started-with-sourcetree-847359026.html
