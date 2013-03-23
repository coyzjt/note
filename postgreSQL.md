
<h1 align="center">闪存数据库实验指南</h1>

* 作者：赖明星  
* 联系方式：mingxinglai@gmail.com  
* 版本：v0.1  

本文介绍了两种完成缓存替换算法的实验方法，第一种方法是基于一个名为Flash-DBSim模拟器平台，该模拟器的实验平台是Windows 和 visual studio 2008 ，使用C++完成缓存替换算法的实验；第二种方法是修改PostgreSQL的源码，然后使用一个叫做BenchmarkSQL 的开源工具测试数据库的性能，这种方法的实验平台是linux ，使用的开发语言是c 语言，这种方法的实验难度要远大于第一种方法，尤其是对不熟悉linux 的读者，这种方法的好处就是能够学到更多的知识，就LRU算法而言，Flash-DBSim的实现远没有PostgreSQL优美。


# 基于固态盘的缓存替换算法实验指南（Flash-DBSim 篇）

FlashDB-Sim篇将介绍一个名为Flash-DBSim的模拟器，这也是我的论文中采用的实验方法，所以对这种方法比较熟悉，如果有什么疑问，可以通过上面的邮件地址与我联系。

##1 Flash-DBSim 介绍

Flash-DBSim是由中国科学与技术大学的知识与数据工程实验室开发的一个模拟器，该模拟器可以根据上层应用的需要模拟出不同特性的固态盘，如闪存的读写代价不一致，写前擦除等特性。很多已有的研究（如缓存替换算法CCF-LRU和AD-LRU）都采用了该模拟器进行算法性能的比较。

###1.1 如何获取Flash-DBSim

可到Flash-DBSim的[官方网站][1]<http://kdelab.ustc.edu.cn/flash-dbsim/>下载，网站上有相关的介绍和说明，建议快速阅读一下官方网站中的说明。网站上有两个版本的Flash-DBSim可供下载，为了方便配置，请下载（Flash-DBSim源代码+示例 版本：1.6.0.32），也可以下载（Flash-DBSim运行组件 版本：1.6.0.32），然后根据网站上的说明进行配置。下面的说明将假设你下载的Flash-DBSim版本为"源代码+示例 版本：1.6.0.32"。

###1.2 实验平台

Flash-DBSim 使用的程序设计语言为c++，使用的开发工具为visual studio 2008，所以，你解压下载来的Flash-DBSim后会获得两个文件夹和一个文件，分别是FlashDBSimDll.sh文件、FlashDBSimDll文件夹和FlashDBSimDll_Sample 文件夹，即一个解决方案，解决方案下面有两个工程文件。直接用 visual studio 2008/2010 打开 FlashDBSimDll.sln 即可。

* 实验平台：Windows XP / Windows 7 + visual studio 2008(及以上版本)
* 程序设计语言：c++

##2 模拟器的使用：

在FlashDBSimDll.sln 这个解决方案中，共有两个工程文件，分别是FlashDBSimDll 和 FlashDBSimDll_Sample ，不需要理会工程FlashDBSimDll ，我们的实验只会用到工程FlashDBSimDll_Sample，在工程（也称文件夹）下，共有4个头文件(stdafx.h，BufferAlgorithm.h，flashdbsim_i.h和LRU.h)，3个实现文件(BufferAlgorithm.cpp，LRU.cpp，main.cpp)，和两个测试数据集(trace1000000，trace200000)，打开文件夹FlashDBSimDll_Sample下的main.cpp，可以直接运行。

###2.1 各文件的作用

下面介绍了各个文件的作用：

1. 不管什么实验，都不应该去修改flashdbsim_i.h和stdafx.h文件;
2. BufferAlogorithm.h 和BufferAlgorithm.cpp 中定义了所有缓存替换算法的基类，所有的缓冲区替换算法都应该从该类继承;
3. LRU.h和LRU.cpp 文件给出了缓冲区替换算法LRU的实现，演示了FlashDBSim的使用;
4. 程序在main.cpp中调用缓冲区替换算法进行算法的测试;
5. trace1000000和trace200000 是两个测试数据集，里面的数据分为两列，若干行，每行代表一次读操作或一次写操作。其中，第一列是数据的逻辑页号，第二列中用0和1表示读操作和写操作。

