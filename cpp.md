<!-- C/C++ 笔试面试题目汇总 -->

<h1 align="center">C++面试/笔试题目汇总</h1>

**精华资料：**
[c++ 对象的内存布局1][1]
[c++ 对象的内存布局2][2]
[c++ 虚函数表解析][3]

#概念题
---

#杂项

* 如果成员变量是const 或者reference,则他们一定需要初值，不能被赋值

##如何禁止或强制使用堆分配

**禁止使用堆分配**

禁止使用堆分配的方法是，重定义new 和 delete
，这样，并将其定义为protected 或protected 或 private ，这里，不能只声明new
和delete
，是因为在一些平台上它们可能会被构造函数和析构函数隐式地调用。出于同样的原因，最好将new 和delete 定义为protected 。

	class NoHeap{
		public:
			//....
		protected:
			void *operator new(size_t){return 0;}
			void operator delete(void *){}
		private:
			void *operator new[](size_t);
			void operator delete[](void *);
	};

**强制使用堆分配**

强制使用堆分配的方法是将析构函数定义为私有的，这样，系统就无法调用析构函数，声明一个auto 变量也就非法了，当然，这样，我们自己也无法delete 掉该变量，所以需要提供一个成员函数来释放资源，如下所示：

	#include <iostream>
	using namespace std;
	
	class A {
	public:
		A (){}
		void Delete(){cout << "I'm going to die" << endl;}
	private:
		~A (){}
		int a;
	};
	
	
	int main(int argc, char* argv[])
	{
		//A a;           // 错误，强制堆分配
		A *pa = new A(); // 正确
	
		cout <<"hello, world" << endl;
		pa->Delete();
		return 0;
	}

## queue, priority_queue,stack 三种适配器分别可以建立在哪些容器上

默认的stack 和queue 都基于deque 容器实现，而priority_queue 则在vector 容器上实现。在创建适配器时， 通过将一个顺序容器制定为适配器的第二个参数，可以覆盖其关联的基础容器类型。

    //empty stack implemented on top of vector
    stack<string, vector<string> >str_stk;
    //str_stk2 is implemented on top of vector and holds a copy of svec
    stack<string, vecotr<string> str_stk2(svec);

对于给定的适配器，其关联容器必须满足一定的约束条件。stack
适配器所关联的基础容器可以是任意一种顺序容器类型。因此，stack
栈可以建立在vector, list 或者 deque 容器之上。而deque
适配器要求其关联的基础容器必须提供push_front 运算，因此，只能建立在list 或者
deque 容器之上，而不能建立在vecotr 容器上。priority_queue
适配器要求提供随即访问功能，因此可以建立在vector 或者deque 上，但不能建立在list
容器上。

##解释复制容器对象的构造函数与使用两个迭代器的构造函数之间的差别

复制容器对象的构造函数只能只能将一个容器初始化为另一容器的副本（即复制另一容器的全部元素），这种构造函数要求两个容器是同类型的；使用两个迭代器的构造函数可以将一个容器初始化为另一容器的子序列，而且采用这种构造函数不要求两个容器是同类型的。

##要标记出有效的迭代器范围，迭代器需要满足什么条件

如果迭代器first 和last 标记出有效的迭代器范围，则必须满足：first 和 last
只想同一个容器的元素或者超出末端的下一位置；如果first 和last 不相等，则对first
反复做自增运算，必须能够到达last;即容器中last 不能位于first 之前。

##假设有`map<string, vector< int > >`类型，指出在该容器中插入一个元素的insert函数具有的参数类型和返回值类型
    
    pair<const string, vector<int> > // 参数类型
    pair<map<string, vector<int> >::iterator, bool> // 返回值类型

    pair<map<string, int>::iterator, bool> ret =
    word_count.insert(make_pair(word, 1));


##智能指针可以存放在容器中吗

    auto_ptr<T> ap1(ap2)
    ap1 = ap2

上面两个操作都会将ap的所有权转给ap1,使ap成为未绑定的。

