###bash常用命令
-----
###### 注意： 以下统一用dir指代某一具体的目录名
	1. cd dir   #切换目录 
	2. ls dir   #查看内容
		1. ls -a   #查看当前目录下的所有文件内容列表
		2. ls -al  #查看当前目录下的所有文件内容的详细信息列表
	3. mkdir dir  #创建目录
		如： mkdir css
	4. touch fileName  #创建文件
		如： touch index.html
	5. cat fileName  #查看文件内容
	6. rm fileName  #删除文件
	7. clear  #清屏
###git配置相关
	# 显示当前的git配置
	$ git config --list

	# 设置提交代码时的用户信息
	$ git config [--global] user.name "[username]"
	$ git config [--global] user.email "[email]"
###git常用命令  
-----
######初始化本地仓库
	01. git init  #初始化远程仓库
	02. git clone URL ./  #克隆远程仓库 
		其中，URL就是远程仓库的地址  ./表示的是克隆到当前文件夹，不再重新创建文件夹
#####分支
	01. git branch  #列出本地已经存在的分支，并且在当前分支前面加*号标记
		git branch -r	#列出远程分支
		git branch -a   #列出本地分支和远程分支
	02. git branch 分支名  #基于当前分支创建子分支
	03. git checkout 分支名  #切换分支  
		git checkout -b 分支名  #创建分支并切换到该分支
		git checkout -b dev origin/dev  #作用是checkout远程的dev分支，在本地起名为dev分支，并切换到本地的dev分支
	04. git merge 被合并的分支名  #合并分支，这里是将目标分支合并到当前执行环境下的分支上(主支)
	05. git branch -d 要删除的分支名  #删除分支
#####代码提交
	01. git add dir1,dir2...dirn  #把文件添加到本地暂存区
		其中dir1~dirn表示的是要添加的文件或是文件夹名称(需要是绝对路径)，可以使用命令 git add * 将所有更改过的文件添加到暂存区
	02. git commit -m '版本描述'  #把文件添加到永久区
	    git commit -am '版本描述'  #把已追踪过的文件添加到暂存区然后再添加到永久区
	03. git push 远程仓库地址或别名 分支名  #将当前分支历史版本推送到远程仓库某个分支上
		git push origin 分支名 -f   #强制push，一般用于版本回退后的强制push
		git push [remote] --force  #强行推送当前分支到远程仓库，即使有冲突
		git push [remote] --all    #推送所有分支到远程仓库
#####撤销
	01. git remote  #查看已添加的主机名称
	02. git remote add 主机名 远程仓库地址  #给远程主机添加一个别名
	03. git reset  #回退历史版本：
	    1. git reset --mixed commitID --mixed是默认方式，除了代码修改保留外其他的都还原，包括git commit和git status里面的内容。
	    2. git reset --soft commitID  --soft只是回退了commit的信息，如果还要提交，直接commit即可
	    3. git reset --hard commitID  --hard是彻底回退到某个版本本地的源码也会变为上个版本的内容，包括代码，git add后的内容以及git commit里面的内容  
#####查看信息
	# 查看文件状态（未追踪的和已修改的是红色，已暂存的是绿色）
	$ git status  

	# 查看当前版本之前的历史版本
	$ git log  
    # 将查询到的历史版本信息压缩到一行显示
	$ git log --oneline 
	# 上帝视角查看文件历史版本的变化(所有的操作记录)	
	$ git reflog  

	# 显示所有提交过的用户，按提交次数排序
	$ git shortlog -sn

	# 显示某次提交发生变化的文件
	$ git show --name-only [commit]
#####标签
	01. git tag  #查看已有的标签
		git tag -l 'v1.4.*'  #git tag支持简单的正则表达式，这里是查看所有1.4.x版本的标签
		git tag 'v1.4'  #创建一个轻量级标签
		git tag -a v1.4 -m 'my version 1.4'  #创建一个含附注的标签，其中-a指定标签名字，-m指定对应的标签说明
	02. git show 标签名  #查看标签信息
	03. git push origin tagName  #推送标签，其中origin可以是远程仓库地址或别名，tagName是标签名
		默认情况下，git push不会把标签传送到远	端服务器上，只有通过显示命令才能将标签分享到远程仓库。
		如果要一次推送所有本地新增的标签上去，可以使用git push origin --tags
	04. git checkout tagName  #切换到标签
	05. git tag -a v1.0.0 -m 'version describe' commitID  #给指定的commit补打标签
	06. git tag -d 标签名	#删除本地标签
	07. git push origin -d tag 标签名	#删除远程服务器标签，git v1.7.0以上支持
#####远程同步 
	# 将远程仓库某个分支上的历史版本拉取到本地 
	$ git pull 远程仓库地址或别名 分支名  
	
	# 下载远程仓库的所有变动
	$ git fetch [remote]