###2.2 模拟器的参数配置

FlashDBSim是一种高效的、可重用和可配置的闪存存储系统仿真平台，可以根据上层应用的需要模拟出不同的特性的固态盘，而且可以方便的为上层应用提供测试环境。典型的配置如下：

	AttributeValuePage Size = 2,048
	BBlock Size = 64 
	pagesRead Latency = 25us/page
	pageWrite Latency = 200us/page
	pageErase Latency = 1.5ms/block
	Endurance = 100,000

这些参数可能根据具体的实验不同而不同，可以在main.cpp文件中修改，实验中可能还会修改缓冲区的大小，可以在BufferAlgorithm.h文件中修改（修改变量 DEFBUFSIZE的值，默认为1536，即缓冲区大小为3M）。

##3 LRU算法

如果实验是实现或者测试一个缓冲区替换算法，则首先应该阅读LRU.h 和LRU.cpp、main.cpp的源代码，通过模拟器作者提供的例子，了解FlashDBSim的使用，以及如何在main.cpp中调用自己实现的缓冲区替换算法。下面对缓冲区替换算法LRU进行简单的介绍，以帮助读者快速了解LRU算法的具体实现，读者还需自行阅读LRU算法的源代码，才能彻底理解LRU算法的具体实现，并通过学习LRU算法的实现来学习FlashDBSim的使用。
此外，读者的缓冲区替换算法应该在LRU算法的基础之上修改，以避免引入未知错误。读者应该在理解了LRU算法的基础上，实现自己的算法，知道哪些成员函数需要修改，哪些成员函数不需要修改。在实现自己的算法中，可以拷贝LRU算法的代码，修改关键的几个成员函数（注1：大部分成员函数不需要修改；注2：因为大部分成员函数不需要修改，所以，如果读者需要实现几个缓冲区替换算法，应该将那些不需要修改的算法移到基类BMgr中去）。

###3.1 LRU算法的调用

在FlashDBSim自带的示例中，将在main.cpp中调用各缓冲区替换算法，main.cpp 中的执行过程如下（任何缓冲区替换算法都将严格按照下列步骤执行）：

1. 初始化配置信息
2. 写入数据页
3. 读入测试数据，开始测试
4. 测试完毕，将内存中的脏页写回闪存
5. 获取测试信息

###3.2 LRU算法的数据结构

FlashDBSim自带的示例中，LRU.h文件里定义了两个结构体，分别代表数据块和链表元素，它们的组织如图1所示，即LRU算法包含了一个链表，一个hash表(ptop)，和一个数组(ftop)，数组用以关联hash表和链表。

为了快速理解LRU算法，读者应该重点阅读FixPage函数和SelectVictim函数的源代码，3.3节给出了所有成员函数以及它们的作用，其中很多成员函数是不用修改，也不用深入理解的，还需要注意的是，所有与外部存储相关的操作都是假动作，只是模拟闪存的读和写操作，并没有实际读、写某个文件，只是统计读写次数。

<a href="http://imgur.com/Jxryv"><img src="http://i.imgur.com/Jxryv.png" title="Hosted by imgur.com" alt="" /></a>

###3.3 LRU算法的成员函数

下面是LRU算法的成员函数，分别介绍各个成员函数的作用：

**Public成员函数：**

	Public:LRU()                                     // 构造函数，初始化数据
	~LRU()                                           // 析构函数，释放资源
	void Init()                                      // 初始化，在1.6节的第2步与第3步之间调用
	int FixPage(int page_id)                         // 安插一个数据页
	NewPage FixNewPage(LBA lba)                      // 安插一个新的数据页
	int UnFixPage(int page_id)                       // 你永远不会用到
	void ReadFrame(int frame_id, char* buffer)       // ; 读一帧数据，用不到
	void WriteFrame(int frame_id, const char*buffer) // 写一帧数据，用不到
	int WriteDirty(void)                             // 将内存中的脏页写会闪存
	double HitRatio(void)                            // 返回命中率
	void RWInfo()                                    // 返回读写统计信息
	int IsLBAValid(LBA lba)                          // 判断lba是否有效
	int LBAToPID( LBA lba)                           // 通过LBA获取PID
	
