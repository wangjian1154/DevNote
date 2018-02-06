- Linux默认有6个命令交互通道和一个图形交互通道
	通过Ctrl+Alt+F1-Ctrl+Alt+F7

- 命令交互模式
	wj@wj-VirtualBox:~$
	-wj:用户名
	-wj-VirtualBox:主机名
	-~:路径
	-$:用户的类型，$代表普通用户.#代表超级用户

- Linux的目录结构</br>
		1.bin:存放可执行的二进制文件</br>
		2.boot:存放系统的引导文件的目录</br>
		3.dev:存放设备文件的目录，Linux把设备当做文件来处理</br>
		4.etc:存放系统的配置文件的目录</br>
		5.home:存放所有用户文件的根目录</br>
		6.lib:共享库</br>
		7.user:相当于win的program files，存放应用的安装路径</br>
		8.opt:自定义存放应用程序的位置</br>
		9.mnt:临时文件系统的挂靠点</br>

- 文件的权限</br>
	w:可写	r:只读	x:可执行  -:无权限</br>
	- 文件权限的字符表示法
		drwxr-xr-x
			第一个符号d表示普通文件，-表示文件夹，c表示串口文件，l表示连接文件
			第2-4字符:该文件的属主用户的权限
			第5-7字符:与属主用户同一组的其他用户的权限
			第8-10字符：不同组的其他用户的权限
	- 文件权限的数组表示法</br>
		-rw-r--r:文件的默认权限644</br>
		drwxr-xr-x:目录的默认权限755</br>
		</p>
- 常用命令</br>
	安装软件(如vim)</br>
	```
	sudo apt-get install vim
	```

	卸载 (如vim)</br>
	```
	sudo apt-get install synaptic
	```

	退出</br>
	```
	exit
	```

	关机</br>
	```
	shuntdown -h now
	```
	重启</br>
	```
	restart
	```

	超级用户执行
	```
	sudo(superuser do)
  ```
	查看
	```
	ls //查看目录内容
	ls -l //查看详细信息
	ls -a //查看所有文件，隐藏的文件
	```
	命令意思查看、帮助
	```
	man ls//man后面跟上需要查看的命令
	```

	创建目录
	```
	mkdir 目录名称
	```

	代码补全
	```
	Tab
	```
	创建一个空白的普通文件
	```
	touch aa.txt
	```
	---
	echo “content” aa.txt
>将内容重定向到指定的文件中，有就打开它，没有就创建他

	cat
> 查看文件的内容

	cp、mv、rm
> 分别是拷贝、剪切、删除
