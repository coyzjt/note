<h1 align="center">fedora FAQ</h1>

#1.在fedora中使用微软雅黑和Ubuntu Mono字体

fedora的默认字体是出了名的丑，而且看着很伤眼，把字体改成微软雅黑＋Ubuntu Mono的组合堪称完美，反正这个组合我是怎么看怎么爽。

为嘛这写篇文章呢，都怪网上讲的不够透彻，如果你搜`fedora 微软雅黑`你会得到很多结果，但是却很难让新手找到直接可用的方法，你要是跟我一样研究过不同的字体，你一定听说过"[最适合编程的十大字体][1]"，但是，那是人类投票的结果，不一定是你最喜欢的字体。我就特别喜欢Ubuntu 12.04自带的字体，所以的又google了一下"fedora ubuntu 字体"和"ubuntu 12.04 自带的是什么字体"，当然是没有满意的结果，不然我也没有写这篇文章的必要了。

下面就来介绍一下如何在fedora下（同样适合Ubuntu)修改系统自带的字体，还有更重要的是，如何让中文和英文使用不同的字体。

### 安装新字体
fedora(Ubuntu)下安装新字体倒也简单，首先下载你喜欢的字体，后缀名应该是ttf,然后将文件或文件夹拷贝到linux中`/usr/share/fonts`目录下，你也可以在该目录下新建子目录，如：`sudo mkdir -p chinese/yahei`然后在`/usr/share/fonts/chinese`目录下执行下面3条语句(我也纳闷为什么要执行3条语句，它们都干了什么，不过linux下命令这个多，哪学得完啊，反正知道安装新字体就是顺序执行下面3条语句就是了)

    sudo mkfontscale
    sudo mkfontdir
    sudo fc-cache -fv


安装完以后注销，再次登陆字体就可用了，但是要设置字体，我们还需要安装一个工具`gnome-tweak-tool`，傻瓜式安装：

    sudo yum install gnome-tweak-tool

然后应用程序里多了一项"高级设置"，打开它就可以设置字体了，也可以在终端输入`gnome-tweak-tool`打开。设置字体是傻瓜式的，这里不再废话，重要的是如何设置字体，使得中文使用微软雅黑字体，英文使用Ubuntu Mono(或者你喜欢的其他英文字体,Monaco也是一个很不错的编程字体)

你可以在[最适合编程的十大字体][1]里面选择一个你特别喜欢的，或者像我一样使用[Ubuntu Mono][2]字体,然后按照上面的方法安装新字体，打开gnome-tweak-tool这个工具，进行如下设置：


这样设置以后就可以实现中文使用微软雅黑，英文使用更适合编程的字体了，这里要友情提醒的是，终端－－配置文件首选项－－常规选项卡里也是可以修改字体的，不过新安装的字体不在里面，只能修改成系统自带的字体，所以这里最好使用默认设置。


#2.自定义快捷键无法使用ctrl

自定义快捷键的方法网上有[很多][3],从Ubuntu 转移过来的用户应该知道ctrl+alt+t,这个快捷键，我准备为终端定义快捷键的时候，发现ctrl无法使用，将就使用alt+t,使用半天以后，我放弃了，实在是无法改变我的使用习惯，虽然alt+t 明明比ctlr+alt+t还要少按一个键,但我还是更喜欢ctrl+alt+t。网上搜了很久也没有找到解决方法，搜到这个[方法][4](使用dconf-editor)在我的电脑上也不能使用，最后被我找到解决的办法，就是升级fedora内核，即执行下面这条命令：

    sudo yum update

#3.sudo: command not found

这个问题很可能是因为sudo依赖的包被卸载了，解决办法就是re-compile sudo package.即用超级用户执行下面语句：

    yum install sudo

#4.xx 不在sudoers文件中，此事将被报告

居然操作系统已经提示你了，说你的不在sudoers文件中，那就把你的名字加进去吧，首先切换成超级用户，然后往/etc/sudoers
文件追加一行语句：
    
    su
    echo 'username ALL=(ALL) ALL' >> /etc/sudoers

#5. fedora下安装jekyll

首先安装ruby,然后安装gem,最后安装jekyll,果然比Ubuntu麻烦。

    sudo yum install ruby
    sudo yum install rubygems
    sudo gem install jekyll

#6. fedora 下安装chrome