**Private成员函数：**

	int hash(int page_id)                  // Hash函数
	LRUBCB* PageToLRUBCB(int page_id)      // 通过page_id 获取块数据
	void RemoveLRUBCB(LRUBCB* pb)          // 移除块数据
	void InsertLRUEle(int frame_id)        // 在链表中插入元素
	void RemoveLRUEle(int frame_id)        // 移除链表元素
	void AdjustLRUList(int frame_id)       // 调整链表元素（命中以后调整命中页的位置）
	void SetDirty(int frame_id)            // 设置frame_id 的脏标识
	int SelectVictim()                     // 选择替换页，缓冲区替换算法的核心函数
	void RegistEntry(LBA lba, PID page_id) // 注册一页，用不到

**Private成员变量：**

	int ftop[DEFBUFSIZE]    // 数组
	LRUBC* ptop[DEFBUFSIZE] // Hash表
	bFrame buf[DEFBUFSIZE]  // 存放一帧数据，用不到
	Map<LBA,PID>            // maplist逻辑页号与物理页号的映射关系
	LRUElement* lru         // 链表的表尾指针
	LRUElement* mru         // 链表的表头指针
	int hit                 // 命中次数
	int total               // 读写操作的次数
	int flashreadcount      // 物理读操作次数
	int flashwritecount     // 物理写操作次数

###3.4 其他注意事项

前面已经介绍过，Flash-DBSim 会在main.c的main
函数里面调用缓存替换算法，然后得出测试数据，但是，在Flash-DBSim
自带的对LRU算法测试的代码中，有两种统计命中率、物理读操作次数 、物理写操作次数和总延迟等性能参数。这两种统计的性能参数的方法（icounter 和 ilatency），统计结果不一样，虽然结果相差不大，但是造成很大的困惑，我也没弄懂有什么区别，不过两种统计方法中各项性能指标还是成比例的，所以，就自己选一种吧。我自己选择的是后一种方法(ilatency)，因为后一种方法是通过延迟来计算物理读操作次数，写操作等信息，感觉比较正确。

##4 生成测试数据集

在本实验指南的最后，介绍测试数据集的生成方法。直接使用附件中的generateTestData.cpp 文件生成测试数据，通常情况下，只需要修改READ, LOCALITY_X和LOCALITY_Y这3个参数即可。

	Const int READ= 3;         // 参数的含义是所有的操作中，读写比例为3:7
	Const int LOCALITY_X = 8;  // 参数的含义是LOCALITY_X % 的读写操作发生在 LOCALITY_Y% 的数据页上
	Const int LOCALITY_Y = 2;  // LOCALITY_X = 2, LOCALITY_Y = 8 表明80%的读写操作集中在20% 的数据页上。
	Const int nPage = 10000;   // 总共有多少个不同的数据页（我估计你不需要修改）
	Const int nSize = 3000000; // 测试数据集中，共有多少次读写操作


---
---


# 基于固态盘的缓存替换算法实验指南（PostgreSQL 篇）

##0 可能面临的问题

下面几个问题是我完成本实验以前一直思考的问题，首先回答这几个问题，能够帮助读者对完成实验有一个整体的认识，心里有个底。

* 做缓存替换算法的实验，是否只需要修改PostgreSQL
  缓存替换算法相关的源码，然后编译安装即可

	是的，在`postgresql-7.3.40/src/backend/storage/buffer`
	目录下修改缓存替换算法，修改完成以后还和以前一样安装。

* 我要实现多个缓存替换算法，是否需要多次安装、卸载PostgreSQL

	是的，一个系统中只有个PostgreSQL
	数据库，要比较多个缓存替换算法，需要实现一个算法，安装PostgreSQL
	，获取测试数据，然后卸载PostgreSQL ，再去实现下一个算法。

* 如何将数据存储到固态盘

	首先用mount 命令挂载固态盘，然后，在postgresql-7.4.30
	目录下`./configure --prefix="your path"`的时候指定路径。

