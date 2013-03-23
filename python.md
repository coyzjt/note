# Python 与其他语言的异同

* python 不支持++ --操作
* 缩进和：是一起的，如果下面一行是子语句，需缩进，则上面一行末尾需要"："
* python 可以这么赋值x,y,z=5,4,3
* lambda 就是一个匿名函数对象，使用方法如：b=filter(lambda x: not x % 2, a)
* 输入使用input 和 raw_input 不过input只能输入数字
* 数据类型分为可变和不可变，可变数据类型有列表和字典，不可变数据类型有字符串，数值和元组。
* python 中无switch, 用 if elif else 来代替switch
* for, while 和其他语言一样，不过多了一个else分支。
* 程序中一切数据都是对象，每个对象都有三个基本属性，即标识，类型，值
* 一切对象都是引用计数的。当分配一个新的名字给一个对象，或者将其放入到一个容器，比如列表，元组，或者字典中，该对象的引用计数就会增加1次。与此同时，Del 语句，脱离变量作用域或者变量被重新定义，都会使对象的引用计数减少。当一个对象的引用计数减少至零时，它就会在适当时机被回收，但是，也可能出现循引用的情况。
* range()函数根据起始值，终止值及步长三个参数在内存中建立一个列表，当需要一个很大的列表时，这个函数既占内存又浪费时间，为了克服它的缺点，python 提供了xrange()函数支持。xrange()函数只有在需要值时通过计算提供值，这大大节省了内存。


# 数据结构

##list

list 相关操作有insert, index, del, append, pop, reverse, sort, extend, count, remove, len 详细内容请help(list)

##tuple

tuple 的特性：任意对象的有序集合，通过偏移读取，一旦生成不可改变，固定长度，支持嵌套

##dict
字典是唯一的内建的映射类型，可以使用任何不可变的对象作为字典的关键字（如字符串，数字，元组等）。列表，字典及包含可变对象的元组不可以作为关键字。

常用方法如下，详细内容请help(dict)

	项目                | 描述
	len(m)              | 返回m中条目个数
	m[k]                | 返回关键字k索引的元素
	m[k] = x            | 设置关键字k索引的值为x
	del m[k]            | 删除一个元素
	m.clear()           | 删除所有元素
	m.copy              | 返回m的一个浅拷贝
	m.has_key(k)        | 若m中存在key k 返回True,否则返回False
	m.items()           | 返回包含关键字和对应值(key, value)的列表
	m.keys()            | 返回由所有关键字组成的列表
	m.update(b)         | 将字典b中的所有对象加入m
	m.values()          | 返回一个包含m中所有对应值的列表
	m.get(k[,v])        | 返回m[k]，若不存在，返回v
	m.setdefault(k[,v]) | 返回m[k]，若不存在，返回v ，并设置m[k] = v
	m.popitem()         | 从m中随机删除一个元素，并以元组的形式返回其关键字和值



* 序列类型包括字符串，列表，元组，xrange
  对象及缓冲区对象，字符串和缓冲区对象是字符序列，xrange
  对象是整数序列，列表和元组是任意Python 的序列。

	项目       | 描述
	s[i]       | 返回序列s的元素i
	s[i:j]     | 返回一个切片
	len(s)     | 序列中元素的个数
	min(s)     | s中的最小值
	max(s)     | s中的最大值
	s[i]=v     | 给某个元赋新值
	s[i:j] = t | 用序列t中的所有元素替换掉s序列中的索引从i值j的元素
	del s[i]   | 删除序列s中索引为i的元素
	del s[i:j] | 删除序列s 中索引从i到j的元素

* 函数

    def	fun(arg):
    	do somethind
    	return a tuple or a value

* 函数的内置方法：

    f.__doc__ or f.func_doc // documentation string
    f.__name__ or f.func_name//function name
    f.func_code// byte-compile code
    f.func_defaults
    f.func_globals

* 控制输出格式

    f = open("out","w")     # Open file for writing
    while year <= numyears:
            principal = principal*(1+rate)
            print >>f,"%3d %0.2f" % (year,principal) #将格式文本输出到文件对象 f
            year += 1
    f.close()

