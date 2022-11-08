###  1.1
使用git在笔记本电脑和主机间进行obsidian笔记同步
在主机进行`git push origin master`后笔记本电脑`git pull gitee master`发生unmerge错误
使用`git status`进行故障查询
![[Pasted image 20220402150621.png]]

解决方法：
* 我需要远程分支，本地分支较为老旧，进行丢弃
```shell
git reset --hard FETCH_HEAD
```

![[Pasted image 20220402151328.png]]
FETCH_HEAD 为远程分支最新版本

* 不能丢弃本地更改，先对unmerge文件进行手动修改，修改掉冲突部分后
```shell
git add filename
git commit -m "message(unmerge)"
```

![[Pasted image 20220402151430.png]]
* 废除本次合并
```shell
git reset --hard HEAD
```

### 1.2 丢弃工作区修改
```shell
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
 
	modified:   readme.txt
 
no changes added to commit (use "git add" and/or "git commit -a")
```
readme.txt加入了暂缓区中，而要丢弃工作区的修改
```shell
$ git checkout -- readme.txt
```

### 1.3
![[Pasted image 20220509105942.png]]


![[Pasted image 20220509110008.png]]

![[Pasted image 20220509110017.png]]
![[Pasted image 20220509110028.png]]

![[Pasted image 20220509110038.png]]



## 1.4
删除git文件
进入所在文件夹，打开git bash
```bash
$ find . -name ".git" | xargs rm -Rf
```
或者
```bash
$ rm -rf .git
```

## 1.5 github删除远程仓库
![[Pasted image 20221030104209.png]]
![[Pasted image 20221030104217.png]]
![[Pasted image 20221030104223.png]]
![[Pasted image 20221030104229.png]]

### 1.5
win11系统中git运行脚本报错
![[Pasted image 20221103095116.png]]
问题：没有安装wget工具

解决：去官网下载
地址：[GNU Wget 1.21.3 for Windows (eternallybored.org)](https://eternallybored.org/misc/wget/)
将其放入安装Git目录下 ./Git/mingw64/bin下
