#安装第三方库

第三方库的[资源](http://pypi.python.org)

**安装方法**

找到setup.py 文件，然后执行python setup.py install

也可以将模块安装到用户的特定库目录下：

	#~/.local/lib/python2.7/site-pack-ages
	python setup.py install --user

或者制定安装路径：

	python setup.py install --prefix=/home/lalor/pypackages

以以上这种方式安装的第三方库，需要调整sys.path 的设置，才能使python
找到最新安装的模块。

<!--重点学习模块-->
# 最最重要的模块

1. collections
2. itertools
3. sys
4. os

# gc

gc模块提供了一个控制垃圾收集器的接口

#inspect

inspect 模块手机关于Python 活动对象的信息，如属性，文档字符串，源代码等信息。

#traceback

发生异常后，traceback 模块用于收集并打印程序的栈跟踪信息。

# contextlib

该模块提供一个装饰器和一些实用工具函数，用于创建与with语句结合使用的上下文管理器。（参见python参考手册第4版P217)

# functools

该模块包含能够创建高阶函数，函数式编程的函数和装饰器。（参见python参考手册第4版P218)

* partial
* **reduce**
* update_wrapper


<!--hello, world-->
# copy

创建一个引用

	b = [1, 2, [3, 4]]
	a = b
	a[0] = 2   #b[0] = 2

浅拷贝

	b = [1, 2, [3, 4]]
	a = b[:]
	a[0] = 2      # b[0] = 1
	a[2][0] = 100 # b[2][0] = 100

实现深拷贝

	import copy
	b = [1, 2, [3, 4]]
	a = copy.deepcopy(b)
	a[0] = 2      # b[0] = 1
	a[2][0] = 100 # b[2][0] = 3

# types
 
标准模块types 内包含所有内建类型对象，我们可以通过它来完成类型检查工作

	import types
	if type(s) is types.ListType:
		print 'Is a list'
	else:
		print 'Is not a list'

# sys

需要重点学习的模块。python中可以通过sys 模块方便的访问命令行参数，获取操作系统，模块路径等信息。

	#!/usr/bin/python
	import sys	
	print('The command line arguments are:')
	for i in sys.argv:
		print i
	
	print '\n\nThe PYTHONPATH is', sys.path, '\n'

# os

os模块提供了一个统一的操作系统接口函数，os模块能在不同操作系统平台如nt，posix中的特定函数间自动切换，从而实现跨平台操作。

* os.getcwd()    获取当前工作目录，即当前python脚本工作的目录路径
* os.chdir("dirname")    改变当前脚本工作目录到dirname
* os.curdir    返回当前目录: ('.')

	import os
	os.getcwd()   #输出'/Volumes/Leopard/Users/Caroline'
	os.chdir('/Volumes')
	os.getcwd()   #输出'/Volumes'
	os.curdir     #输出"."

* os.makedirs()    可生成多层递归目录
* os.removedirs()    可删除多层递归空目录，若目录不为空则无法删除
* os.mkdir()    生成单级目录
* os.rmdir()    删除单级空目录，若目录不为空则无法删除，报错
* os.pardir()    获取当前目录的父目录字符串名
* os.listdir()    列出指定目录下的所有文件和子目录，包括隐藏文件
* os.tmpfile()    创建并打开‘w+b’一个新的临时文件
* os.remove()    删除一个文件
* os.rename("oldname","newname")    重命名文件
* os.sep    输出操作系统特定的路径分隔符。win下为"\\",macx下为"/"
* os.linesep    输出当前平台使用的行终止符
* os.pathsep    输出用于分割文件路径的字符串
* os.name    输出字符串指示当前使用平台。win->'nt'; mac->'posix'
* os.system(command)    运行shell命令
* os.path.isfile()
* os.path.isdir()
* os.path.exists()

	import os
	os.sep     #输出"/"
	os.linesep  #输出"\n"
	os.pathsep  #输出":"

