#1. 如何修改hostname

	vim /etc/hostname 修改即可

#2. sudo： 无法解析主机  

这个问题一般会出现在修改了 hostname 之后，解决办法就是将/etc/hosts
目录下的主机名一起修改。

	vim /etc/hosts

#3. Ubuntu 下配置jdk

1. 下载jdk
2. chmod +x jdk-6u38-linux-i586.bin
3. ./jdk-6u38-linux-i586.bin
4. mkdir -p /usr/java
5. mv jdk1.6.0_38 /usr/java
6. vim /etc/profile
		
	JAVA_HOME=/usr/java/jdk1.6.0_38
	CLASSPATH=.:$JAVA_HOME:/lib/tools.jar:/lib.dt.jar
	PATH=$JAVA_HOME/bin:$PATH
	export JAVA_HOME CLASSPATH PATH

#4. 无法获取锁

	sudo rm /var/cache/apt/archives/lock
	sudo rm /var/lib/dpkg/lock

#5. 找不到软件源

	sudo apt-get update


#6. cron 自动关机 || disable power management

首先，如果你在命令行关机，不能完全关机，系统提示disabling power
management，则执行下面的语句：

	gsettings set org.gnome.settings-daemon.plugins.power active false  

然后再试试：

	sudo shutdown now  


然后用crontab写计划任务，由于关机需要root权限，所以应该将关机命令写到超级用户的crontab中。
执行：

	sudo crontab -e  

输入密码，在最后一行键入：

	00 00 * * * sudo -u root shutdown -h 1  

意思是在凌晨关机，可以自己修改。保存并退出以后，再重启服务：

	sudo restart cron  


#7.  mutt gmail msmtp ubuntu 命令行发送邮件

安装 mutt 和 msmtp：

	sudo apt-get install mutt msmtp

修改配置文件

	vim ~/.msmtprc

输入以下内容：

	account default
	host smtp.gmail.com
	from gmailuser@gmail.com
	tls on
	auth on
	port 587
	user gmailuser@gmail.com
	password xxxxxx
	#logfile /var/log/msmtp.log
	logfile ~/.msmtp.log
	tls_certcheck off

更改文件权限，主要是防止密码泄漏

	chmod 600 ~/.msmtprc

修改muttrc

	vim ~/.muttrc

输入以下内容：

	set pop_last=yes
	set pop_pass=xxxxxx
	set pop_user=gmailuser@gmail.com
	set pop_host=pops://pop.gmail.com
	set sendmail="/usr/bin/msmtp"

更改文件权限

	chmod 600 ~/.muttrc

测试

	echo 'test mutt+msmtp'|mutt -s 'test mutt+msmtp' user@gmail.com -a attatch.file

#8. 帮你的vim找回它的<F10>

* first, disable global F10 shortcut

		mkdir -p ~/.config/gtk-3.0
		cat<<EOF > ~/.config/gtk-3.0/gtk.css
		
		@binding-set NoKeyboardNavigation {
		     unbind "<shift>F10"
		}
		
		* {
		     gtk-key-bindings: NoKeyboardNavigation
		}
		
		EOF

* now disable gnome-terminal's F10 shortcut:
	In a gnome-terminal open up Keyboards Shortcuts (Edit --> Keyboard Shortcuts) and uncheck Enable the menu shortcut key (F10 by default)

#9. ubuntu安装MySQL-Python出现mysql_config not found报错解决

配置MySQL-Python的时候系统报错，提示：

	EnvironmentError: mysql_config not found

Google后得知mysql_config是属于MySQL开发用的文件，而使用apt-get安装的MySQL是没有这个文件的，于是在包安装器里面寻找

	sudo apt-get install libmysqld-dev
	sudo apt-get install libmysqlclient-dev

这两个包安装后问题即可解决

#10. ubuntu 下matplotlib 输出显示中文

matplotlib 是python
的第三方库，显示中文不比较麻烦，很多人都要配置好几个小时，特此记录，请参考[这里](http://blog.sina.com.cn/s/blog_4d4afb6d010008xq.html)


#11. 删除python 软件包

	python setup.py install --record files.txt 记录安装后文件的路径
	cat files.txt | xargs rm -rf  删除这些文件