* 输入数据是什么，从何而来

	不需要我们自行获取输入数据，我们会用一个名为BenchmarkSQL
	的测试工具，该测试工具会自动帮我们创建表、创建索引，添加数据。

* 如何获取运行时间

	数据库性能测试工具 —— BenchmarkSQL ，会显示运行时间。

* 如何设置缓冲区大小

	在PostgreSQL 的源码中，有一个名为NBuffer 的变量，默认值是1000
	，意思是有几个缓冲区(数据页)，修改缓冲区大小，只需要修改这个变量的值即可。

* 如何统计写操作次数和命中率

	BenchmarkSQL虽然帮助我们输入了数据，完成了数据库性能的测试，获取了数据库的执行时间，但是，没有给出命中率，物理写操作次数等信息。不过PostgreSQL源码中有统计这些信息，我们需要修改PostgreSQL ，把这些信息打印出来。

##1 使用postgresql 完成缓存替换算法的步骤

1. 修改 PostgreSQL 缓存相关的源码
2. 用源码安装的方式安装PostgreSQL 到固态盘
3. 安装BenchmarkSQL
4. 使用BenchmarkSQL 进行测试，获取测试结果


以上每一步都可以分为很多小步，其中，第一步最复杂，需要读者首先阅读PostgreSQL 源码，在此基础上，实现自己的缓存替换算法。我们将在最后介绍PostgreSQL
的缓存替换算法，以及如何实现自己的缓存替换算法。下面首先介绍如何在linux系统下PostgreSQL安装、BenchmarkSQL安装，使用BenchmarkSQL对PostgreSQL 的性能进行测试。读者最好根据下面的步骤，先安装一次PostgreSQL 和 BenchmarkSQL ，熟悉一下测试PostgreSQL 性能的完整过程。

###1.1 下载PostgreSQL

请到<http://www.postgresql.org/ftp/source/>下载PostgreSQL v7.4.30 ，下载PostgreSQL v7.4.30 是因为这是最后一个使用LRU算法的版本，更新版本的PostgreSQL
使用了更复杂的缓存替换算法，阅读起来更有难度，而且，在实现自己的缓存替换算法，几乎都要和LRU算法进行比较，下载这个版本，就不用再实现一遍LRU算法。


###1.2 安装PostgreSQL 1

将下载的PostgreSQL 解压后会得到一个名为 postgresql-7.4.30 的文件夹，在linux下安装PostgreSQL还是很方便的，在`postgresql-7.4.30`目录下，有安装说明文件INSTALL，说明文件中关键的几行代码如下：
	
	./configure  #配置
	gmake #make
	su #切换到root用户
	gmake install  #安装，以上几步是linux 下安装软件的典型方式
	adduser postgres #添加一个用户，添加完用户以后需要通过passwd postgres来修改该用户的密码
	mkdir /usr/local/pgsql/data #新建一个目录，以后数据库的所有数据和操作都在该目录下
	chown postgres /usr/local/pgsql/data #更改目录的所有者为 postgres
	su - postgres  #切换用户为postgres
	/usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data #初始化工作区
	/usr/local/pgsql/bin/postmaster -D /usr/local/pgsql/data >logfile 2>&1 &  
	/usr/local/pgsql/bin/createdb test #启动服务，并新建一个名为test 的数据库
	/usr/local/pgsql/bin/psql test  #启动数据库
	

以上是PostgreSQL的安装过程，已经添加了相应的注释，可以看到，PostgreSQL的安装还是比较方便的，但是，对于第一次使用PostgreSQL的用户，尤其是之前不怎么接触linux的用户，要使用这么多的命令，还是很繁琐的，而且，由于我们要将PostgreSQL安装到固态盘，还要使得新建的数据库能够被测试工具BenchmarkSQL使用，要做的工作还有很多，如果没有人指导，靠自己摸索，会耗费很多时间，这也是我写这份文档的价值所在。

###1.3 安装PostgreSQL 2

