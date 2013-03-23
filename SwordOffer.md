用下面这条shell语句找出那些还没有弄懂的题目

	egrep "#{2}.*" SwordOffer.md

#面试题1: 赋值运算函数

**题目：** 为string 写一个赋值运算函数

**答案：**

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
 

#面试题2: 实现Singleton 模式(C#忽略)


#面试题3: 二维数组中的查找

**题目：**
在一个二维数组中，每一行都按照从左到右递增的顺序排列，每一列都按照从上到下递增的顺序排列。请完成一个函数，输入这样的一个二维数组，和一个数字，判断该数组中是否有该整数。

**提示：**与右上角的数字比较，如果比它大，就向下移动，如果比它小，就向左移动。


#面试题4: 替换空格

**题目：**请实现一个函数，把字符串中的每个空格都替换成"%20"，例如，输入"We are
happy" ，则输出"We%20are%20happy"。

**提示：**首先问清楚，是复制到另一个数组，还是在当前数组中修改，如果是在当前数组中修改，则可以从后向前扫描，替换。首先，计算有几个空格与非空格字符，这样就可以知道需要多少空间，然后判断所需要的空间够不够，如果够，就从后向前扫描，替换。


#面试题5:从尾到头打印链表

**题目：**输入一个链表的头节点，从尾到头反过来打印每个节点的值。

**答案：**毫无疑问，使用桟，但是呢，栈和递归，是一对，所以，也可以用递归，在这里，用递归会显得更清晰明了。

	void PrintListReversingly_Recursively(ListNode *pHead)
	{
		if (pHead != NULL) 
		{
			if (pHead->m_pNext != NULL) 
			{
				PrintListReversingly_Recursively(pHead->m_pNext)
			}
			printf("%d\t\n", pHead->m_nValue);
		}
	}


#面试题6:重建二叉树

**题目：**输入二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历都不含重复的数字。

**提示：**前序遍历中，第一个数字总是树的根节点的值，在中序遍历总，根节点的值在中间，左子树的节点的值位于根节点的值的左边，而右子树的节点位于根节点的右边。这样，我们就获取了根节点，以及左子树的序列，右子树的序列，以递归的方式构建树即可。


#面试题7:用两个桟实现队列

**题目：**用两个桟实现一个队列。

**提示：**在桟1中只做插入操作，在栈2中只做弹出操作，当栈2中没有元素时，将栈1中的所有元素弹出并插入到栈2 ，这是很耗费资源的，比较节省资源的方式是在一个两个栈共用一个数组。

#面试题8:

**题目：**把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。

**提示：**太简单，没什么好说的。


#面试题9: Fibonacci 数列

**题目：**写一个函数，输入n,求Fibonacci 数列的第n项。

**提示：**不要用递归，太浪费时间，该成循环。

**相关题目：**一直青蛙一次可以跳1级台阶，也可以跳2级台阶，求该青蛙跳上一个n级的台阶总共有多少种跳法。


#面试题10:二进制中1的个数

**题目：**请实现一个函数，输入一个整数，输出该数二进制表示中1的个数。

**答案：**

	//这种方法在负数的时候会陷入死循环
	int NumberOf11(int n)
	{
		int count = 0;
		while (n) 
		{
			if (n & 1) 
				count++;	
			n = n >> 1;
		}
		return count;
	}
	
	int NumberOf12(int n)
	{
		int count = 0;
		unsigned int flag = 1;
		while (flag) 
		{
			if (n & flag) 
				count ++;
			flag = flag << 1;
		}
		return count;
	}
	
	int NumberOf13(int n)
	{
		int count = 0;
		while (n) 
		{
			++ count;
			n = (n - 1) & n;
		}
		return count;
	}


**相关题目：**不用sizeof,判断int 型占几个字节

    int main(int argc, char* argv[])  
    {  
        unsigned int a = ~0;  
        int i = 1;  
        while ( (a = a >> 1) )  
            i++;  
        printf("%d byte %d bits\n", i / 8, i);  
        return 0;  
    }  



