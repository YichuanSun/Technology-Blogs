﻿# UVA 1339  Ancient Cipher 古老的密码 字符出现次数

标签（空格分隔）： 编程错题 算法竞赛 算法 紫书

---
题干很长，不妨参考紫书上的解释。

        给定两个长度相同且不超过100的字符串，判断是否能把其中一个字符串的各个字母重排，然后对26个字母做一个一一映射，使得两个字符串相同。例如，JWPUDJSTVP重排后可以得到WJDUPSJPVT，然后把每个字母映射到它前一个字母（B->A,C->B,···，Z->Y,A->Z)，得到VICTORIOUS。输入两个字符串，输出YES或者NO。

这道题我刚开始看没有头绪，如果按照书后面的内容来看的话，这道题应该用排序函数，但是这样的话肯定时间复杂度得炸了，并且可能性太多，穷举容易出错还不好写，所以必须想别的办法。书上说，这样只需要考虑，让出现次数相同的字母数一样多就行了。就是str1中有n个出现m次的字母，那str2中也得有n个出现m次的字母。所以，我们需要两个数组，统计两个字符串中用字母序号代表的字母的出现次数。除此之外，我们还需要另外两个数组，来储存出现n次的字母的数量。

代码如下，对于题中需要的6个数组，我用str1和str2代表两串字符串，num1和num2数组代表各个字母的出现次数，jud1和jud2表示出现n次的字母数。
```C
#include <stdio.h>
#include <string.h>
#define N 101
int main()
{
	char str1[N],str2[N];
	while (gets(str1))
	{
		gets(str2);
		int len=strlen(str1),i;
		int num1[N],num2[N],jud1[N],jud2[N];
		memset(num1,0,sizeof(num1));
		memset(num2,0,sizeof(num2));
		memset(jud1,0,sizeof(jud1));
		memset(jud2,0,sizeof(jud2));
		for (i=0;i<len;i++)
		{
			num1[str1[i]-'A'+1]++;
			num2[str2[i]-'A'+1]++;
		}
		for (i=0;i<27;i++)
		{
			jud1[num1[i]]++;
			jud2[num2[i]]++;
		}
		for (i=0;i<27;i++)
			if (jud1[i]!=jud2[i])
				break;
		if (i==27)
			printf("YES\n");
		else
			printf("NO\n");
	}
	return 0;
} 
```
题中出现了一个很有趣的现象。数组大小小于等于100时，就会WA。这个错误我找了很久，最后才找到。想了半天，我觉得，只能用字母出现次数来解释了。测试用例中肯定有出现次数为100的，100次超出了jud的范围，所以发生了数组越界，就炸了。但是这么坑人的用例也实在太无聊了，有这个兴趣弄100，何不弄1000，10000？理论上说，只要重复次数足够大，算法对，程序也不可能通过的。单纯考察范围，没有任何意义。最后我不停提交，找到的最小数组大小为101，我将它宏定义为N。

除此之外，题目中这种字符串中元素出现次数的方法，也就是将数组作为下标的方法值得注意，这是一个通法。但是嵌套多重数组（就像题目中用了三重），容易糊涂，最好在旁边写注释，别把自己绕晕了。