* os.environ    获取系统环境变量
* os.access('pathfile',os.W_OK)    检验文件权限模式，输出True，False
* os.chmod('pathfile',os.W_OK)    改变文件权限模式

	import os
	os.access('/Volumes/Leopard/Users/Caroline/Desktop/1.mp4',os.W_OK) #输出True
	os.chmod('/Volumes/Leopard/Users/Caroline/Desktop/1.mp4',os.X_OK) #修改权限为X
	os.access('/Volumes/Leopard/Users/Caroline/Desktop/1.mp4',os.W_OK) #输出False
	os.access('/Volumes/Leopard/Users/Caroline/Desktop/1.mp4',os.X_OK) #输出True

# time

该模块主要用于获取系统时间，或者调用sleep函数。在获取系统时间的时候，通过格式控制符控制，与shell中的date命令相似。示例如下：

	#!/usr/bin/python
	
	import os
	import time
	
	def main():
		"""docstring for main"""
		#1. The files and directories to be backed up are specified in a list
		source = [ '/home/lalor/note/private', '/home/lalor/note/public']
		#2. The backup must be stored in a main backup direcotry
		target_dir = '/home/lalor/temp'
		#3. The files are backed up into a zip file.
		#4. The name of the zip archive is the current date and time
		today = time.strftime('%Y%m%d')
	
		now = time.strftime('%H%M%S')
		if	not os.path.exists(today):
			os.mkdir(today)
			print('Successfully created directory')
	
		target = today + os.sep + now + ".tar.gz"
	
		tar_command = "tar -cjf {0} {1}".format(target, ' '.join(source))
		#Run the backup
		if	os.system(tar_command) == 0:
			print('Successful backup to')
		else:
			print('Backup FAILED')
	
	
	if __name__ == '__main__':
		main()
	

sleep 示例如下：

	import time
	try:
		f = file('poem.txt')
		while True:
		line = f.readline()
		if (len) == 0:
			break
		time.sleep(2)
		print line,
	finally:
		f.close()
		print 'Cleaning up...closed the file'

# Pickle

如果希望透明地存储Python对象，而不丢失其身份和类型等信息，则需要某种形式的对象序列化：它是一个将任意复杂的对象转成对象的文本或二进制表示的过程。

pickle模块及其同类模块cPickle向Python提供了pickle支持。后者是用C编码的，它具有更好的性能。

	#!/usr/bin/python3
	#python provides a standard modules called pickle using which 
	#you can store any python object in a file and then get it back later. 
	
	import pickle
	
	def main():
		#the name of the file where we will store the object
		shoplistfile = 'shoplist.data'
		#the list of things to buy
		shoplist = ['apple', 'mango', 'carrot']
		people = ['lalor', 'mxl', 'zyl']
		
		#write to the file
		f = open(shoplistfile , 'wb')
		pickle.dump(shoplist, f) #dump the object to a file
		pickle.dump(people, f) #dump the object to a file
		f.close()
		
		del shoplist #destory the shoplist varibale
		#read back from the storage
		f = open(shoplistfile , 'rb')
		storedlist = pickle.load(f) #load the object from the file
		storedpeople = pickle.load(f)
		print(storedlist)
		print(storedpeople)
	
	
	if __name__ == '__main__':
		main()


# shelve
shelve 模块类似与Pickle ，但它将对象保存在一个类似字典的数据库中：

	import shelve
	
	obj = SomeObject()
	db = shelve.open("filename") #打开一个shelve
	db['key']= obj#将对象保存在shelve
	...
	obj = db['key']
	db.close()#关闭shelve

#marshal 

类似于shelve与pickle模块，不过功能不强，我估计都没什么人用了。

# numpy

