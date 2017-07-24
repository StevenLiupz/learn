###bash常用命令
-----
###### 注意： 以下统一用dir指代某一具体的目录名
	1. cd dir   切换目录 
	2. ls dir   查看内容
		1. ls -a 查看当前目录下的所有文件内容列表
		2. ls -al 查看当前目录下的所有文件内容的详细信息列表
	3. mkdir dir   创建目录
		如： mkdir css
	4. touch fileName  创建文件
		如： touch index.html
	5. cat fileName  查看文件内容
	6. rm fileName  删除文件
	7. clear  清屏
###git常用命令  
-----
	1. git init 初始化远程仓库
	2. git clone URL ./  克隆远程仓库 
		其中，URL就是远程仓库的地址  ./表示的是克隆到当前文件夹，不再重新创建文件夹
	3. git add dir1,dir2...dirn 把文件添加到本地暂存区
		其中dir1~dirn表示的是要添加的文件或是文件夹名称，可以使用命令 git add * 将所有更改过的文件添加到暂存区
	4. git commit -m '版本描述'  把文件添加到永久区
	   git commit -am '版本描述' 把已追踪过的文件添加到暂存区然后再添加到永久区
	5. git status  查看文件状态
		未追踪的和已修改的是红色，已暂存的是绿色
	6. git remote  查看已添加的主机名称
	7. git remote add 主机名 远程仓库地址  给远程主机添加一个别名
	8. git log  查看当前版本之前的历史版本
	   git log --oneline  将查询到的历史版本信息压缩到一行显示
	   git reflog  上帝视角查看文件历史版本的变化(所有的操作记录)
	9. git reset  回退历史版本：
	   1. git reset --mixed commitID --mixed是默认方式，只保留源码 
	   2. git reset --soft commitID  --soft只是回退了commit的信息，如果还要提交，直接commit即可
	   3. git reset --hard commitID  --hard是彻底回退到某个版本本地的源码也会变为上个版本的内容
	   
	   