#面试题11:数值的整数次方

**题目：**实现函数`double Power(double base, int exponent)`，求base的exponent
次方。不得使用库函数，同时不需要考虑大数问题。

**提示：**注意exponent和base为0的情况，以及exponent为负数的情况。还有就是注意`a^n = a^n/2.a^n/2`，这几乎是必须要想到的，不然就混不下去了。


#面试题12: 打印1到最大的N位数

**题目：**输入数字n ，按顺序打印出从1到最大的n
位十进制数。比如输入3,则打印1,2,3,一直到最大的3位数999.

**提示：**考虑大数的情况，所以，把它当成字符串吧，排列一下而已。


#面试题13:在O(1)的时间删除链表节点

**题目：**给定单向链表的头指针和一个节点指针，定义一个函数在O(1)时间删除该节点。

**提示：**把下一个节点中的内容复制到当前节点，然后删除下一个节点即可。


#面试题14:调整数组顺序，使奇数位于偶数前面

**题目：**输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

**提示：**实现这个功能是非常简单的，所以需要多思考，有什么可以改进的，答案是肯定的，考虑可扩展性。

**答案：**

	void Reorder(int *pData, unsigned int length, bool (*func)(int))
	{
		if (pData == NULL || length == 0) 
			return;
	
		int *pBegin = pData;
		int *pEnd = pData + length - 1;
	
		while (pBegin < pEnd) 
		{
			while (pBegin < pEnd && !func(*pBegin)) 
				pBegin++;
	
			while (pBegin < pEnd && !func(*pEnd)) 
				pEnd--;
	
			if (pBegin < pEnd) 
			{
				int temp = *pBegin;
				*pBegin = *pEnd;
				*pEnd = temp;
			}
		}
	}
	
	bool isEven(int n)
	{
		return ( n & 1) == 0;
	}


#面试题15:链表中倒数第K个节点

**题目：**输入一个链表，输出该链表中倒数第K个结点。

**提示：**注意代码的鲁棒性，考虑链表为空，链表中不足k个结点，以及正好只有k个节点的情况。

**相关题目：**求链表的中间节点，判断一个单向链表是否形成了环结构。


#面试题16:反转链表

**题目：**反转链表

**提示：**可以画图来理清思路


#面试题17:合并两个排序的链表

**提示：**注意两个链表一个为空，和两个链表都为空的情况


#面试题18:树的子结构

**题目：**输入两棵二叉树，A和B，判断B是不是A的子结构。

**提示：**递归判断即可。


#面试题19:二叉树的镜像

**题目：**请完成一个函数，输入一个二叉树，该函数输出它的镜像。

**提示：**依次递归地交换一个节点的左右孩子即可。


#面试题20:顺时针打印矩阵

**题目：**输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

**提示：**


#面试题21:包含min函数的栈

**题目：**定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的min
函数。在该栈中，调用min ，push及pop 的时间复杂度都是O(1)。

**提示：**每次向栈压入一个元素时，都向另一个辅助栈压入当前最小元素，每次弹出一个元素时，都在辅助栈中弹出一个元素。


#面试题22:栈的压入，弹出序列

**题目：**输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。

**提示：**判断一个序列是不是栈的弹出序列的规律：如果下一个弹出的数字刚好是栈顶数字，那么直接弹出，如果下一个弹出的数字不在栈顶，我们把压栈序列中还没有入栈的数字压入辅助栈，直到把下一个需要弹出的数字压入栈顶为止。如果所有的数字都压入栈了仍然没有找到下一个弹出的数字，那么该序列不可能是一个弹出序列。


#面试题23:从上往下打印二叉树

**题目：**从上到下打印二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

**提示：**就是一个层次遍历，层次遍历需要用到队列，先根后根什么的非递归，需要用到栈。


#面试题24:二叉搜索树的后序遍历序列

**题目：**输入一个整数数组，判断该数组是不是某二叉树的后序遍历结果。假设输入的数组的任意两个数字都互不相同。

