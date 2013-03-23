<!--Array and hash table-->
#1.1 implament an algorithm to determine if a string has all unique characters.  What if you can not use additional structures?

1. 简单的hash即可
2. 还可以使用位向量节省空间
3. 如果不运行使用额外的空间，就用两个for循环，时间复杂度为O(n^2)
4. 如果允许修改字符串，就排序，时间复杂度为O(nlogn)

#1.2 write code to reverse a C-style string.

一个更好的问题是置逆英文里单词的顺序，先置逆整个句子，再置逆每个单词。

#1.3 Design an algorithm and write code to remove the duplicate characters in a string without using and additional buffer. 

NOTE: One or two additional variables are fine. An extra copy of the array is not.

FOLLOW UP:Write the test cases for this method.

Ask the interviewer, can we use an additional array of constant size ? if the
answer is yes, the algorithm is :

1. for each character, check is a duplicate of already found characters.
2. skip duplicate characters and update the non duplicate characters.

Time complexity is O(N^2 )

TEST CASES:

1. string does not contain any duplicates.
2. string contain add duplicates.
3. NULL string
4. string with all continuous duplicates.

#1.4 write a method to decide if two strings are anagrams or not

There are two easy ways to solve this problem.

1. sort the strings
2. check is the two string have identical counts for each unique char.

#1.5 write a method to replace all spaces in a string with "%20"

从后向前扫描即可。

#1.6 given an image represented by an NxN matrix, where each pixel in the image is 4 bytes, write a method to rotate the image by 90 degrees. Can you do this in place?

没有难度，考的就是细心。

	#include <iostream>
	#include <assert.h>
	using namespace std;
	
	const int N = 5;
	
	void printMatrix(int data[][N])
	{
		int i, j;
		for (i = 0; i < N; i++) 
		{
			for (j = 0; j < N; j++) 
			{
				cout << data[i][j]<< "\t";
			}
			cout << endl;
		}
	}
	
	void createMatrix(int data[][N])
	{
		int k = 0;
		int num = 1;
	
		for (int layer = 0; layer < N / 2; layer++ ) 
		{
			int first = layer;
			int last = N - 1 - layer;
	
			for ( k = first; k < last; k++)//top
				data[first][k] = num++;
	
			for ( k = first; k < last; k++)//right
				data[k][last] = num++;
	
			for ( k = last; k > first; k--)//down
				data[last][k] = num++;
	
			for ( k = last; k > first; k--)//left
				data[k][first] = num++;
		}
	
		if (N % 2) 
			data[N/2][N/2] = N * N;
	
	
	
	}
	
	void rotateMatrix(int data[][N])
	{
		cout << "\n======================================="<<endl;
		for (int layer = 0; layer < N / 2; ++layer) 
		{
			int first = layer;
			int last = N - 1 - layer;
			for (int i = first; i < last; ++i) 
			{
				int offset = i - first;
				int top = data[first][i];//save top
				//left -> top
				data[first][i] = data[last - offset][first];
				//bottom -> left
				data[last - offset][first] = data[last][last - offset];
				//right -> bottom
				data[last][last - offset] = data[i][last];
				//top->right
				data[i][last] = top;
			}
		}
	}
	
	
	int main(int argc, char* argv[])
	{
		int data[N][N] = {0};
		createMatrix(data);
		printMatrix(data);
		rotateMatrix(data);
		printMatrix(data);
		return 0;
	}

#1.7 write an algorithm such that if an element in an MxN matrix is 0, its entire row and column is set to 0.

扫描一遍矩阵，保存值为0的行和列，然后对所在的行和列置零。

#1.8 Assume you have a method is substring which checks if one word is a substring of another.Given two strings, s1 and s2, wirte code to check if s2 is a rotation of s1 using only one call to issubstring.

首先判断s1 与 s2 的长度，长度相等则继续判断，如果s2 是s1s1
的字串，则满足条件。


<!--Linked Lists-->
#2.1 write code to remove duplicates from an unsorted linked list.

FOLLOW UP:how would you solve this problem if a temporary buffer is not
allowed.

1. 用hash table 即可。
2. 如果不能用hash table ，那就只能迭代了，时间复杂度为O^2

#2.2 implement an algorithm to find the nth to last element of a singly linked list