因为复制和赋值是破坏性操作，所以不能将auto_ptr
对象存储在标准容器中。标准库的容器类要求在复制或赋值之后两个对象相等。auto_ptr
不满足这一要求，如果将ap2 复制给ap1,则复制之后会有ap1 !=
ap2,赋值也类似。所以，auto_ptr 不能存放在容器中。

##matable 关键字的作用

如果我们希望类的数据成员是可以修改的，即使是const
成员函数也可以修改它，则我们可以将该成员变量声明为matable.

可变数据成员（即声明为matable
）是可变的，且永远不能为const,要将数据成员声明为可变的，必须将关键字mutable
放在成员声明之前。

    class Screen
    {
        public:
            //interface member functions
        private:
            mutable size_t access_ctr; // may change in a const members
            //other data members as before
    };

##初始化列表(与构造函数的区别)

* 成员初始化的次序就是定义成员的次序
* 不管成员是否在构造函数初始化列表中显示初始化，类类型的数据成员总是在初始化阶段初始化。

基于上述原因，我们应该尽可能在初始化列表中初始化数据成员。考虑下面的例子：

A:

    Sales_item::Sales_item(const string &book)
    {
        isbn = book;
        units_sold = 0;
        revenue = 0.0;
    }


B:

    Sales_item::Sales_item(const string &book):isbn(book), units_sold(0),
    revenue(0.0)
    {

    }

我们只考虑isbn 的初始化，在第一中情况下，由于"不管成员是否在构造函数初始化列表中显示初始化，类类型的数据成员总是在初始化阶段初始化"，所以，编译器在初始化阶段先初始化为isbn 为空字符串，然后在构造函数的函数体内再将book的值赋值给isbn,需要两个操作，即初始化和赋值，而对于第二种情况，我们直接在初始化列表中对isbn初始化，在构造函数的函数体里没有任何操作，所以效率比第一种情况高。因此，我们应该尽可能在初始化列表中对数据成员初始化。

##强制类型转换

C++ 同时提供了四种新的强制转型形式（通常称为新风格的或 C++ 风格的强制转型）： 

* const_cast(expression) 
* dynamic_cast(expression) 
* reinterpret_cast(expression) 
* static_cast(expression)

每一种适用于特定的目的：

dynamic_cast 主要用于执行“安全的向下转型（safe downcasting）”，也就是说，要确定一个对象是否是一个继承体系中的一个特定类型。它是唯一不能用旧风格语法执行的强制转型，也是唯一可能有重大运行时代价的强制转型。
    
static_cast 可以被用于强制隐型转换（例如，non-const 对象转型为 const 对象，int 转型为 double，等等），它还可以用于很多这样的转换的反向转换（例如，void\* 指针转型为有类型指针，基类指针转型为派生类指针），但是它不能将一个 const 对象转型为 non-const 对象（只有 const_cast 能做到），它最接近于C-style的转换。

const_cast 一般用于强制消除对象的常量性。它是唯一能做到这一点的 C++ 风格的强制转型。

reinterpret_cast 是特意用于底层的强制转型，导致实现依赖（implementation-dependent）（就是说，不可移植）的结果，例如，将一个指针转型为一个整数。这样的强制转型在底层代码以外应该极为罕见。
　　
旧风格的强制转型依然合法，但是新的形式更可取。首先，在代码中它们更容易识别（无论是人还是像 grep 这样的工具都是如此），这样就简化了在代码中寻找类型系统被破坏的地方的过程。第二，更精确地指定每一个强制转型的目的，使得编译器诊断使用错误成为可能。例如，如果你试图使用一个 const_cast 以外的新风格强制转型来消除常量性，你的代码将无法编译。

##RTTI

RTTI
即运行时类型识别，程序能够使用积累的指针或引用来检索这些指针或引用所指对象的实际派生类型。c++中通过下面两个操作符提供RTTI:

1. typeid 操作符，返回指针或引用所指向的实际类型
2. dynamic_cast操作符，将基类类型的指针或引用安全地转换为派生类型的指针或引用