下面介绍如何将PostgreSQL安装到固态盘，以及如何配置PostgreSQL，使得BenchmarkSQL
能够顺利的完成对PostgreSQL的性能测试。

	./configure --prefix=想要安装的路径 --without-readline --without-zlib
	make
	sudo make install

我们通过第一条语句指定数据库的安装路径，如果不指定，则默认安装到 `/usr/loca/pgsql`
目录下，要是读者现在身边没有固态盘，则可以不指定 --prefix 参数，让数据库安装到默认位置。为了描述方便，下面假设读者安装到默认位置，如果安装到其他位置，在下面的语句中，请自行修改路径。

执行完上面3条语句以后，数据库就安装完成了，但是还有很多工作要做，首先，修改`/usr/local/pgsql/share`目录下的conversion_create
文件，将所有的 "$libdir" 替换成 "安装目录/lib"，如果你不会vim ，也不会shell
脚本，这真是件很有挑战性的事情，不过不用担心，这里的所有操作我都写了脚本，你只需要运行脚本就可以了，我这里之所以还把安装过程讲得这么详细，是方便读者在遇到问题的时候能够自行调试，自行解决。

替换完成以后，初始化工作区：

	mkdir /usr/local/pgsql/data
	chown userName /usr/local/pgsql/data
	/usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data #初始化工作区

初始化工作区以后，还需要修改配置文件：

>进入存储空间所在文件夹(/usr/local/pgsql/data)，打开postgresql.conf文件，将#tcpip_socket = false的#删除，false改为true。将#port = 5432的#删除

上面步骤完成以后，配置工作就完成了，下面只需要启动数据库服务，然后新建数据库就可以了。

	#启动服务
	/usr/local/pgsql/bin/pg_ctl -D /usr/local/pgsql/data -l logfile start
	
	#创建一个名为test 的数据库
	/usr/local/pgsql/bin/createdb test

这样，我们的数据库就创建完成了，下面就让我们使用BenchmarkSQL 来测试一下吧。

##2 BenchmarkSQL

###2.1 BenchmarkSQL 的安装

**注意：**
BenchmarkSQL 依赖于java，所以，需要在linux下配置jdk
。这个就不介绍了，不然，这份指南就太长了。

BenchmarkSQL是一款采用TPC-C的开源的数据库测试工具，几乎不用修改就能测试主流的数据库，在linux下使用BenchmarkSQL非常方便，只需要下载下来，运行里面的shell
脚本即可，都不需要安装。

下载地址：<http://sourceforge.net/projects/benchmarksql/>


###2.2 BenchmarkSQL 的使用

要运行BenchmarkSQL，首先需要修改BenchmarkSQL-2.3.2/run/postgres.properties文件，设置正确的数据库名和密码。配置文件如下，如果你之前创建了一个名为test的数据库，那么，你只需要给出你的账户密码就可以了，配置不可思议的简单。

	driver=org.postgresql.Driver
	conn=jdbc:postgresql://localhost:5432/test
	user=lalor
	password=123456

然后在BenchmarkSQL-2.3.2/run/下运行如下命令：

		./runSQL.sh postgres.properties sqlTableCreates

此命令用于创建我们进行TPCC测试所需的数据库表

		./loadData.sh postgres.properties numWarehouses 10

此命令用于加载我们进行TPCC测试所需的数据 numWarehouses后的数字可自设。

		./runSQL.sh postgres.properties sqlIndexCreates

此命令用于创建我们进行TPCC测试所需的索引

		./runBenchmark.sh postgres.properties

开始TPCC测试，这时会跳出一个对话框，用户可以根据自己的测试需要设定相关的warehouse数目和terminal数目，然后进行测试。

上面就是开源工具BenchmarkSQL 的使用，如果觉得还不够详细，可以参考<http://blog.sina.com.cn/s/blog_448574810101a276.html>这篇文章，里面有截图，可能看起来更直观一点。

###2.3 获取测试数据

使用BenchmarkSQL可以帮我们生成测试数据，获取数据库的性能，但是，如果要获取底层的详细信息，如缓冲区命中率，物理读操作次数和物理也操作次数还需要查看PostgreSQL 的源码，通过修改源码来获取这些信息。
具体方法如下：

	vim PostgreSQL-7.4.30/src/backend/libpq/pqcomm.c