numpy 是一个科学计算库，可以简单的实现多维数组，以及矩阵运算。

	>>> from numpy import *
	>>> a = arange(10)
	>>> a
	array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
	>>> a.shape = 2, 5
	>>> a
	array([[0, 1, 2, 3, 4],
	       [5, 6, 7, 8, 9]])
	>>> a = arange(32)
	>>> a.shape = 2, 4, 4
	>>> a
	array([[[ 0,  1,  2,  3],
	        [ 4,  5,  6,  7],
	        [ 8,  9, 10, 11],
	        [12, 13, 14, 15]],
	
	       [[16, 17, 18, 19],
	        [20, 21, 22, 23],
	        [24, 25, 26, 27],
	        [28, 29, 30, 31]]])

矩阵运算：

	>>> a = mat('1 2 3; 4 5 6')
	>>> a
	matrix([[1, 2, 3],
	        [4, 5, 6]])
	>>> a.T
	matrix([[1, 4],
	        [2, 5],
	        [3, 6]])
	>>> a.I
	matrix([[-0.94444444,  0.44444444],
	        [-0.11111111,  0.11111111],
	        [ 0.72222222, -0.22222222]])
	>>> a * a.I
	matrix([[  1.00000000e+00,   2.22044605e-16],
	        [  0.00000000e+00,   1.00000000e+00]])
#matplotlib

画图。

<!--数据库-->
#sqlite3

