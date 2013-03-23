<!-- 笔试面试题目汇总 -->
#笔试面试题目汇总

#链表

#字符串

#sizeof

##判断输出结果

    #include <iostream>
    using namespace std;
    
    int main(int argc, char* argv[])
    {
        int i = 1;
    
        cout <<"sizeof(i++) = " << sizeof(i++) << endl;
        cout << "i = " << i << endl;
    
        cout << "sizeof(i + 1.0) = " << sizeof( i + 1.0) << endl;
        cout << "i = " << i << endl;
    
        cout << "sizeof(i += 1.0) = " << sizeof( i += 1.0) << endl;
        cout << "i = " << i << endl;
    
        return 0;
    }


输出结果为：

    sizeof(i++) = 4
    i = 1
    sizeof(i + 1.0) = 8
    i = 1
    sizeof(i += 1.0) = 4
    i = 1

这是因为sizeof
是关键字，它会推断括号中数据的返回类型，以此得到数据类型所占的位数，但是它不会计算括号的值。

##不用sizeof,判断int 型占几个字节

    #include <stdio.h>  
    int main(int argc, char* argv[])  
    {  
        unsigned int a = ~0;  
        int i = 1;  
        while ( (a = a >> 1) )  
        {  
            i++;  
        }  
        printf("%d byte %d bits\n", i / 8, i);  
        return 0;  
    }  

>cout<<sizeof(char)<<endl;                    
>
>结果是1
>cout<<sizeof(int)<<endl;                       
>
>结果是4
>cout<<sizeof(unsigned int)<<endl;
>
>结果是4 
>cout<<sizeof(long int)<<endl;               
>
>结果是4 
>cout<<sizeof(short int)<<endl;              
>
>结果是2
>cout<<sizeof(float)<<endl;                    
>
>结果是4 
>cout<<sizeof(double)<<endl;               
>
>结果是8
>
 

##指针变量

指针变量的sizeof的值与指针所指的值没有任何关系，所以指针变量的内存大小都是相等的 ，如：

    int* a;
    
    char* b="abc";
    
    char** c=&b;
    
    sizeof(a)==sizeof(b)==sizeof(c)==4;

但是

    sizeof(*a)=4;
    
    sizeof(*b)=1;
    
    sizeof(*c)=4;
    
    sizeof(**c)=1;

##数组的大小

数组的sizeof值等于数组所占用的内存字节数

    char a1[] = "abc";
    int a2[3];
    sizeof( a1 ); // 结果为4，字符 末尾还存在一个NULL终止符
    sizeof( a2 ); // 结果为3*4=12（依赖于int）

但是，当数组作为函数参数的时候呢？这时候，数组是传地址的，即这时用指针来处理 ，所以，如果有：

    void foo(char a[])
    {
        int b= sizeof( a ); // c == 4
    }

所以，这时候，a的类型为char\*，而sizeof(char\*)==4;

##联合体和结构体

**联合体：**

    union a{int a;char b;float c;};

由于联合体公用储存空间，所以只取最大的来计算联合体占用空间，这里sizeof(a)==4;

**结构体：**

比较麻烦，注意3点：

1. 首地址应为结构中最宽成员的倍数
1. 整个结构体长度应为最宽成员的倍数
1. 把单一元素看成结构体

如:

    struct a{double a;char b;int c;};
    sizeof(a)==8+1+3+4==16;

这里，先分配double，首地址为8的倍数0，再分配char，这时其偏移量为8，为1的倍数，再分配int，其偏移量为9，不是4的倍数，应该用3个空字节补齐，所以int的偏移为9+3=12，再加上4=16，这时16为最宽成员8的倍数，所以该结构体的长度为16。

 
当结构体中包含结构体时，最宽成员不包括内部结构体，最宽成员应从这两个结构体的基础成员中去找。

    struct a{char a;int b;};
    
    sizeof(a)==8;
    
    sturct b{char a; a b;char c};
    
    sizeof(b)==16;