老掉牙的问题了，你懂的。

#2.3 implement an algorithm to delete a node in the middle of a single linked list, given only access to that node.

老掉牙的问题了，你懂的。

#2.4 you have two numbers represented by a linked list, where each node contains single digit. The digits are stored in reverse order, such that the 1st digit is at the head of the list. write a function that add two numbers and return the sum as a linked list.

链表做加法嘛，松松的。

#2.5 Given a circular linked list, implement an algorithm which returns node at the beginning of the loop.

1. 用两个指针，一个一次走一步，一个一次走两步，我们可以通过数学公式决定如果有环，两个指针必然相遇
2. 知道有环以后，在两个指针相遇的地方，固定一个指针，移动另一个指针，再次相遇时知道环长为L 。
3. 让一个指针先走L步，另一个指针才开始走，相遇时，就是环的入口。

可以参考[这里](http://www.gocalf.com/blog/circle-of-link-list.html/comment-page-1#comment-3334)

关于链表，[这里](http://blog.csdn.net/lalor/article/details/7607486)还有很多问题，可以参考。


<!--Array and hash table-->

#3.1 Describe how you could use a single array to implement three stacks.

#3.2 How would you design a stack which , in addition to push and pop, also has a function min which returns the minimum element? Push, pop, and min should all operate in O(1) time.

剑指Offer里面有，讲得非常清楚。

#3.3 implement a set of stack.

#3.4 The Towers of Hanoi

#3.5 Implement a MyQueue class which implement a queue using two stacks.

so easy.

#3.6 write a program to sort a stack in ascending order. You should not make any assumptions about how the stack is implemented. The following are the only functions that should be used to write program:push|pop|peek|isEmpty

需要用到一个辅助栈，假设有两个栈s1 和 s2
，数据都保存到s中，现在开始排序，将排序后的结果保存到s中。方法如下：

首先，取出s1中的元素，判断是否大于s2中最顶上的元素，如果大于，就直接压入，如果不大于，就把s2 中大于 该元素的值取出，压入s1 ，反复执行，时间复杂度为O(n^2)

<!--Trees and Graphs-->

#4.1 Implement a function to check if a tree is balanced. For the purposes of this question, a balanced tree is defined to be a tree such that no two leaf nodes differ in distance from the root by more than one.

一开始没想到，看来图论学得不深入啊。

分别求出树的最大深度和最小深度，然后判断即可。

#4.2 Given a directed graph, design an algorithm to find out whether there is a route between two nodes.

深度优先搜索或者广度优先搜索。

#4.3 Given a sorted( increasing order) array, wirte an algorithm to create a binary tree with minimal height

algorithm:
1. Insert into the tree the middle element of the array.
2. Insert (into the left subtree) the left subarray elements.
3. Insert (into the right subtree) the right subarray elements.
4. Recurse

#4.4 Given a binary search tree, design an algorithm which creates a linked list of all the nodes at each depth.

层次遍历即可，为了知道一层结束，需要添加一个标致，推荐使用傀儡节点。

#4.5 Write an algorithm to find the 'next' node( in-order successor) of a given node in a binary search tree where each node has a link to its parent.

自己以前[做过](http://blog.csdn.net/lalor/article/details/7621239)都忘了，无语。

分为三种情况：
1.一个节点有右孩子，则在中序遍历中，该节点的后继是它的右子树的最左节点。
2. 这个节点是它父亲的左孩子，则该节点的后继节点是它的父亲
3. 这个节点是它父亲的右孩子，则需要一直向上搜索，直到它们n-1代祖先是它第n代祖先的左孩子，则它的后继就是第n个祖先。如果一直搜索到根节点，也没有找到n-1代祖先是它第n代祖先的左孩子，则该节点是整个树的中序遍历中的最后一个节点，即它没有后继。

#4.6 Design an algorithm and write code to find the first common ancestor of two nodes in a binary tree. Avoid storing additional nodes in a data structure.

详细内容，请参考剑指offer里的最后一题。

#4.7 You have two very large binary trees: T1 with millions of nodes, and T2, with hundreds of nodes. Create an algorithm to decide if T2 is a subtree of T1.

详细内容，请参考剑指offer。

#4.8 You are given a binary tree in which each node contains a value. Design an algorithm to print all paths which sum up to that value. Note that it can be any path in the tree-it does not have to start at the root.

详细内容，请参考剑指offer。

<!--Bit Manipulation-->
#5.1 You are given two 32-bit numbers, N and M, and two bit positions, i, and j. write a method to set all bits between i and j in N equal to M.

**SOLUTION**

This code operates by clearing all bits in N between positon i and j , and then ORing to put M in there.说得俗一点，就是把N
中i到j位的1清除（挖空），然后与上M即可。

	unsigned int updateBits(unsigned int  n, unsigned int m, int i, int j)  
	{  
	    int max = ~0;  
	  
	    unsigned int left = max - ((1 << j) -1);  
	    unsigned int right = ((1 << i) -1);  
	  
	    unsigned int mask = left | right;  
	  
	    return (n & mask) | (m << i);  
	}  

#5.2 Given a number that is passed in as a string, print the binary representation. If the number can not be represented accurately in binary.  print "ERROR"

如果整数部分大于32位或者小数部分大于32位就输出ERROR ，否则，正常输出即可。

#5.3 Given an integer, print the next smallest and next largest number that have the same number of 1 bits in their binary representation.

#5.4 Explain what the following code does: ((n & ( n - 1)) == 0)

赖明星，你懂的。

#5.5 Write a function to determine the number of bits required to convert integer A to integer B.

异或一下，判断有几个1就可以了。

#5.6 Write a program to swap odd and even bits in an integer with as few instructions as possible.

	public static int swapOddEvenBits(int x)
	{
		return ( ((x&0xaaaaaaaa) >> 1) | ((x &0x55555555) << 1));
	}

<!--Recursion-->
#8.1 Write a method to generate the nth Fibonacci number

谁用递归谁脑残。

#8.3 Write a method that return all subsets of a set.

1. 用二进指表示法
2. 递归＋回溯

	#include <iostream>
	using namespace std;
	int N = 5;
	
	void printArray(int data[], int len)
	{
		int i;
		for (i = 0; i < len; i++) 
			cout << data[i] << "\t";
	
		cout << endl;
	}
	
	void printSubsets(int source[],int target[], int len, int index)
	{
		for (int k = index; k < N; k++) 
		{
			target[len] = source[k];
			printArray(target, len + 1);
			printSubsets(source, target, len + 1,  k+1);
		}
	}
	
	int main(int argc, char* argv[])
	{
		int a[] = {1, 2, 3, 4, 5};
		//int *b = (int)malloc(sizeof(a));
		int *b = new int[N];
		printSubsets(a, b, 0, 0);
		return 0;
	}
	
#8.4 Write a method to compute all permutations of a string.

1. 首选取出第一个字符
1. 那么第一个字符可以插入到身下的字符的不同排列的不同位置
1. 递归实现之

时间复杂度为O(n!)

#8.5 Implement an algorithm to print all valid combinations of n-pairs of parentheses.

1. 用两个数来记录左括号与右括号的个数；
1. 左括号仍然大于0,可以放左括号，递归；
1. 左括号少于右括号，也就说，再已经排放的括号里面，左括号比右括号多，所以，可以放右括号，递归。



<!--Sorting and Searching-->
#9.1 You are given two sorted arrays. A and B, and A has a large enough buffer at the end to hold B. Write a method to merge B into A in sorted order.

这个。

#9.2 Write a method to sort an array of strings so that all the anagrams are next to each other

signature

#9.3 Given a sorted array of n integers that has been rotated an unknow number of times, given an O(logn) algorithm that finds an element in the array. You may assume that the array was originally in increasing order.

详细信息，请见剑指offer.

#9.4 If you have a 2GB file with one string per line, which sorting algorithm would you use to sort the file and why ?

可以用多道排序嘛。

#9.5  Given a sorted array of strings which is interspersed with empty strings, write a method to find the location of a given string.

you know.遇到空串向前移动，忽略它们就好了。

#9.6 Given a matrix in which each row and each column is sorted, write a method to find an element in it.

这题都考烂了，估计碰不到了吧。

<!--mathematical-->

<!--chapter11 system design and memory limits-->

<!--chapter12 Testing-->

<!--chapter13 Virtual functions-->