**提示：**在后序遍历中，最后一个数字是树的根节点的值。数组中前面的数字可以分为两部分：第一部分是左子树的值，它们都比根节点的值小；第二部分是右子树的值，它们都比根节点的值大。


#面试题25:二叉树中和为某一值的路径

**题目：**输入一颗二叉树和一个整数，打印出二叉树节点值的和为输入整数的所有路径。从属的根节点开始往下一直到叶节点所经过的节点形成一条路径。

**提示：**路径是指根节点到叶节点，保存路径。

	void FindPath
	(
	
	 BinaryTreeNode* pRoot,
	 int expectedSum,
	 std::vector<int>& path,
	 int& currentSum
	 )
	{
		currentSum += pRoot->m_nValue;
		path.push_back(pRoot->m_nValue);
	
	
		bool isLeaf = pRoot->m_pLeft == NULL && pRoot->m_pRight == NULL;
		if (currentSum == expectedSum && isLeaf) 
		{
			printf("A path is found:\n");
			std::vector<int>::iterator iter = path.begin();
			for ( ; iter != path.end(); ++iter) 
				printf("%d\t", *iter);
	
			printf("\n");
		}
	
		if (pRoot->m_pLeft != NULL) 
		{
			FindPath(pRoot->m_pLeft, expectedSum, path, currentSum);
		}
	
		if (pRoot->m_pRight != NULL) 
		{
			FindPath(pRoot->m_pRight, expectedSum, path, currentSum);
		}
	
		currentSum -= pRoot->m_nValue;
		path.pop_back();
	}
	
##面试题26:复杂链表复制

**题目：**

**提示：**


##面试题27:二叉搜索树与双向链表

**题目：**

**提示：**


##面试题28:字符串的排列

**题目：**

**提示：**


#面试题29:数组中出现次数超过一半的数字

**题目：**数组中一个数字出现的次数超过数组长度的一半，请找出这个数字。

