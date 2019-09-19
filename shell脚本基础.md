shell脚本基础

1、bash的基本特性
	a、history 命令历史
				查看之前的命令的历史记录

		扩展知识   数组：一段连续的内存空间
				  变量：一段被命名的内存空间
		【环境变量：bash程序下默认能够生效的变量，不需要定义；】
		查看：env  set
			env 只能查看系统支持的部分环境变量
			set 查看所有环境变量，同时set可以设置变量
			但是，如果要使用 export 将 set 设置变量导入到环境变量中
			--- 以上都是临时的！
		/etc/profile   指定环境变量
		/etc/rcbash		用来存储普通变量
		~/.profile    
		~/.rcbash
		source让其生效
		exec 【剥离子进程，单独创建父进程】
		变量：一段被命名的内存空间；
	
		history所定义的环境变量
			HISTFILE=/root/.bash_history
			HISTFILESIZE=500
			HISTSIZE=500
		history
			-d  删除指定行
			-c  清除所有记录
			-a  将当前的记录信息，添加到/root/.bash_history 中去；
		history 命令的调用方式
			!#  指定调用哪一行命令
			!string  匹配最近一次的字符串  
			!!  上一条命令
			*!$*  上一条命令的最后的一个参数
				ls !$ == ESC+ . 
	b、命令补全、目录补全
		tab键实现命令和目录的补全；
		扩展：
		PATH路径：命令被查找的一个默认路径；
			PATH=$PATH:/usr/bin
		如果执行文件不再PATH路径下，只能使用全路径进行执行；
	c、命令展开—date命令
		cp /usr/local/nginx/nginx.conf /usr/local/nginx/nginx.conf.bak
		cp /usr/local/nginx/{nginx.conf,nginx.conf.bak}
		创建1_a 1_b 2_a 2_b 可以写成touch {1,2}_{a,b}
		时钟分为系统时钟和硬件时钟
		date  --  显示当前系统时钟 
	    	拓展：ntp服务器：进行计算机时钟同步
				 clock、hwclock（可以做时钟同步）
				 cal 显示本月日历 
		date用法：
				date [option] ... [+format]
			格式：
				%F   2019-09-16
				%D  09/16/19
				%T   15:55:30
				%H  小时
				%S   秒  %s  19700101至今
				%M  分
				%m  月份
		压缩和归档
	 		归档：把多个文件组合到一个文件中。归档好处是，把文件数目变少，有利于降多个文件作为电子邮件附		  件发送，以及备份文件。
			压缩：利用算法将文件有损或无损地处理，以达到保留最多文件信息，而令文件体积变小。压缩好处就是		  节约硬盘空间，以及减小电子邮件附件的大小，提高传输效率
			gzip    ungzip   zcat 
			bzip2
			xz
			【注意：压缩操作只能对文件进行】
			tar  --  本身只做归档
			j(bzip2) J(xz) z(gzip) x(解压) c(归档) v(详细信息) f(指定创建的压缩文件的文件名)
			tar cjvf etc.bak.bzip2 /etc/
			tar xf 解压文件  //提示：解压不要指定解压工具，可以自动是被
			练习：每天周期性备份/etc/文件，并取号名字；
				crontab做定时任务
				crontab -e
	    00 00 * * * /bin/tar cjvf /root/etc.bak`/bin/date +%F`.bzip2 /etc/ &> /dev/null
	   分/小时/几号/几月/星期几（*/*/*/*/*）	
	d、命令的执行状态
		命令执行后，有两类返回值：1、命名的默认返回值 2、命令的执行状态
		状态有两类：
			1、 0  --  代表命令执行成功
			2、1-255  -- 均代表执行失败
			可以自己定义返回值
		$? -- 一个特殊的环境变量。用来存储上一条命令的执行状态；
			ls /
			[ $? -eq 0 ] && exit 0 || exit 1
	e、命令的快捷键
		ctrl + a  光标移到行首
		ctrl + e  光标移到行尾
		ctrl + u  删除光标前的内容
		ctrl + k  删除光标后的内容
		ctrl + w  删除光标所在处之前的一个词
		ctrl + shift + t  //新建图形终端
		ctrl + l  清屏
		ctrl + s  锁屏
		ctrl + q  解锁
		ctrl + d   //关闭当前bash程序
		ctrl + r   //和 !string 调用最近一次匹配 string 的命令
	f、alias别名
		取外号 -- 为了方便命令的调用，取一些容易记忆的名字；
		alias cls='clear'  //注意，等号前后没有空格；
		/etc/profile   //指定环境变量
		/etc/rcbash   //用来存储普通变量
	    ~/.profile   (~/当前家目录)
		~/.rcbash
	g、文件通配符  -- globbing
		文件通配符
		*        匹配任意长度的任意字符（不能匹配空白符）
		？      匹配任意单个字符；
		[]       [0-9]  [a-z]  [A-Z]  [a-zA-Z]  [0-9a-zA-Z]
		[^]     [^0-9]
		符号集合   [:digit:]（0-9）, [:graph:]（除空格与[tab]之外所有按键）, [:lower:]（a-z）, 					  [:print:], [:punct:]（^0-9a-zA-Z）, [:space:]（空格符）, [:upper:]（A-Z）,  			   [:xdigit:]（A-Fa-f0-9）(十六进制)
	        	  [:alpha:]（a-zA-Z）， [:alnum:]（0-9a-zA-Z）
			注意：在使用的时候需要再加中括号 [[:digit:]] = [0-9]
			注意：文件通配符globbing都是用来匹配文件名的；

练习：
	1、显示/var目录下所有以1开头，以一个小写字母结尾，且中间出现至少一位数字的文件或目录；
		ls /var/1*[0-9]*[[:lower:]]
	2、显示/etc目录下，以任意一位数字开头，且以非数字结尾的文件或目录
		ls /etc/[0-9]*[^0-9]
	3、显示/etc目录下，以非字母开头，后面跟了一个字母及其它任意长度任意字符的文件或目录
		ls /etc/[^[:alpha:]][[:alpha:]]*
	4、复制/etc目录下，所有以m开头，以非数字结尾的文件或目录至/tmp/test目录中；
		cp -a /etc/m*[^0-9] /tmp/test
	5、复制/etc目录下，所有以.d结尾的文件或目录至/tmp/test.com目录中
		cp -a /etc/*.d /tmp/test.com
	6、复制/etc目录下，所有.conf结尾，且以m n r p开头的文件或目录至/tmp/test.com目录中
		cp -a /etc/[mnrp]*.conf /tmp/test.com