* typeid 返回一个类型的ID,相同的类型具有相同的ID,以此来判断两个变量是否同一个类型
* dynamic_cast 将基类转化为派生类指针， 方法如下
    

    if (Drived *drivedPtr = dynamic_cast<Drived*>(basePtr))
    {
        //use the Derived object to which derivedPtr points
    }
    else
    {
        //uste the Base object to which basePtr points
    }


这里有一个很好的例子演示typeid 的作用

    #include <iostream>
    #include <typeinfo>
    #include <string>
    using namespace std;
    
    class Base {
    public:
        virtual void fun() {}
    
    private:
        int a;
    };
    
    
    class Drived: public Base {
    public:
        void fun() {}
    
    private:
        int b;
    };
    
    
    int main(int argc, char* argv[])
    {
        Base b;
        Drived d;
    
        if (typeid(b) == typeid(d)) 
            cout << "equal" << endl;
        else
            cout << "not equal" << endl;
    
    
        Base *pb = new Drived();
        Drived *pd = new Drived();
    
        if (typeid(*pb) == typeid(*pd)) 
            cout << "equal" << endl;
        else
            cout << "not equal" << endl;
    
        return 0;
    }

输出结果为：

    not equal
    equal

如果把基类中virtual 去掉，即基类没有虚函数，则输出结果为：

    not equal
    not equal

这说明了RTTI 只有在多态的时候才能起作用。

RTTI的一个经典应用就是判断两个变量是否相等，例如，有一个基类和一个派生类，有两个不知道是否相同类型的变量判断是否相等的时候，只需要首先通过typeid 判断他们是否属于同一个类型的变量，然后再调用它们的成员函数equal判断是否相等。

我们定义了一个基类，一个派生类，并且定义了一个virtual 的成员函数equal
用来判断两个对象是否相等。

    class Base{
        friend bool operator== (const Base&, const Base&);
        public:
        //interface members for Base
        protected:
        virtual bool equal (const Base&) const;
        //data and other implementation members of Base
    };
    
    class Derived: public Base{
        friend bool operator== (const Base&, const Base&);
        public:
        //other interface members for Derived
        private:
        bool equal(const Base &) const;
        //data and other implementation members of Derived
    };
    
则，判断两个对象是否相当的函数如下：

    bool operator== (const Base& lhs, const Base &rhs)
    {
        //return false if typeids are different otherwise
        //return lhs.equal(rhs)
        return typeid(lhs) == typeid(rhs) && lhs.equal(rhs);
    }
    

如果操作数类型不同，这个操作符就返回假；如果操作数类型相同，它就将实际比较操作数的工作委派给使用的virtual 函数equal 。如果操作数是Base 对象，就调用Base::equal,如果操作数是Derived 对象，就调用Derived::equal.

在每个类中，都定义自己的equal函数，派生类中的equal函数将以相同的方式开始：它们将实参强制转换为类本身的类型。

    bool Derived::equal(const Base &rhs) const
    {
        if (const Derived *dp
                = dynamic_cast<const Derived*>(&rhs)) 
        {
            //do work to compare two Derived objects and return result
        }
        else
        {
            return false;
        }
    }

如果我们没有RTTI,判断两个对象是否相等需要怎么做呢？需要4个函数，如下所示：

    bool operator== (const Base &, const Base&)
    bool operator== (const Derived&, const Derived&)
    bool operator== (const Base &, const Derived&)
    bool operator== (const Derived &, const Base&)


##explicit 关键字的作用

让我们考察下面这个类的定义：

    class Sales_item{
        public:
            Sales_item(const std::stirng &book = ""):isbn(book), units_sold(0), revenue(0.0){}
            Sales_item(std::istream &is);
            //as before
    
            bool same_isbn(const Sales_item& rhs);
    };

这里的构造函数都定义了一个隐式转换。因此，在期待一个Sales_item
类型对象的地方，可以使用一个string 或一个istream:

    string null_book = "9-999-99999-9";
    //ok:builds a Sales_item with 0 units_sold and revenue from
    //and isbn equal to null_book
    item.same_isbn(null_book);