注册一个客户端退出时的回调函数，用该函数来调用 ShowBufferUsage()
函数，打印统计信息。

注意：ShowBufferUsage()函数是Postgresql 自带的函数，用以打印与缓冲区相关的信息，在`/src/backend/tcop/postgres.c` 里被调用过一次，一开始我在该函数被调用的地方打印命中率等信息，但是，好像没有被调用，Postgresql 太复杂了。

注册回调函数的方法如下：
1. 先声明回调函数printBufferUsageInfo(void);

		/* Internal functions */
		static void pq_close(void);
		static void printBufferUsageInfo(void);
		static int	internal_putbytes(const char *s, size_t len);
		static int	internal_flush(void);

2.定义回调函数

		void printBufferUsageInfo(void)
		{
			FILE *fp = NULL;
			char *usage;
			usage = ShowBufferUsage();
		
			fp = fopen("/home/lalor/code/data.txt", "w+");
			if (fp == NULL) 
			{
				fprintf(stderr, "open file error");
				fclose(fp);
			}
			else
			{
				fprintf(fp, "%s\n", usage);
				fclose(fp);
			}
		}

3. 在合适的地方(pg_init)注册回调函数

		void
		pq_init(void)
		{
			PqSendPointer = PqRecvPointer = PqRecvLength = 0;
			PqCommBusy = false;
			DoingCopyOut = false;
			on_proc_exit(printBufferUsageInfo, 0);
			on_proc_exit(pq_close, 0);
		}


##3 删除 PostgreSQL 数据库

这样我们就测试完了，前面说过，一个系统中只能有一个PostgreSQL数据库，而我们完成
缓存替换算法的研究，必然要实现几个算法，进行算法之间的比较，所以，我们只能删除PostgreSQL
数据库，再修改PostgreSQL数据库中缓存相关的替换算法，再安装，再测试。删除PostgreSQL 的步骤如下：

1. 关闭数据库服务

		/usr/local/pgsql/bin/pg_ctl -D /usr/local/pgsql/data -l logfile stop

2. 删除数据库

		/usr/local/pgsql/bin/dropdb test

3. 卸载PostgreSQL

		./configure --without-readline --without-zlib
		sudo make uninstall
		sudo make clean

4. 删除目录

		sudo rm -rf /usr/local/pgsql


##4 自动化测试

如果我们实现多个缓存替换算法，那么上面的步骤就要执行多次，作为一个计算机专业的人，我们是不允许这么简单的重复工作的，所以我写了几个shell 脚本，读者只要按顺序执行下面几个脚本就可以了，非常方便。

把createPostgreSQL 和 removePostgreSQL 复制到 postgresql-7.4.30
目录下，使用下面的命令给它们添加可执行权限
	
	chmod +x createPostgreSQL removePostgreSQL 

如果使用默认的安装路径，则什么都不用修改，直接运行就可以了，如果指定了数据库的安装路径，则需要修改这两个文件中的 path 变量，修改完成以后，像下面这样执行即可：

	./createPostgreSQL #removePostgreSQL runCmdInPack 也像这样执行

执行完成以后，将runCmdInPack 文件复制到 BenchmarkSQL-2.3.3/run
目录下，添加可执行权限，运行即可。

运行完成，得到测试数据以后，回到postgresql-7.4.30 目录下，执行removePostgreSQL
文件，删除数据库系统。


##5 修改PostgreSQL 的缓存替换算法

在我们使用的PostgreSQL 7.4.30
版本中，使用的缓存替换算法是LRU算法，与缓存算法相关的文件位于`postgresql-7.4.30/src/backend/storage/buffer`目录下。打开该目录下的freelist.c 文件，可以看到，有一个变量SharedFreeList ，有几个关键的函数：AddBufferToFreeList ，GetFreeBuffer ，InitFreeList ，PinBUffer ，UnpinBuffer 。下面首先介绍PostgreSQL 中LRU算法的思想，然后介绍上面几个函数，读者再根据需要修改上面几个函数即可。

###5.1 PostgreSQL 中的LRU算法

