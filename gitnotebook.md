# 1 git简介

## 1.1 安装git

直接在网站上下载，安装好后打开git bash出现命令行说明安装完成
然后输入两行代码：

`git config --global user.name "Your Name"`
`git config --global user.email "email@example.com"`
因为git是分布式版本控制系统，所以这个操作就是自报家门



## 1.2 创建版本库

初始化一个Git仓库，使用`git init`命令。

添加文件到Git仓库，分两步：

- 使用命令`git add <file>`，注意，可反复多次使用，添加多个文件； 
- 使用命令`git commit -m <message>`，完成。



# 2 时光穿梭机

## 2.1 查看状态

- 要随时掌握工作区的状态，使用`git status`命令。
- 如果git status告诉你有文件被修改过，用`git diff`可以查看修改内容。



## 2.2 版本回退

- HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。



## 2.3 管理修改

每次修改，如果不用git add到暂存区，那就不会加入到commit中。



## 2.4 撤销修改

- 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

- 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

- 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。



## 2.5 删除文件

命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。





# 3 远程仓库

1. 首先需要一个GitHub账号。

2. 由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

   2.1 创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

   ```
   $ ssh-keygen -t rsa -C "youremail@example.com"
   ```

   你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

   如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

   2.2 登陆GitHub，打开“Account settings”，“SSH Keys”页面：

   然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：



## 3.1 添加远程库

1. 首先登陆GitHub，然后创建一个新的库repository

2. 输入库名，其余默认

3. 然后将本地库与之关联，再推送内容到远程库：

   要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

   关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

   此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

   分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！



## 3.2 从远程库克隆

要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

Git支持多种协议，包括`https`，但通过`ssh`支持的原生`git`协议速度最快。