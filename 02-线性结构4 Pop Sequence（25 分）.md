# 02-线性结构4 Pop Sequence（25 分）

标签（空格分隔）： 数据结构 C++

---
    02-线性结构4 Pop Sequence（25 分）
    Given a stack which can keep M numbers at most. Push N numbers in the order of 1, 2, 3, ..., N and pop randomly. You are supposed to tell if a given sequence of numbers is a possible pop sequence of the stack. For example, if M is 5 and N is 7, we can obtain 1, 2, 3, 4, 5, 6, 7 from the stack, but not 3, 2, 1, 7, 5, 6, 4.
    
    Input Specification:
    Each input file contains one test case. For each case, the first line contains 3 numbers (all no more than 1000): M (the maximum capacity of the stack), N (the length of push sequence), and K (the number of pop sequences to be checked). Then K lines follow, each contains a pop sequence of N numbers. All the numbers in a line are separated by a space.
    
    Output Specification:
    For each pop sequence, print in one line "YES" if it is indeed a possible pop sequence of the stack, or "NO" if not.
    
    Sample Input:
    5 7 5
    1 2 3 4 5 6 7
    3 2 1 7 5 6 4
    7 6 5 4 3 2 1
    5 6 4 3 7 2 1
    1 7 6 5 4 3 2
    Sample Output:
    YES
    NO
    NO
    YES
    NO
看了半天才看明白，其实和当初做的那道经典的UVA514 Rail是一样的。不一样的地方就是现在的栈有最大容量。只需要写一个栈的模拟就行了。
此外，还需要注意的就是边界条件了。这里的边界条件，就是程序跳出循环的点。YES的条件很好找，只需要i遍历全部元素就行了。但是NO呢？这时考虑到，由于每次元素都需要进栈，之后再判断该元素是否等于目标元素，所以只需要判断栈空间满了的时候，栈顶元素是否等于目标序列对应元素就行了，如果不等，就输出NO。
```C++
#include <iostream>
#include <algorithm>
#include <stack>
#define N 1003
using namespace std;
int main()	{
	int m,n,k;
	scanf("%d %d %d",&m,&n,&k);
	while (k--)	{
		int aim[N],i=1,j=1;
		stack<int> st;
		for (int ii=1;ii<=n;ii++)
			scanf("%d",&aim[ii]);
		while (1)	{
			if (st.empty())	{st.push(j);j++;}
			else if (!st.empty()&&st.top()==aim[i])	{st.pop();i++;}
			else	{st.push(j);j++;}
			if (i>n)	{printf("YES\n");break;}
			else if (st.size()>=m)	{
				if (st.top()!=aim[i])	{
					printf("NO\n");
					break;
				}
			}
		}
	}
	return 0;
}
```



