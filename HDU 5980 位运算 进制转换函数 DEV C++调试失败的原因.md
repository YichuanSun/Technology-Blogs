# HDU 5980 位运算 进制转换函数 DEV C++调试失败的原因

标签（空格分隔）： 算法竞赛 编程窍门

---

        Find Small A
        Time Limit: 2000/1000 MS (Java/Others)    
        Memory Limit: 65536/65536 K (Java/Others)
        Total Submission(s): 427    
        Accepted Submission(s): 220
        
        
        Problem Description
        As is known to all,the ASCII of character 'a' is 97. Now,find out how many character 'a' in a group of given numbers. Please note that the numbers here are given by 32 bits’ integers in the computer.That means,1digit represents 4 characters(one character is represented by 8 bits’ binary digits).
         
        
        Input
        The input contains a set of test data.The first number is one positive integer N (1≤N≤100),and then N positive integersai (1≤ ai≤2^32 - 1) follow
         
        
        Output
        Output one line,including an integer representing the number of 'a' in the group of given numbers.
         
        
        Sample Input
        3
        97 24929 100
         
        
        Sample Output
        3
看到这题，有何感想？当时我没用位运算去做，我用的对256取商，取模。但是这里碰到一个问题：**long long型数据在做模运算时，有些编译器（比如dev C++）内存会崩溃，导致内存溢出。这也是dev无法正常调试的原因之一**
其实，可以用位运算很好地实现这个过程，程序如下
```C
#include <stdio.h>

int main()
{
	int n,p=0;
	long long i;
	scanf("%d",&n);
	while (n--)
	{
		scanf("%lld",&i);
		while (i!=0)
		{

			if ((i&255)==97)
				p++;
			i = i >> 8;
		}
	}
	printf("%d\n",p);
	return 0;
}
```
用位运算，比用其他方法写起来简单多了。但是注意，**位运算的优先级比较低，在关系运算符之下，逻辑运算符之上**

除此之外，补充一个进制转换函数：itoa().

        该函数包含在stdlib库中，用途是将一个十进制数转换为任意进制数
        itoa(number,array,system)
        结果是一个字符串，分别存放转换后的进制数的每一位。
如下面的范例程序
```C
#include <stdio.h>
#include <stdlib.h>
int main()
{
	int j=1001;
	char a[1001];
	printf("%s\n",itoa(j,a,2));
	return 0;
}
```