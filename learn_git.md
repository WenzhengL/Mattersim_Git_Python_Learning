## 简介
git学习笔记。主要基于linux集群环境下运行git，同时与github联动学习代码的团队开发流程。

## git基本概念
git是是目前世界上最先进的分布式版本控制系统。
1. 版本控制系统：记录文件每次修改的版本，方便退回或对比版本差异，同时支持他人协同编辑。
2. 分布式：与集中式版本控制系统（如SVN和CVS）相对，每个个人电脑都有完整的版本库，优势之一在于不需要要求联网到中央服务器。
3. git的三大工作区
	- 工作区 (Working Directory)：你当前在编辑的真实文件所在的位置。`git status` 查看修改状态
	- 缓存区 (Staging Area)：暂存准备好下一次提交的文件快照（并不是提交）`git add <file>` 添加进缓存区
	- 本地仓库 (Local Repository)： `.git` 目录下保存的对象数据库，记录历史提交，`git commit` 提交到本地仓库
	![alt text](/image/image2.png)
4. 开始git代码管理有两种开始方式，后续操作也从这两类开始介绍：  
	- 从clone远程仓库开始
	- 从init本地仓库开始

## 注意注意
## PUSH失败时请检查网络
## 切换分支前先交提本地的修改


## 问题收集处
 1、json格式文件中不允许出现注释，但是在vscode的setting文件中注释可以正常存在
 2、VScode插件似乎修正了Markdown在预览中的换行规则，在github界面换行失效，部分如图：
 ![alt text](./image/image.png)  
	- 策略1：多空一行  
	- 策略2：在句子结尾处打2个空格，确保在github界面正常显示

## git的安装和初始配置
到git官网下载git并安装,安装配置第一步，查看git文档：[Git - First-Time Git Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup) 在命令行输入（提前注册好的github账户，这里的用户名似乎不需要一致）：
```
$ git config --global user.name "WenzhengL"
$ git config --global user.email 1849153430@qq.com
```
## git基础操作指令
**查看工作区和暂存区修改的状态**：`git status`，输出如下  
![alt text](/image/image3.png)  
**添加工作区内容到暂存区**：`git add xxx`, 添加一个或多个文件到暂存区  
**交提暂存区到本地仓库**：`git commit -m '注释内容'`，注释内容用以说明交提的修改内容  
**查看交提日志**：`git log [option]`，以看到commitID，用以回退：  
![alt text](/image/image4.png)  
**版本回退**：`git reset --hard commitID`，回退后如何查看已经被删除的记录：`git reflog`，查看commitID后可以退回到该版本  
**添加文件到忽略列表** 在工作目录中可能存在不希望被track的文件，这是可以创建`.gitignore`文件，可以列出需要忽略的文件模式。下面是一个示例：  
```
# no .a files
*.a
# but do track lib.a, even though you're ignoring .a files above
!lib.a
# only ignore the TODO file in the current directory, not subdir/TODO
/TODO
# ignore all files in the build/ directory
build/
# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt
# ignore all .pdf files in the doc/ directory
doc/**/*.pdf
```
## 分支
**查看本地分支** `git branch`  
**创建本地分支** `git branch 分支名`  
**切换分支** `git checkout 分支名` & 创建并切换到新分支`git checkout -b 分支名`  
**删除分支** 检查后删除分支：`git branch -d 分支名`，不做检查删除分支`git branch -D b1`。删除都不能删除当前分支，只能删除其他分支  

## 解决冲突
两个分支对文件的修改可能会存在冲突，例如同时修改了同一文件的相图位置，需要手动解决。步骤如下：  
1. 处理文件中冲突的地方，选择其中一种，删除其余符号  
2. 将解决完冲突的文件加入暂存区`add`  
3. 提交到仓库`commit`  

## 远程仓库
**添加远程仓库** 先初始化本地仓库后与已创建的远程仓库进行对接，命令：  
`git remote add <远端名称> <仓库路径>`  
**远程名称** 远程名称是指向某一个远程仓库的名称，以方便用户在本地仓库与远程仓库间进行交互，默认名称为`origin`，直接clone远程仓库是时会自动创建一个名为`origin`的远程名称。  
**查看远程仓库** `git remote -v` 查看远程仓库极其对应的URL  
**修改远程名称** `git remote rename <当前名称> <修改名称>`  
**删除远程名称** `git remote remove <仓库名称>`  
**推送到远程仓库**	`git push [-f] [--set-upstream] [远程名称] [本地分支名] [远程分支名]`  
-f代表强制覆盖，如果远程分支名与本地分支名相图可以指写本地分支名（已经关联的远程分支名也可以不写），--set-upstream表示推送到远程仓库并建立和远程分支的关联  
**查询本地分支与远程分支的关联** `git branch -vv`  


**克隆远程仓库** `git clone <仓库路径> <本地目录>`克隆已经有的远程仓库到本地，本地目录可以省略，会自动创建  

**从远程仓库抓取和拉取** 
1. 抓取 `git fetch <远程名称> <分支名称>` 抓取指令就是将仓库里的更新都抓取到本地，不会进行合并，如果不指定远程名称和分支名，则抓取所有分支  
2. 拉取命令 `git pull <远程名称> <分支名称>` 拉取指令就是将远端仓库的修改拉到本地并自动进行合并，等同于fetch+merge。
fetch命令会创建一个本地的远程跟踪分支，可以用命令`git branch -r`查看  



## 从clone远程仓库开始
### `ssh`生成密钥
1、在命令行输入：`ssh-keygen -t rsa -b 4096 -C "wenzheng-git"`生成公钥和密钥
ssh-keygen为生成密钥的命令行工具，-t rsa为指定加密方式为rsa，-b 4096指定密钥长度（越长越安全，默认是 2048），-C "wenzheng-git"添加一段注释，用于标识密钥。
![image.png](/image/image_1747214815093_0.png)  
在屏幕输出的路径中复制id_rsa.pub文件中的全部内容：/public/home/wenzheng/.ssh/id_rsa.pub
在github中的ssh密钥中加入上面复制的密钥

