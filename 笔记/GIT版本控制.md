## GIT:版本控制

### 1、基本配置

> git config -l

配置信息

删除：直接删除gitconfig文件

> 系统在安装目录下的gitconfig中，本地在c盘中

配置用户

> git config --global user.name "用户名"
>
> git config --global user.email "2934497498@qq.com"

### 2、Git基本理论

+ Workspace:工作区，平时存放代码的地方

+ Index/Stage:暂存区，临时存放你的改动，一个保存文件列表信息的文件

+ Repository:仓库区，安全存放数据的地方

+ Remote:远程仓库，托管代码的服务器

  

![5](C:%5CUsers%5CGRC2001%5CDesktop%5C5.png)



![6](C:%5CUsers%5CGRC2001%5CDesktop%5C6.png)

**需要记住的六个命令**



+ 本地仓库搭建

  > git init ->初始化项目
  >
  > git clone [url] ->克隆一个远程项目到本地
  >
  > 

+ 文件的四种状态

  > untracked: 未跟踪，通过git add状态变为 Staged
  >
  > Unmodify: 文件已经入库，未修改，修改后变为Modified，git rm 后，成为Untracked
  >
  > Modified: 文件已经修改，通过git add 进入Staged,通过git checkout则丢弃修改过的，回到 Unmodify，即从库中取出文件，覆盖当前修改
  >
  > Staged: 暂存状态，执行git commit 则将修改同步到库中，此时库中的文件与本地文件一致，文件为unmodify状态，执行git reset HEAD filename 取消暂存，文件状态为Modified

> git status  -> 文件状态
>
> git add .  ->提交到暂存区
>
> git commit -m -> 提交暂存的东西，-m 提交信息

+ .gitignore

  ```c++
  *.txt #忽略所有以.txt结尾的文件
  !lib.txt #除了lib.txt以外都忽略
  /temp #不忽略目录temp下的文件
  build/ #忽略build/目录下的所有文件
  doc/*.txt #仅忽略doc目录下的.txt文件，不包括它的子目录
  ```

+ 使用github

  免密码登录SSH：

  > 生成公钥：ssh -keygen -t rsa

  github创建仓库并克隆到本地

  

  