**提示：**最简单的方法就是快速排序的思想，但是这样会改变原始数据，另一种更巧妙的办法，我懂的，不懂？[懂](http://zhedahht.blog.163.com/blog/static/25411174201085114733349/)。


#面试题30:最小的k个数字

**题目：**输入n个整数，找出其中最小的k个数。

**提示：**看这篇[论文](http://download.csdn.net/detail/lalor/4297616)即可。


#面试题31:连续子数组的最大和

**题目：**输入一个整型数组，数组里有正数也有负数。

**提示：**动态规划的典型例子，更多详细内容，请参考[编程之美](http://blog.csdn.net/zsuguangh/article/details/6338124)。

**相关题目**最大子矩阵之和，答案在[这里](http://blog.csdn.net/lalor/article/details/7610505)


#面试题32:把数组排成最小的数

**题目：**例如，输入数组{3,32,321}打印这3个数字能排成的最小数字321323。

**提示：**就排呗。


##面试题34:丑数
**题目：**
**提示：**


#面试题35:第一个只出现一次的字符

**题目：**题目有点歧义。

**提示：**


##面试题36:数组中的逆序对

**题目：**

**提示：**


#面试题37:两个链表的第一公共节点

**题目：**找出一个公共节点。

**提示：**有两种方法，第一种，用桟，找出最后一个相同的节点，第二种方法就是获取它们的长度。


#面试题38:数字在排序数组中出现的次数

**题目：**统计一个数字在排序数组中出现的次数。

**提示：**用二分法找出该数的第一次出现和最后一次出现。


#面试题39:二叉树的深度

**题目：**输入一棵二叉树的深度。

**提示：**递归即可。

**相关题目：**判断一棵二叉树是否平衡。可以简单递归，不过耗时，可以返回当前节点的左孩子的深度与右孩子的深度。


##面试题40:数组中只出现一次的数字

**题目：**

**提示：**


#面试题41:和为s的两个数字VS和为s 的连续正数序列。

**题目：**输入一个递增排序的数组和一个数字s
，在数组中查找两个数，使得它们和正好是s 。如果有多对数字的和等于s
，输出任意一对即可。

**提示：**两个游标，有个从前往后，一个从后往前。


#面试题42:翻转单词顺序 VS 左旋转字符串

**提示：**整个字符串翻转，再翻转单词。

**第二个问题**就是旋转数组呗，见编程珠玑。


##面试题43:n个骰子的点数

**题目：**

**提示：**


#面试题44:扑克牌的顺序

**题目：**从扑克牌中随机抽5张，判断是不是一个顺子，即这5张牌是不是连续的。大小王可以看成是任意数字。

**提示：**首先，将获得的5个数排序，再统计数组中0的个数，最后统计排序之后的数组中相邻数字时间的空缺总数，如果空缺总数小于或者等于0的个数，那么这个数组就是连续的；反之则不连续。需要注意的是，有对子就不可能有顺子。


#面试题45:圆圈中最后剩下的数字

**题目：**约瑟夫环

**提示：**要是面试遇到这个问题，就算是人品爆发了。

	/*
	 * @file Josephus.c
	 * @author Mingxing LAI
	 * @version 0.1
	 * @date 2013-01-29
	 */
	#include <stdio.h>
	#include <stdlib.h>
	#include <assert.h>
	typedef struct node* link;
	struct node { int item; int next;};
	
	int main(int argc, char* argv[])
	{
		int i, N = atoi(argv[1]), M = atoi(argv[2]);
		int num;
		link x;
		struct node* data = (struct node*)(malloc(sizeof(struct node) * (N+1)));
	
		for (i = 1; i <= N ; i++) 
		{
			data[i].item = i;
			data[i].next = i + 1;
		}
		data[N].item = N;
		data[N].next = 1;
	
		x = &data[N];
		while (x != &data[x->next])
		{
			num = M % N;
			for (i = 0; i < num; i++)
			{
				x = &data[x->next];
			}
			assert(x->next <= N || x->next >= 1);
			x->next = data[x->next].next;
			N--;
		}
		assert(N == 1);
		printf("%d\n", x->item);
		free(data);
	}



#面试题46:求1+2+...+n

**题目：**等差数列，不能用乘除法，for,while,if,else,swith等关键字。

**提示：**发散思维题目。

1. 构造函数
2. 虚函数

	#include <iostream>
	using namespace std;
	
	class A;
	A *Array[2];
	
	class A
	{
		public:
			virtual unsigned int Sum( unsigned int n)
			{
				return 0;	
			}
	};
	
	class B: public A
	{
		public:
			virtual unsigned int Sum( unsigned int n)
			{
				return Array[!!n]->Sum(n-1) + n;
			}
	};
	
	int main(int argc, char* argv[])
	{
	
		const int n = 100;
		A a;
		B b;
		Array[0] = &a;
		Array[1] = &b;
	
		int value = Array[1]->Sum(n);
		cout << value << endl;
	
		return 0;
	}
	


#面试题47:

**题目：**不用加减乘除作加法

**提示：**除了位操作，你还能怎么办呢。

**相关题目**

1. 交换整数的奇数位和偶数位，答案在[这里](http://blog.csdn.net/lalor/article/details/7635987)
2. 把N的i-j 位设置为M 。把N的i-j 位清除，然后与上M即可。

		unsigned int updateBits(unsigned int  n, unsigned int m, int i, int j)  
		{  
		    int max = ~0;  
		  
		    unsigned int left = max - ((1 << j) -1);  
		    unsigned int right = ((1 << i) -1);  
		  
		    unsigned int mask = left | right;  
		  
		    return (n & mask) | (m << i);  
		}  


#面试题48:不能被继承的类

**提示：**把构造函数置为私有即可，也可以使用boost::noncopy


#面试题49:把字符串转换成整数

**题目：**

**提示：**


#面试题50:树中两个节点的最低公共祖先

**题目：**

**提示：**