### 安装git
1、登陆到集群后直接命令行输入：`git`  
2、显示一大串：`usage: git [--version] [--help]...`说明已经安装了git，如果屏幕返回未安装，可以使用`sudo apt install git`等方式安装git

### 将repo库clone到本地
1、下载库`git clone git@github.com:WenzhengL/Mattergem_Git_Python_Learning.git`
首次链接github库可能出现如下警告，是正常情况：  
![image.png](/image/image_1747214753984_0.png)  
2、对于大的代码库，可以在命令中添加 `--depth 10`，目的是只下载最近十次更新的结果  
3、同时，对于大的代码库，对于很久远的分支，需要更新子模块`git submodule update --init --recursive`
4、git status 查看状态,确定一下是否有没有track的子模块  
![image.png](/image/image_1747215518572_0.png)

### 创建新的分支
创建分支learngit：`git checkout -b learngit`  
将本文件添加到项目目录  
添加md文件到缓存区：`git add learn_git.md`  
`git commit -m "Add markdown file for learning Git"`  
将更改push到Github：`git push origin learngit`

### 用命令行将learngit分支merge到主分支
切换到主分支`git checkout main`  
尝试先拉取主分支内容防止存在冲突`git pull origin main`  
合并learngit分支`git merge learngit`  
推送到远程仓库`git push origin main`  

### 上传图片
上次上传后，无法正常显示图片，补充上传：  
`git add image/image_ssh_key.png image/image_git_status.png image/image_git_clone.png`  
修改md文件中对应图片引用的路径：`![SSH Key](image/image_ssh_key.png)`  
`git commit -m "Add tutorial images to image/ directory"`  
`git push origin learngit`  
push后有显示警告：  
![image.png](/image/image_1747578357670_0.png)  
尝试用token密钥连接后仍push失败，而后尝试重新clone仓库，连接失败：  
![image.png](/image/image_1747618713079_0.png)

**此处省略大量无意义尝试·····最后检查是网络问题**

## 从init本地仓库开始
从任意的本地文件开始，在命令行输入`git init`，创建成功后可以看到`.git`文件夹





## 尝试VScode+Git+Github
### 放弃集群git原因
1、公共集群连接不稳定，询问管理员后告知集群开始屏蔽网络连接以确保安全  
2、个人习惯编辑文件和上传文件较为不便

### 安装多个版本的python解释器
似乎是为了后续基于此创建对应的虚拟环境，但我已经安装conda，可以很方便的切换
python3.10
python3.12

### 下载安装VScode
直接官网下载安装：[Visual Studio Code - Code Editing. Redefined](https://code.visualstudio.com/)

### VScode初始配置
1、在插件中安装python使得解释器能够使用，右下角可以选择python解释器版本  
2、修改TERMINAL改为默认命令行工具：commend prompt  
3、在setting中找到字体font的setting文件，添加：`"editor.mouseWheelZoom": true`使得按住ctrl即可用鼠标滚轮调整字体大小；也可以顺便加一句补全命令并增加括号：`"python.analysis.completeFunctionParens": true`  
4、安装python-snippets （类似的似乎很多，选择了作者为cstrap的版本），功能主要是快速帮助定义函数和类等，使用方法举例，预输入def，出现备选列表，回车选择第一个def即帮助补完，函数名等位置被色块占满，直接输入可以修改第一个参数如函数名，完成后按`tab`键即可转移到下一个参数输入。  
自定义snippets，在左下角点击齿轮即可找到，模版：
```
"quick Print": {
"prefix": "pr",    // 输入的内容，似乎会排在备选列表首位
"body": [
	"print('$1')"  // $1的位置即光标位置
],
"description": "Log output to console"
}
```
5、虚拟环境配置：`python3.10 -m venv .venv` 根据解释器版本进行虚拟环境创建

### Git下载和配置
到git官网下载git并安装,安装配置第一步，查看git文档：[Git - First-Time Git Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup) 关键命令：
```
$ git config --global user.name "WenzhengL"
$ git config --global user.email 1849153430@qq.com
```
尝试commit当前文件夹，发现非常卡顿，发现原因在于创建的虚拟环境`venv`处在当前文件中，提交的文件量极大，删除虚拟环境后，重新更改虚拟环境目录到`../venv`中。  
更改后顺利将在本地创建新repo到github项目`git_demo`  
在graph界面可以很清晰的显示本地仓库与远程仓库的状态： ![image.png](/image/image_1747661656137_0.png)
查看Git日志：` git log `  
![image.png](/image/image_1747662006490_0.png)
push上传时再次遇到报错：fatal: unable to access 'https://github.com/WenzhengL/Mattersim_Git_Python_Learning.git/': Empty reply from server  
尝试git pull，仍报错如下：  
git pull origin learngit
error: RPC failed; curl 28 Recv failure: Connection was reset  
fatal: expected flush after ref listing  

**传Git需要一个好梯子!**
多次更换VPN节点和订阅后成功上传，之前集群的报错也是网络问题。

### 在VScode中快速编写markdown文件
#### 准备插件：
1、Markdown All in One，其中包含一些markdown的常用工具
2、Markdown Preview Github Styling，预览
3、Markdown 作者：starkwang 功能：复制任意图片后，快捷键`ctrl+alt+v`直接粘贴图片

### VScode快捷键
删除注释：`ctrl`+`/`
调出命令输入框：`ctrl`+`shift`+`P`，可以查找文件、更改设置和输入插件命令等