系统自带，用于操作sqlite[数据库](http://mingxinglai.com/cn/2013/02/python-database/)

#MySQLdb

[MySQL for Python](http://sourceforge.net/projects/mysql-python/),第三方库开源库。


<!--文件和目录-->
#bz2

bz2 模块用与根据bzip2 压缩算法读取和写入压缩数据
	import bz2
	>>> f = bz2.BZ2File("./test.bz2", "w")  #返回类文件对象
	>>> data = "ashin is test bz2 of Python"
	>>> f.write(data)
	>>> f.close()
	>>> 
	 
	>>> c = bz2.BZ2Compressor()            
	>>> #创建用于顺序压缩数据块序列的压缩器对象，参数可设定1-9的压缩级别，默认为最高级别9
	>>> cd = c.compress(data)              
	>>> #向压缩器添加新数据，如果可能，返回一串压缩数据
	>>> print cd
	 
	>>> fd = c.flush()                      #返回compress后全部剩余数据的压缩字符串
	>>> print fd
	BZh91AY&SY"...
	 
	>>> d = bz2.BZ2Decompressor()           #创建解压缩器对象，无参数
	>>> dfd = d.decompress(fd)             
	>>> #返回解压数据，需重复调用，直到遇到数据结束流标记为止（因为数据是成块处理。返回的字符串中可能包含全部信息也可能不包含）
	>>> print dfd
	ashin is test bz2 of Python
	 
	>>> bzcs = bz2.compress(data)           #返回压缩数据，可设置压缩级别
	>>> print bzcs
	BZh91AY&SY"...
	>>> bzds = bz2.decompress(bzcs)         #返回解压数据
	>>> print bzds
	ashin is test bz2 of Python
	
#filecmp

[filecmp](http://docs.python.org/2/library/filecmp.html)模块用于比较文件和目录。

	>>> from filecmp import dircmp
	>>> def print_diff_files(dcmp):
	...     for name in dcmp.diff_files:
	...         print "diff_file %s found in %s and %s" % (name, dcmp.left,
	...               dcmp.right)
	...     for sub_dcmp in dcmp.subdirs.values():
	...         print_diff_files(sub_dcmp)
	...
	>>> dcmp = dircmp('dir1', 'dir2') 
	>>> print_diff_files(dcmp) 

#fnmatch 

该模块用于测试通配符是否匹配。该模块只有三个函数。

1. fnmatch(filename, pattern) 测试文件名是否匹配模式
2. fnamtchcase(filename, pattern) 同上，区分大小写
3. fileter(names, pattern) 对于names序列中的所有名称，应用fnmatch()函数

		#!/usr/bin/python
		#-*- coding: UTF-8 -*-
		
		import fnmatch
		import os
		#Example of finding files in an entire directory tree
		#using os.walk(), fnmatch, and generator
		#
		def findall(topdir, pattern):
			for path, files, dirs in os.walk(topdir):
				for name in files:
					if fnmatch(name, pattern):
						yield os.path.join(path, name)
		
		
		#Find all .py files
		for pyfile in findall(".", "*.py"):
			print pyfile

再来一个例子：

	import fnmatch
	import os
	
	for file in os.listdir('.'):
	    if fnmatch.fnmatch(file, '*.txt'):
	        print(file)
	
#glob

glob 模块返回一个目录中的全部文件名。

* glob(pattern) 返回与pattern 相匹配的路径名
* iglob(pattern) 返回与glob 相同的结果，但使用迭代器

例：
	htmlfile = glob('*.html')
	imgfiles = glob('image[0-5]*.gif')

#gzip

gzip 文件提供了一个类，即Gzipfile
。它可以用来向文件读取或写入数据，就像操作普通文件一样，不过，它会自动压缩。

#shutil

该模块用于执行高级文件操作。

* copy(src, dst)
* copymode(src, dst)
* copystat(src, dst)
* copytree(src, dst)

#tarfile

用于操作tar归档文件。

压缩：

	#!/usr/bin/env python
	import os
	import tarfile
	
	#创建压缩包名
	tar = tarfile.open("/tmp/tartest.tar.gz","w:gz")
	
	#创建压缩包
	for root,dir,files in os.walk("/tmp/tartest"):
		for file in files:
			fullpath = os.path.join(root,file)
			tar.add(fullpath)
	tar.close(）


解压tar.gz包:

	#!/usr/bin/env python
	import tarfile
	tar = tarfile.open("/tmp/tartest.tar.gz")
	names = tar.getnames()
	for name in names:
	    tar.extract(name,path="/tmp")
	tar.close()

# tempfile

如果你的应用程序需要一个临时文件来存储数据，但不需要同其他程序共享，那么用TemporaryFile函数创建临时文件是最好的选择。其他的应用程序是无法找到或打开这个文件的，因为它并没有引用文件系统表。用这个函数创建的临时文件，关闭后会自动删除。

	#!/usr/bin/python
	#-*- coding: UTF-8 -*-
	
	import os
	import tempfile
	print 'Building a file name yourself:'
	filename = '/tmp/guess_my_name.%s.txt' % os.getpid()
	temp = open(filename, 'w+b')
	try:
		print 'temp:', temp
		print 'temp.name:', temp.name
	finally:
		temp.close()
		os.remove(filename)
	
	print 'TemporaryFile:'
	temp = tempfile.TemporaryFile()
	try:
		print 'temp:', temp
		print 'temp.name:', temp.name
	finally:
		temp.close()

可以在程序中调用seek()函数，重定位文件的游标，以达到读数据的目的。如果要创建命名的临时文件，可以使用NamedTemporaryFile 函数，尽管文件带有名字，但它仍然会在close()后自动删除。

# Zipfile
该模块用户操作流行的zip 格式编码的文件。

# zlib

又一个压缩数据的模块。参考[这里](http://blog.csdn.net/jgood/article/details/4608546)。

	#coding=gbk
	import zlib, urllib
	
	fp = urllib.urlopen('http://localhost/default.html')
	str = fp.read()
	fp.close()
	
	str1 = zlib.compress(str, zlib.Z_BEST_COMPRESSION)
	str2 = zlib.decompress(str1)
	print len(str)
	print len(str1)
	print len(str2)
	 
	结果
	5783
	1531
	5783

<!--数据结构与算法-->
# abc(abstract base classes)

用于定义一个新的抽象类。@abstractmethod
定义抽象方法，派生类只有定义了该方法才能实例化。　@abstraceproperty
创建一个抽象特性，它的参数与普通property()函数相同。

	#!/usr/bin/python
	#-*- coding: UTF-8 -*-
	from abc import ABCMeta, abstractmethod, abstractproperty
	
	class Stackable():          # 在python中，应该用该语法
		__metaclass__ = ABCMeta # class Stackable(metaclass=ABCMeta)
		@abstractmethod
		def push(self, item):
			pass
	
		@abstractmethod
		def pop(self):
			pass	
	
		@abstractproperty
		def size(self):
			pass
	
	class Stack(Stackable):
		def __init__(self):
			self.items = []
		def push(self, item):
			self.items.append(item)
		def pop(self):
			return self.items.pop()
	
	class CompleteStack(Stack):
		@property
		def size(self):
			return len(self.items)
	
	s = CompleteStack()
	s.push("foo")
	print s.size #不用括号，这就是@property 的作用
	
#array

array 模块定义一个新对象类型array
,该类型的工作原理与列表极其相似，但它的内容仅限于单一类型。数组的类型在创建时确定。此模块定义了以下方法：

	array(typecode, [, initializer])

创建类型为[typecode](http://docs.python.org/3/library/array.html?highlight=array#array.array)的数组。initializer
是一个字符串或值的列表，用于初始化数组中的值。

使用方法如下：

	a = array.array("i", [1, 2, 3, 4, 5])
	b = array.array(a.typecode, (2*x for x in a))

因为使用数组是为了节省空间，所以执行"原地"操作可能更符合需要，为此，有效的方式是使用enumerate()来实现，比如：
	
	a = array.array("i", [1, 2, 3, 4, 5])
	for i, x in enumerate(a):
		a[i] = 2*x

对于大型数组，这种原地修改的运行速度比使用生成器表达式创建新数组快大约15%。

#bisect

使用二分算法来快速的维护一个有序的列表。该模块可以实现二分查找，以及相关的[其他功能](http://docs.python.org/3/library/bisect.html?highlight=bisect#bisect)。

	>>> def grade(score, breakpoints=[60, 70, 80, 90], grades='FDCBA'):
	...     i = bisect(breakpoints, score)
	...     return grades[i]
	...
	>>> [grade(score) for score in [33, 99, 77, 70, 89, 90, 100]]
	['F', 'A', 'C', 'C', 'B', 'A', 'A']

#collections

该模块定义或封装了好几种数据结构，主要有一下三个：
* deque
	双端队列，有该队列以后，我们就可以方便的模拟队列和栈。这里有一个广度优先搜索的例子：

		"""
		约定：图的存储方法
		这里的图采用邻接表的方法存储。有两种可行的方式：
		1. 边无权重：
		采用集合存储该点可到达的点集
		2. 边有权重：
		采用字典(点:边的权重)存储该点可到达的点集
		例：
		"""
		V = a, b, c, d, e, f, g, h = range(8)
		E = [
			set([b, c, f]),
			set([e]),
			set([d]),
			set([a, h]),
			set([f, g, h]),
			set([b, g]),
			set(),
			set([g])
			]
		G = (V, E)
		"""
		from collections import deque
		
		inf = float("inf")
		
		def bfs(G, s):
			"""Input: Graph G = (V, E), directed or undirected; vertex s in V
			Output: For all vertices u reachable from s, dist[u] is set to the
				distance from s to u
			"""
			V, E = G
			dist = [inf for u in V]
			dist[s] = 0
			Q = deque([s])
			while Q:
				u = Q.popleft()
				for v in E[u]:
					if dist[v] == inf:
						Q.append(v)
						dist[v] = dist[u] + 1
			return dist

* defaultdict

defaultdict
与字典基本一样，除了在对缺少键的处理上。当查找不在的键时，将调用作为default_factory
提供的函数来提供一个默认值，然后将该值保存为关联键的值。

	#!/usr/bin/python
	#-*- coding: UTF-8 -*-
	from collections import defaultdict
	s = "yeah but no but yeah but no but yeah"
	words = s.split()
	wordlocations = defaultdict(list) #set default_factory
	for n, w in enumerate(words):
		wordlocations[w].append(n)
	
	print str(wordlocations)

* namedtuple

使用数字来索引元组虽然方便，但是不够清晰，尤其是当元组很长的时候，变得难以维护，所以有了命名元组。

	#!/usr/bin/python
	#-*- coding: UTF-8 -*-
	from collections import namedtuple
	NetworkAddress = namedtuple('NetworkAddress', ['hostname', 'port'])
	a = NetworkAddress('www.python.org', 80)
	
	print a.hostname
	print len(a)
	print str(type(a))

命名元组各个方面都和普通tuple一样，不会在内置tuple中增加任何开销，如果定义仅用作数据结构的对象，那么最好使用命名元组。例如，无需像下面这样定义一个类：

	class Stock():
		def __init__(self , name, share, price):
			self .name= name
	    	self .share= share
	    	self .price =price
		
可以用命名元组来代替：

	import collections
	Stock = collections.namedtuple('Stock', 'name shares price')

* 抽象基类

collections模块定义一系列抽象基类。设计这些类是为了描述各种容器。这些类主要有两个用途，作为用户自定义对象的基类，其次，他们可以用于类型检查。

	#!/usr/bin/python
	#-*- coding: UTF-8 -*-
	import collections
	if isinstance(x, collections.Sequence) :
		last = x[-1]

#heapq

实现了堆相关的算法。
* heapify(x) 将列表原地转换为堆
* heappop(heap)
* heappush(heap, item)

它的源码分析在[这里](http://wangcong.org/blog/archives/2013)。

# operator

operator 模块提供了访问内置运算符和特殊解释器方法的函数。这样做都是为了效率。

	>>> timeit("reduce(operator.add, a)", "import operator; a=range(100)")
	5.212958812713623
	>>> timeit("reduce(lambda x, y:x+y,a)", "import operator; a = range(100)")
	11.031323194503784

#itertools

**神一样的模块，重点**[学习](http://docs.python.org/3/library/itertools.html?highlight=itertools#itertools)。该模块包含创建有效迭代器的函数，可以用各种方式对数据执行循环操作。此模块中的所有函返回的迭代器都可以与for 语句以及其他包含迭代器的函数结合使用。有点类似于C++中的函数对象，预先定义了一些有用的函数，这些函数可以组合使用。

* chain(iter1, iter2,....) 把几个列表连起来
* combinations(iterable, r) 返回所有长度为r的子序列
* count([n]) 返回一个迭代器，从n开始计数
* cycle(iterable) 对iterable 中的元素反复执行循环操作
* dropwhile(predicate, iterable) 只要predicate(item) 为True ，就丢弃iterable 中的项
* groupby(iterable, [,key]) 分组
* ifilter(predicate, iterable) 因生成predicate(item) 为True 的项
* ifilterfalse(predicate, iterable) 因生成predicate(item) 为False 的项
* imap(function, iter1, iter2, ....) 生成项 function(i1, i2,...)
* islice(iterable)
* izip(iter1, iter2, ....) 与zip 类似，不过，可以有很多项
* permutations(iterable, [,r]) 返回iterable 中所有长度为r 的项目序列
* product(iter1, iter2, ....) 生成迪卡尔积
* repeat(object, [, times]) 重复生成object
* starmap(func, [, iterable]) 创建一个迭代器，生成func(\*item)
* takewhile(predicate, [, iterable])
  生成predicate(item)为True的项，只要有一个为False ，就停止
* tee(iterable, [,n]) 
* accumulate()

		#!/usr/bin/python
		#-*- coding: UTF-8 -*-
		from itertools import *
		#在数字0,1,...10,9,8...1上执行无限循环
		for i in cycle(chain(range(10), range(10, 0, -1))):
			print i
		
		#创建a 中唯一项列表
		a = [ 1, 4, 5, 4, 9, 1, 2, 3, 4, 5, 1]
		a.sort()
		b = [k for k , g in groupby(a)] #b = [1, 2, 3, 4, 5, 9]
		
		#对x 和y 中所有可能值对组合进行迭代
		x = [1, 2, 3, 4, 5]
		y = [10, 11, 12]
		
		for	r in produce(x, y)
			print (r)


<!--数学运算-->

# decimal

python 使用双精度二进制编码表示float
数据类型，这种方式的缺点就是不能准确表示浮点数，decimal
模块能够准确的表示十进制，它还能够精确控制运算精度，有效位数，和四舍五入操作。

#fractions

有理数。

#math

各种数学函数。

#numbers

该模块定义了一组抽象基类，用于管理各种数值。

* Number
* Complex
* Real
* Rational
* Integral

该模块中的类并不用于实例化，而是用于对数值进行各种检查。例如：

	if isinstance(x, numbers.Number ):
		statements
	
	if isinstance(x, numbers.Integral)
		statements
	
# random

生成[随机数](http://docs.python.org/3/library/random.html?highlight=random#random)，非常好用。
* seed([x])
* randint(a, b)
* randrange(start, stop, [,step])
* choice(seq)
* sample(s, len)
* shuffle()
* random()

# warnings

用于发出和筛选警告信息。

#weakref

该模块用于支持弱引用，类似于boost库中的weak_ptr

#distutils

Python 3自带一个名为Distutils 的打包框架。Distutils
包含许多功能：构建工具（为你所准备），安装工具（为用户所准备），数据包格式（为搜索引擎所准备）等。

1. 组织代码（在每个目录里添加__init__)文件
2. 在最顶级目录中创建setup.py 文件

		#!/usr/bin/python
		#-*- coding: UTF-8 -*-
		from distutils.core import setup
		setup(  name       = "spam",
				version    = "1.0",
				py_modules = ['libspam']
				packages   = ['spampkg']
				scripts    = ['runspam.py']
				)


#optparse

该模块已经被 Deprecated ，现在用[argparse](http://docs.python.org/3/library/optparse.html?highlight=optparse#optparse) 模块。

	#!/usr/bin/python
	#-*- coding: UTF-8 -*-
	import optparse
	p = optparse.OptionParser()
	
	p.add_option("-o", action="store", dest="outfile")
	p.add_option("--output", action="store", dest="outfile")
	
	
	p.add_option("-d", action="store_true", dest="debug")
	p.add_option("--debug", action="store_true", dest="debug")
	
	p.set_defaults(debug=False)
	
	opts, args = p.parse_args()
	outfile = opts.outfile
	
	debugmode = opts.debug


# atexit

你懂的，就和C语言中的atexit一样。

	 import atexit, gc
	 atexit.register(gc.collect)

#doctest

在文档字符串中，写一些简单的交互式例子，使用doctest
模块测试结果是否正确。推荐使用unittest模块。


#unittest

顾名思意，进行单元测试的模块，只要包含该模块，编写单元测试类即可。

1. 编写单元测试类，继承unittest.TestCase
2. 变成测试用例，以test 开头的函数
3. 调用unittest.main()执行测试

#timeit

用于测试一条语句执行N次(默认1百万)的时间，它的定义如下：

	timeit.timeit(stmt='pass', setup='pass', timer=<default timer>, number=1000000)

还有一个repeat 函数，与timeit
几乎一样，不过，它会执行三次，并返回一个结果列表。

	>>> from timeit import timeit
	>>> timeit('s.appendleft(37)', 'import collections; s=collections.deque()', number=100000)
	0.019330978393554688
	>>> timeit('s.insert(0,37)', 's=[]', number=100000)
	3.5327699184417725
	>>> 

#base64

base64 编码，使用超级简单。

	>>> import base64
	>>> encoded = base64.b64encode('data to be encoded')
	>>> encoded
	'ZGF0YSB0byBiZSBlbmNvZGVk'
	>>> data = base64.b64decode(encoded)
	>>> data
	'data to be encoded'

编码：

	#!/usr/bin/python
	#-*- coding: UTF-8 -*-
	import base64
	
	inFile = open('a.txt')
	outFile = open('b.txt', 'w')
	
	base64.encode(inFile, outFile)
	
	inFile.close()
	outFile.close()

解码：

	#!/usr/bin/python
	#-*- coding: UTF-8 -*-
	import base64
	
	inFile = open('b.txt')
	outFile = open('c.txt', 'w')
	
	base64.decode(inFile, outFile)
	
	inFile.close()
	outFile.close()

#md5

已启用，在python3 中可以这样：

	from haslib import md5

用法如下所示：

	def getmd5(filename):
	    file_txt = open(filename,'rb').read()
	    m = md5.new(file_txt)
	    return m.hexdigest()

#csv

操作CSV文件的库。具体使用方法可以参考[这里](http://mingxinglai.com/cn/2013/02/python-database/)。

#smtplib

发送邮件。

	#!/usr/bin/python
	#-*- coding: UTF-8 -*-
	
	import smtplib
	from email.mime.text import MIMEText
	from email.mime.multipart import MIMEMultipart
	from email.mime.audio import MIMEAudio
	
	def main():
		"""send mail with python"""
	
		#set info
		sender   = "joy_lmx@163.com"
		receiver = "mingxinglai@gmail.com"
		subject  = "Faders up!"
		body     = "I naver should have moved out of Texas -J"
		
		#create a Messager object
		m            = MIMEMultipart()
		m["to"]      = receiver
		m["from"]    = sender
		m["subject"] = subject
		
		#write email body
		m.attach(MIMEText(body))
	
		#add attachment
		apart = MIMEAudio(open(audio, "rb").read(), "mpeg")
		apart.add_header("Content-Disposition", "attachment", filename=audio)
		
		#crate a SMTP object
		s = smtplib.SMTP()
		s.connect("smtp.163.com")
		s.login("joy_lmx@163.com", "password")
		s.sendmail(sender, receiver, m.as_string())#send mail
		s.close()
	
	if __name__ == '__main__':
		main()

#hashlib

该模块实现了各种安全的散列和消息摘要算法，如MD5和SHA1 .

#HTMLParser(html.parser)

解析HTML 文件的模板，功能非常有限，可以使用Beautiful Soup 。

#re

正则表达式模块。

主要函数：

1. re.compile() 编译一个正则表达式对象
2. re.search() 可以理解为正则表达式的快捷方式
3. reobj.match() 是否匹配

		>>> m = re.match(r"(\w+) (\w+)", "Isaac Newton, physicist")
		>>> m.group(0)       # The entire match
		'Isaac Newton'
		>>> m.group(1)       # The first parenthesized subgroup.
		'Isaac'
		>>> m.group(2)       # The second parenthesized subgroup.
		'Newton'
		>>> m.group(1, 2)    # Multiple arguments give us a tuple.
		('Isaac', 'Newton')


#ConfigParser 

python 3 中是configparser

读取：

* read(filename) 直接读取ini文件内容
* readfp(fp) 可以读取一打开的文件
* sections() 得到所有的section，并以列表的形式返回
* options(sections) 得到某一个section的所有option
* get(section,option) 得到section中option的值，返回为string类型

写入：

	from ConfigParser import SafeConfigParser
	from StringIO import StringIO
	 
	f = StringIO()
	scp = SafeConfigParser()
	 
	print '-'*20, ' following is write ini file part ', '-'*20
	sections = ['s1','s2']
	for s in sections:
	    scp.add_section(s)
	    scp.set(s,'option1','value1')
	    scp.set(s,'option2','value2')
 
#datetime 

该模块包含了以下几个对象：

1. date 日期
2. time  时间
3. datetime  日期和时间
4. timedelta  时间差
5. tzinfo 时区信息

#fcntl

类似与操作系统中的fcntl 调用

#io

实现了各种形式的IO

#logging

记录日志的模块

#msvcrt 

仅用于微软操作系统，提供对Microsoft Visual C 运行时大量有用的函数的访问

#signal 

信号处理

#subprocess 

#winreg

操作windows 操作系统的注册表