把内部结构体打开来看，分配char为1，再分配a，a中最宽成员为4，则其起始偏移应为最宽的倍数，1+3=4，4+8=12，再分配char，这时，其偏移为12，是char的倍数，12+1=13，根据规则2，再用3来补齐，13+3=16，是int的倍数，所以结构体b的长度为16.

##类的sizeof大小

1. 空类大小为1
1. 虚函数大小为4，构造析构不算大小
1. 类大小等于所有数据成员大小之和
1. 继承中类大小等于父类和子类的数据成员大小之和

##空结构体的大小为什么不为0

    #include <stdio.h>  
      
    struct stu  
    {  
    };  
    int main(int argc, char* argv[])  
    {  
            printf("%d\n", sizeof(struct stu));  
            return 0;  
    }  

用g++编译输出结果为1，用vc++6.0/vs2008编译输出结果为1，只有**用gcc编译输出结果为0**，经常考的题目是定义一个空的类，然后sizeof这个类，输出结果为1，问：为什么是1不是0？

答：编译器不能产生一个没有任何容积的数据类型，编译器认为任何一种数据类型都有其大小，用他来定义一个变量能够分配确定大小的空间，所以，编译器认为任何数据类型都有其大小。并且，编译器构造一个结构体数据类型是用来打包一些数据的，而最小的数据成员(char)需要一字节，编译器为每个结构体类型至少预留一个字节的空间，所以空结构体/空类的大小为一字节。

下面程序的输出结果：

    #include <iostream>
    using namespace std;
      
    class Empty
    {};
      
    class Derived1 : public Empty
    {};
      
    class Derived2 : virtual public Empty
    {};
      
    class Derived3 : public Empty
    {    
        char c;
    };
      
    class Derived4 : virtual public Empty
    {
        char c;
    };
      
    class Dummy
    {
        char c;
    };
      
    int main()
    {
        cout << "sizeof(Empty) " << sizeof(Empty) << endl;
        cout << "sizeof(Derived1) " << sizeof(Derived1) << endl;
        cout << "sizeof(Derived2) " << sizeof(Derived2) << endl;
        cout << "sizeof(Derived3) " << sizeof(Derived3) << endl;
        cout << "sizeof(Derived4) " << sizeof(Derived4) << endl;    
        cout << "sizeof(Dummy) " << sizeof(Dummy) << endl;
      
        return 0;
    }

输出结果如下：

    sizeof(Empty) 1
    sizeof(Derived1) 1
    sizeof(Derived2) 4
    sizeof(Derived3) 1
    sizeof(Derived4) 8
    sizeof(Dummy) 1

**思考：**如果类Empty 有一个虚析构类，会怎么样？

下面对话摘子剑指offer 。

**面试官:**定义一个空的类型，里面没有任何成员变量和成员函数，对该类型求sizeof
，得到的结果是多少？

**应聘者:**答案是1.

**面试官:**为什么不是0 ？

**应聘者:** 空类型的实例中不包含任何信息，本来求sizeof 应该是0
，但是我们声明该类型的实例的时候，它必须在内存中占有一定的空间，否则无法使用这些实例。至于占用多少内存，有编译器决定。Visual Studio 中每个空类型的实例占用1字节的空间。

**面试官:**如果在该类型中添加一个构造函数和析构函数，再对该类型求sizeof
，得到的结果又是多少？

**应聘者:**和前面一样，还是1
。调用构造函数和析构函数只需要知道函数的地址即可，而这些函数的地址只与类型相关，而与类型的实例无关，编译器也不会因为这两个函数而在实例内添加任何额外的信息。

**面试官:**那如果把析构函数标记为虚函数呢？
**应聘者:**C++ 编译器一旦发现一个类型中有虚拟函数，就会为该类型生成虚函数表，并在该类型中的每一个实例中添加一个指向虚函数表的指针。

#树