下载google-chrome-stable_current_i386.rpm后，打开终端，输入 cd Downloads切换到下载文件保存的目录，再输入下面命令安装chrome浏览器

    sudo rpm -ivh google-chrome-stable_current_i386.rpm

结果出现下面的错误提示

    error: Failed dependencies:
    lsb >= 4.0 is needed by google-chrome-stable-21.0.1180.79-151411.i386
    libXss.so.1 is needed by google-chrome-stable-21.0.1180.79-151411.i386
    wget is needed by google-chrome-stable-21.0.1180.79-151411.i386

这是因为缺少依赖的包，安装即可。

    sudo yum install wget
    sudo yum install redhat-lsb
    sudo yum install libXScrnSaver

安装完以后再执行下面的语句即可。

    sudo rpm -ivh google-chrome-stable_current_i386.rpm

#7. rake: command not found

这个问题也耗掉了我进半个小时，真是鄙视自己，如果你用搜索引擎搜"rake: command not found"，找到的答案基本都是关于环境变量的问题，以前使用Ubuntu根本不存在这个问题。如果系统提示没有这个命令，首先检查是否已经安装了rake,使用下面的语句检查：

    gem list --local

如果出现了rake,则说明已经安装，是环境变量的问题，你只需要
    
    sudo ln -s `which rake` /usr/local/bin/

如果没有rake,则说明没有安装，则需要安装，千万别像我刚开始一样执行`yum install rake`,这是非常愚蠢的，正确的安装方法应该是：

    sudo gem install rake

#开机屏幕亮度问题
1. 先检查/etc/shells文件，必须包含bash、sh的路径，如果没有，就执行

    echo /bin/bash >> /etc/shells
    echo /bin/sh >> /etc/shells

2. 新建/etc/rc.d/rc.local文件，写入自己需要的初始亮度值

    #!bin/sh
    echo 5 > /sys/class/backlight/acpi_video0/brightness

3. 增加权限

    chmod a+x /etc/rc.d/rc.local

4. 创建链接

    ln -s /etc/rc.d/rc.local /etc

5. 修改brightness文件的权限，该文件原本是所有用户都只读的，包括root（网上没提到这一步，本菜鸟担心开机运行的脚本没有权限修改它，所以自行加了这一步）

    chmod 777 /sys/class/backlight/acpi_video0/brightness


#fedora 下解压rar文件

1. [下载][5]rar for linux
2. 安装

    make #执行make命令即可

#安装mp3[解码器][6]

	su -c 'yum localinstall --nogpgcheck http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-stable.noarch.rpm http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-stable.noarch.rpm'

**GNOME:**

	sudo yum install gstreamer-plugins-bad gstreamer-plugins-bad-free-extras gstreamer-plugins-bad-nonfree gstreamer-plugins-ugly gstreamer-ffmpeg

**KDE:**

	sudo yum install xine-lib-extras xine-lib-extras-freeworld k3b-extras-freeworld

#fedora下配置jdk

首先，下载jdk文件，以jdk-6u38-linux-i586-rpm.bin 为例

	chmod +x jdk-6u38-linux-i586-rpm.bin 
	./jdk-6u38-linux-i586-rpm.bin 
	chmod +x jdk-6u38-linux-i586.rpm
	rpm -ivh jdk-6u38-linux-i586.rpm

[配置][7]环境变量，vim /etc/profile ，数据如下语句：

	JAVA_HOME=/usr/java/jdk1.6.0_38/
	CLASSPATH=.:$JAVA_HOME:/lib/tools.jar:/lib.dt.jar
	PATH=$JAVA_HOME/bin:$PATH
	export JAVA_HOME CLASSPATH PATH

#fedora 下安装g

	sudo yum install libxslt-devel

[1]: http://www.iplaysoft.com/top10-programming-fonts.html 
[2]: http://www.ffonts.net/Ubuntu-Mono.font.zip
[3]: http://blog.sina.com.cn/s/blog_67e34ceb01013drn.html
[4]: http://www.linuxidc.com/Linux/2012-01/51510.htm 
[5]: http://www.rarlab.com/download.htm
[6]: https://ask.fedoraproject.org/question/625/solved-what-plugins-do-i-need-to-install-to-watch
[7]: http://os.51cto.com/art/201001/177657.htm