在PostgreSQL里，一个数据页被命中后，就将它LRU链表中移除，到访问结束后，再把该数据页插入到LRU链表。因为PostgreSQL是多进程的数据库，缓冲区中的数据页可能同时被几个进程使用，所以，PostgreSQL用了一个引用计数来记录有几个进程“正在”使用这个数据页。很显然，正在被使用的数据页是不能被替换的，所以引用计数不为0的数据不会放在LRU链表中。正因为这样，PostgreSQL中可能会出现LRU链表为空的情况，这就是缓冲区中的所有数据页都正在被使用了。**注意**空闲缓冲区一般指的是还没有装入数据页的缓冲区，英文写作free buffer。但是，在PostgreSQL 和论文《PostgreSQL 闪存缓冲区置换算法扩展与性能验证》中，空闲缓冲区是指"装有数据页但没被进程使用的数据页"，在《PostgreSQL 闪存缓冲区置换算法扩展与性能验证》中，将数据页说成是缓冲区，将缓冲区说成是缓冲池，如果去读那篇论文的话，需要特别注意。

###5.2 PostgreSQL 缓存相关函数介绍

* AddBufferToFreeList 

	向LRU链表添加一个元素

* GetFreeBuffer 

	选择一个驱逐页，这是替换算法的核心，也是实现自己的缓存替换算法时主要修改的地方

* InitFreeList 

	初始化LRU链表（LRU链表不是真正的链表，而是一个数组）

* PinBUffer 

	前面说过，PostgreSQL 是一个多进程的数据库系统，当有一个进程访问LRU链表中的一个元素，即访问一个数据页时，就将该元素从LRU链表移除，并增加该元素的引用计数，并在访问结束以后，调用UnpinBuffer 减少引用计数，在UnpinBuffer 函数中减少引用计数以后还要判断引用计数是否为0，如果引用计数为0 ，表示没有任何进程引用该页，则将该页插入到LRU链表中。

* UnpinBuffer 

###5.3 修改PostgreSQL 缓存替换算法的步骤

修改PostgreSQL 的缓存替换算法分两步：

1. 修改描述子，增加新属性

	若新算法为缓冲区赋予了新的属性，则需要修改PostgreSQL
	缓冲区描述子的数据结构，在其中定义新的参数，以描述缓冲区的新属性

2. 增加新的数据结构和操作

	LRU算法要求缓冲池是一条LRU链表，若新算法要求缓冲池是另一种数据结构，就需要对缓冲区管理部分进行相应的修改。首先，需要修改缓冲区描述子的数据结构和增加新的数据结构，满足缓冲池的构建要求；然后，需要修改缓冲池的初始化操作，在初始化时构建新结构的缓冲池；最后，通过修改原有的缓冲池维护操作，或者替换和增加新的维护操作，保证缓冲池的数据结构始终满足新算法的要求。


###5.4 描述子

所谓描述子就是一个结构体（BufferDesc），用于表示LRU链表的一个元素，位于头文件`buf_internals.h`中。如果我们实现的算法中，需要增加新的属性，就在该结构体中添加。


###5.5 PostgreSQL 中的LRU算法的实现

<a href="http://imgur.com/Y6Aon"><img src="http://i.imgur.com/Y6Aon.png" title="Hosted by imgur.com" alt="" /></a>

上图就是LRU算法中的LRU"链表"，注意，并不是真正的链表，而是用数组实现，数组名为BufferDescriptors ，数组中有NBuffer + 1 个描述子(BufferDesc)，前NBuffer 用来表示空闲缓冲区（数据页），最后一个是卫兵，方便操作，SharedFreeList 是一个一直指向卫兵的指针，当满足 SharedFreeList->freeNext == Free_List_Descriptor 的时候，就说明缓冲区中的所有数据页都在被使用，有了SharedFreeList ，我们就可以很快的选择LRU 位置的数据页为驱逐页，将新的数据页插入到MRU位置。

在PostgreSQL 中，有了上面的数据结构，实现LRU算法只用到了两个函数，分别是 AddBufferToFreeList 向LRU链表添加一个元素，新元素位于MRU位置；另一个是 GetFreeBuffer ，用于返回链表中LRU 位置的元素。