这段程序使用一个string 类型对象作为实参传给Sales_item 的same_isbn
函数，该函数期待一个Sales_item 对象作为实参。编译器使用接受一个string
的Sales_item 构造函数从null_book 生成一个新的Sales_item
对象，新生成的(临时的)Sales_item 被传递给same_ibsn 。

上面的程序中，构造了一个临时对象，一旦isbn_same
结束，就不能再访问它。实际上，我们构造了一个在测试完成后马上就丢弃的对象，这个行为几乎肯定是一个错误。为了避免隐式类型转换，可以将构造函数声明为explicit ，以此防止在需要隐式转换的上下文中使用构造函数：

    class Sales_item{
        public:
            explicit Sales_item(const std::stirng &book = ""):isbn(book), units_sold(0), revenue(0.0){}
            explicit Sales_item(std::istream &is);
            //as before
    
            bool same_isbn(const Sales_item& rhs);
    };

explicit
关键字只能用于类内部的构造函数声明上。现在，两个构造函数就不能用于隐式地创建对象。前两个使用都不能编译：

    item.same_isbn(null_book); //error: string constructor is explicit
    item.same_isbn(cin); //error: istream constructor is explicit

如果真的需要将string 转换成Sales_item,可以通过显示转换的方式：

    string null_book = "9-999-99999-9";
    //ok:builds a Sales_item with 0 units_sold and revenue from
    // and isbn equal to null_book
    item.same_isbn(Sales_item(null_book));



##static 成员不是类对象的组成部分

普通成员都是给定类的每个对象的组成部分，而static
成员独立于任何对象而存在，不是类型对象的组成部分。因为static
数据成员不是任何对象的组成部分，所以他们的使用方式对于非static
数据成员而言是不合法的。

例如，static 数据成员的类型可以是该成员所属的类型，非static
成员被限定声明为其自身类对象的指针或引用。

    class Bar
    {
        public:
            static Bar mem1; //ok
            Bar *mem2; //ok
            Bar mem3; //error
    };

类似的，static 数据成员可用默认实参,非static
数据成员不能用作默认实参，因为它的值不能独立于所属的对象而使用。使用非static
数据成员作默认实参，将无法提供对象以获取该成员的值，因而是错误的。

##重载操作符必须具有一个类类型操作数

* 用于内置类型的操作符，其含义不能改变
    int operator*(int, int) //error
* 不能为任何内置类型定义额外的新的操作符。
    例如，不能定义接受两个数组类型操作数的operator+
* 不能通过连接合法符号来创建新的操作符
    如试图定义一个operator**
* 操作符的优级，结合性或操作数目不能改变
    如，x==y+z 总是将实参y 和z 绑定到operator+并将结果用作operator==的右操作数
* (待补充)
* IO操作符必须为非成员函数
    我们不应该将IO操作符定义为类的成员，否则，左操作数将只能是该类类型的对象

        Sales_item item;
        item << cout;

    这个用法与其他类型定义的输出操作符的正常使用方式相反，重载输入操作符时，需要处理错误和文件结束的可能性。

##如何禁止复制一个类

《Effective
C++》里的条款6明确指出，若不想使用编译器自动生成的函数，就该明确拒绝。

只要将拷贝构造函数和赋值构造函数定义为private 就可以阻止类的拷贝。

标准库中的ios_base,basic_ios都是这么干的，但是，这样还不够，因此，成员函数和friend函数还是可以调用私有的拷贝构造函数的，为了，彻底拒绝类的拷贝，可以定义一个拷贝构造函数和赋值函数为private的基类，所以需要拒绝拷贝的类，都从这个类派生，这样做的理由是，派生类的拷贝构造函数和赋值构造函数需要调用基类的拷贝构造函数或赋值构造函数，而基类的拷贝构造函数是私有的，所以拷贝失败。

代码如下：

    class Uncopybale
    {
        protected:
            Uncopybale(){}
            ~Uncopybale(){}
        private:
            Uncopybale(const Uncopybale &);
            Uncopybale& operator = (const Uncopybale&);
    };
    
    class HomeForSale: private Uncopybale
    {
    };

这个功能是如此的常用，以致于boost都提供了这么一个傀儡类，专门用来让用户继承，以阻止类的拷贝。

    #include <boost/utility.hpp>
    class do_not_copy: boost::noncopyable
    {
    
    };
    
##字符串

transform(s.begin(), s.end(), s.begin(), toupper);

if (s1.size() == s2.size() && equal(s1.begin(), s1.end(), s2.begin(),
			nocase_compare()))

##下列数组的值是什么

    string sa1[10];
    int ia1[10];

    int main()
    {
        string sa2[10];
        int ia2[10];
    }

答：sa1和sa2都包含10个空串，ia1包含10个为0的整形，ia2为长度为10的数组，没有初始值，这是因为sa1和ia1是全局变量，在C++中，会自动初始化全局变量，sa2为string字符串，因此会调用string的默认构造函数，只有ia2是没有初始化的数组。

##struct 与 class 的区别
    
用struct保留字定义的类和用class定义的类，唯一的不同只是默认的成员保护级别和默认的派生保护级别不同，除此之外，没有任何区别。


##从标准输入读入一些数，再将读取的不重复的数写到标准输出

    istream_iterator<int> cin_it(cin);
    istream_iterator<int> end_of_stream;
    
    vector<int> vec(cin_it, end_of_stream);
    sort(vec.begin(), vec.end();
    
    ostream_iterator<int> output(cout, " ");
    uniq_copy(vec.begin(), vec.end(), output);


##引用与指针的区别

1. 引用一旦被创建，就不能修改，也不能回收，指针则没有这个限制
1. 引用不能为空，指针则可以为空
1. 引用在声明的时候必须初始化，指针则没有这个限制


##内存分配方式有三种

1. 从静态存储区域分配。内存在程序编译的时候就已经分配好，这块内存在程序的整个运行期间都存在。例如全局变量，static变量。
1. 在栈上创建。在执行函数时，函数内局部变量的存储单元都可以在栈上创建，函数执行结束时这些存储单元自动被释放。栈内存分配运算内置于处理器的指令集中，效率很高，但是分配的内存容量有限。
1.  从堆上分配，亦称动态内存分配。程序在运行的时候用malloc或new申请任意多少的内存，程序员自己负责在何时用free或delete释放内存。动态内存的生存期由我们决定，使用非常灵活，但问题也最多。


##清除文件流的状态

如果程序员需要重用文件流读写多个文件，必须在读另一文件之前调用clear清除该流的状态。

例如，有如下程序：该程序中，有一个vector<string>
对象，里面存放的是文件名，程序要对每个文件中存储的单词做一些处理。假设该vector
对象命名为files,程序也许会有如下循环：

    //for each file in the vector
    while (it != files.end())
    {
        ifstream input(it->c_str()); // open file
        //if the file is  ok, read and "process" the input
        if (!input)
            break; //error: bad out!
    
        while (input >> s)//do the work on this file
            process(s);
    
    ++it // increment iterator to get next file
    }

关闭流并不能改变流对象的内部状态。如果最后的读写操作失败了，对象的状态将保持为错误模式，直到执行clear 操作重新恢复流的状态为止。调用clear 后，就像重新创建该对象一样。

如果打算重用已存在的流对象，那么while
循环必须在每次循环时记得关闭(close)和清空(clear)文件流。

    ifstream input;
    vector<string>::const_iterator it = files.begin();
    //for each file in the vector
    while (it != files.end())
    {
        input.open(it->c_str()); // open the file
        // if the file is ok, read and "process" the input
    
        if (!input) 
            break;
    
        while (input >> s) 
            process(s);
    
        input.close();
        input.clear();
        ++it
    }

如果忽略clear的调用，则循环只能读入第一个文件。这是因为，假设成功打开第一个文件，则读取文件直到文件结束或者出现其他错误条件为止，此时，input处于错误状态，如果在关闭(close)流前没有调用clear清除流的状态，接着在input上做的任何输入运算都会失败。一旦关闭该文件，再打开下一个文件时，在内层while循环上读input仍会失败，毕竟最后一次对流的读操作到达了文件结符。

##转换操作符

转换操作符是一种特殊的类成员函数。它定义将类类型值转变为其他类型值的转换。转换操作在类定义体内声名，在保留字operator 之后跟着转换的目标类型。

    class SmallInt
    {
        public:
            SmallInt(int i = 0):val(i)
        {
            if ( i < 0 || i > 255)
                throw std::out_of_range("Bad SmallInt initializer");
        }
            operator int() const {return val;}
        private:
    std:size_t val;
    };

转换函数采用如下通用形式：

    operator type();

这里，type
表示内置类型名，类类型名或由类型别名所定义的名字。对任何可作为凡属返回类型的类型（除void 外）都可以定义转换函数。一般而言，不运行转换为数组或函数类型，可以转换为指针类型以及引用类型。

**转换函数必须是成员函数，不能指定返回值类型，并且形参表必须为空**

##定位new(如何定义一个对象而不分配新的内存)

##解释一下虚函数表的概念

<!--------------------------------------------------------------------------------->

#编程题
---

##写一个string 类

已知类String的原型为：

    class String
    {
      public:
    	String(const char *str = NULL);	// 普通构造函数
    	String(const String &other);	    // 拷贝构造函数
    	~ String(void);					    // 析构函数
    	String & operate =(const String &other);	// 赋值函数
      private:
    	char  	*m_data;				// 用于保存字符串
    };

请编写String的上述4个函数。

    String::String(const char *str)
    {
        if ( str == NULL ) //strlen在参数为NULL时会抛异常才会有这步判断
        {
            m_data = new char[1] ;
            m_data[0] = '\0' ;
        }
        else
        {
            m_data = new char[strlen(str) + 1];
            strcpy(m_data,str);
        }
    } 
    
    String::String(const String &another)
    {
        m_data = new char[strlen(another.m_data) + 1];
        strcpy(m_data,other.m_data);
    }
    
    String& String::operator =(const String &rhs)
    {
        if ( this == &rhs)
            return *this ;
        delete []m_data; //删除原来的数据，新开一块内存
        m_data = new char[strlen(rhs.m_data) + 1];
        strcpy(m_data,rhs.m_data);
        return *this ;
    }

	//上面的operator = 虽然正确，可是在new 失败的时候，就完了，完了。
	//更好的方式是像下面这样
	String& String::operator =(const String &rhs)
    {
		if	(this != &rhs)
		{
			String strTemp(str);
			char * pTemp = strTemp.m_data;
			strTemp.m_data = m_data;
			m_data = pTemp;
		}
    }
    
    String::~String()
    {
        delete []m_data ;
    }

##编写strcpy函数

已知strcpy函数的原型是  

    char *strcpy(char *strDest, const char *strSrc);  

其中strDest是目的字符串，strSrc是源字符串。  

* 不调用C++/C的字符串库函数，请编写函数 strcpy

        char *strcpy(char *strDest, const char *strSrc);
        {
            assert((strDest!=NULL) && (strSrc !=NULL));	// 2分
            char *address = strDest;						// 2分
            while( (*strDest++ = * strSrc++) != ‘\0’ )		// 2分
               NULL ; 
            return address ;								// 2分
        }

* strcpy能把strSrc的内容复制到strDest，为什么还要char * 类型的返回值？  
答：为了实现链式表达式。例如:

    int length = strlen( strcpy( strDest, “hello world”) );  


<!--------------------------------------------------------------------------------->

#输出结果
---
##Q1

    #include <iostream>
    using namespace std; 
    
    struct Base{
        Base():i(1){}
        int i; 
    }; 
    
    struct Derived:Base{
        Derived():i(2){}
        int i; 
    }; 
    
    int main()
    {
        const int size = 5; 
        Base b[size]; 
        Derived d[size]; 
        int i; 
    
        for(i=0; i<size; i++)
        {
            b[i] = d[i]; 
            cout<<b[i].i<<endl; 
        }
        cout<<"-----------------------"<<endl; 
    
        Base *p = d; 
        for(i=0; i<size; i++)
            cout<<p[i].i<<endl; 
        return 0; 
    }

输出结果如下：

    1
    1
    1
    1
    1
    -----------------------
    1
    2
    1
    2
    1
    
解释如下：

Base 的结构是 |--i--| 4字节  
Derived的结构是 |--i1--|--i2--|8字节，i1属于Base  
Derived对象初始化时，Derived()将i2初始化为2，Base()将i1初始化为1.  
Derived赋值给Base直接将Base部分赋值过去，即i1  
指针加下标的访问p[i]相当于  
int(p) + sizeof(Base)\*i  
所以就出现了1，2交替  

**注意：**在gcc 4.6.3和vs2010下都是这个结果，导致这个结果的原因是基类和派生类连续存储，但是，C++语言并没有要求编译器将对象的基类部分和派生类部分连续排列（见c++ primer中文版第四版P478）。


##Q2

    #include<iostream>
    using namespace std;
     
    class Test {
        int value;
    public:
        Test(int v);
    };
     
    Test::Test(int v) {
        value = v;
    }
     
    int main() {
        Test t[100];
        return 0;
    }

结果：  
编译错误，因为类Test 没有默认构造函数。

##Q3

    #include<iostream>
    using namespace std;
     
    class Test
    {
    public:
      Test();
    };
     
    Test::Test()  {
        cout<<"Constructor Called \n";
    }
     
    int main()
    {
        cout<<"Start \n";
        Test t1();
        cout<<"End \n";
        return 0;
    }
Output:

    Start  
    End  


这是因为，`Test T1()`没有定义变量，只是声明了一个返回值为`Test`类型的无参函数。

##Q4

    #include<iostream>
     
    using namespace std;
     
    class Test {
        int &t;
    public:
        Test (int &x) { t = x; }
        int getT() { return t; }
    };
     
    int main()
    {
        int x = 20;
        Test t1(x);
        cout << t1.getT() << " ";
        x = 30;
        cout << t1.getT() << endl;
        return 0;
    }

Output:  
    
    Compiler Error

这是因为成员变量是引用，成员变量是引用的时，只能在初始化列表中初始化。正确的用法如下：

    #include<iostream>
     
    using namespace std;
     
    class Test {
        int &t;
    public:
        Test (int &x):t(x) {  }
        int getT() { return t; }
    };
     
    int main() {
        int x = 20;
        Test t1(x);
        cout << t1.getT() << " ";
        x = 30;
        cout << t1.getT() << endl;
        return 0;
    }
    
##Q5 

**思考：**如果基类有一个virtual的析构函数，输出结构是否有变化？

##Q6

    #include <stdio.h>
    int func(int x)
    {
        int countx = 0;
        while(x)
        {
            countx ++;
            x = x&(x-1);
        }
        return countx;
    } 
    
    int main(int argc, char const* argv[])
    {
        printf("%d\n", func(999));
        return 0;
    }

Output:

    8

这是因为999 的二进制表示有8个1 。

    
    echo "ibase=10;obase=2;999" | bc | tr -d '0'  | awk '{ print length($0)} '


<!--------------------------------------------------------------------------------->

#C++ 的缺点
---

##可以修改private 成员变量

    #include <iostream>
    using namespace std;
     
    class Fraction
    {
    private:
        int den;
        int num;
    public:
       void print() { cout << num << "/" << den; }
       Fraction() { num = 1; den = 1; }
       int &Den() { return den; }
       int &Num() { return num; }
    };
     
    int main()
    {
       Fraction f1;
       f1.Num() = 7;
       f1.Den() = 9;
       f1.print();
       return 0;
    }



[1]: http://haoel.blog.51cto.com/313033/124567
[2]: http://haoel.blog.51cto.com/313033/124561
[3]: http://haoel.blog.51cto.com/313033/124595