###5.6 CF-LRU 实现算法

CF-LRU 算法不需要增加新属性，只需要判断干净页优先区(Clean-First Region)中是否有干净页，如果有干净页，就替换干净页，如果没有干净页，就替换LRU位置的数据页。在CF-LRU 算法中，我们只需要增加一个变量 nWindowSize 表示干净页优先区的大小，然后修改GetFreeBuffer 函数即可。

如下所示：

LRU 算法：

	/*
	 * GetFreeBuffer() -- get the 'next' buffer from the freelist.
	 */
	BufferDesc *
	GetFreeBuffer(void)
	{
		BufferDesc *buf;
	
		if (Free_List_Descriptor == SharedFreeList->freeNext)
		{
			/* queue is empty. All buffers in the buffer pool are pinned. */
			ereport(ERROR,
					(errcode(ERRCODE_INSUFFICIENT_RESOURCES),
					 errmsg("out of free buffers")));
			return NULL;
		}
		buf = &(BufferDescriptors[SharedFreeList->freeNext]);
	
		/* remove from freelist queue */
		BufferDescriptors[buf->freeNext].freePrev = buf->freePrev;
		BufferDescriptors[buf->freePrev].freeNext = buf->freeNext;
		buf->freeNext = buf->freePrev = INVALID_DESCRIPTOR;
	
		buf->flags &= ~(BM_FREE);
	
		return buf;
	}

CF-LRU 算法：

	/*
	 * GetFreeBuffer() -- get the 'next' buffer from the freelist.
	 */
	BufferDesc *
	GetFreeBuffer(void)
	{
		BufferDesc *buf;
	
		if (Free_List_Descriptor == SharedFreeList->freeNext)
		{
			/* queue is empty. All buffers in the buffer pool are pinned. */
			ereport(ERROR,
					(errcode(ERRCODE_INSUFFICIENT_RESOURCES),
					 errmsg("out of free buffers")));
			return NULL;
		}
		int n = 0;
		buf = &(BufferDescriptors[SharedFreeList->freeNext]);
	
		/* select a replace page in clean-first region */
		while (n < nWindowSize) 
		{
			/* Is buffer dirty? */
			if (buf->flags & BM_DIRTY || buf->cntxDirty)
			{
				buf = &(BufferDescriptors[buf->freeNext]);
				/* no more buffer */
				if ( buf == SharedFreeList)
				{
					buf = NULL;	
					break;
				}
			}
			else
			{/* select this page as replace page */
				break;
			}
			n++;
		}
	
		/*
		 *1. buf == NULL 说明缓冲区中总共也没有 nWindowSize 个 free buffer
		 *2.  n >= nWindowSize 说明缓冲区中干净页优先区中，全都是脏页
		 * 上面两种情况，都选择位于LRU 位置的数据页为驱逐页
		 */
		if (n >= nWindowSize || buf == NULL) 
		{
			buf = &(BufferDescriptors[SharedFreeList->freeNext]);
		}
	
		/* remove from freelist queue */
		BufferDescriptors[buf->freeNext].freePrev = buf->freePrev;
		BufferDescriptors[buf->freePrev].freeNext = buf->freeNext;
		buf->freeNext = buf->freePrev = INVALID_DESCRIPTOR;
	
		buf->flags &= ~(BM_FREE);
	
		return buf;
	}
	

**注意：**在实现了自己的算法以后，在该目录下执行make命令，开始编译文件，这样可以发现语法错误，如果有逻辑错误，需要调试——这个我暂时也不会，所以在实现自己算法的时候一定要小心，要仔细检查，或许读者在看这份文档的时候我已经会调试了，可以发邮件问我。

###5.7 CCF-LRU 算法的实现

###5.8 其他注意事项

* 我在实验的过程中，碰到一个很奇怪的问题，创建数据库失败，错误信息是"createdb: could not connect to database template1: FATAL: the database system is shutting down"，把数据库删除了重新安装也不行，google了很久也没有找到解决方案，最后，计算机重启就正常了。


[1]: http://kdelab.ustc.edu.cn/flash-dbsim/
