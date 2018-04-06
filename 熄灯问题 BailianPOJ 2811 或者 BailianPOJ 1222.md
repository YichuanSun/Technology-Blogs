# 熄灯问题 BailianPOJ 2811 或者 BailianPOJ 1222

标签（空格分隔）： 算法 算法竞赛

---
题目有些长，就不写了，在北大百炼平台上有，1222是英文版题，2811是中文版。这道题我听的郭炜老师的课，做的真心巧妙，用的也是出乎意料。具体解题思路在下面的代码里，是老师的源程序，我手动敲的。其中有很多个地方不好懂，统一分析在代码最后。
```C++
#include <memory>
#include <string>
#include <cstring>
#include <iostream>
using namespace std;
char oriLights[5];	//存放原始输入的灯矩阵，每个字符表示一行
char lights[5];		//变化中的灯的矩阵 
char result[5];		//最终的开关结果

int GetBit(char c,int i)	//返回某一行的第i个元素，表示灯的开关情况 
{
	return (c >> i)&1;
} 
void SetBit(char &c,int i,int v)	//将某盏灯的开关情况改变	
{
	if (v)
		c|=(1<<i);
	else
		c&=~(1<<i);
}
void FlipBit(char &c,int i)		//将某盏灯的明暗情况反转
{
	c^=(1<<i);
}
void OutPutResult(int t,char result[])		//输出结果数组 
{
	cout << "PUZZLE #" <<t << endl;
	for (int i=0;i<5;i++)	{
		for (int j=0;j<6;j++)	{
			cout << GetBit(result[i],j);
			if (j<5)	cout << " ";
		}
		cout << endl;
	}
}

int main()	{
	int T;
	cin >> T;
	for (int t=1;t<=T;t++)	{			//对每一次测试用例 
		for (int i=0;i<5;i++)	{		//对每次用例的每一行 
			for (int j=0;j<6;j++)	{	//输入每一个灯的开关情况
				int s;
				cin >> s;
				SetBit(oriLights[i],j,s);	//将输入的灯的情况加入原始数组的比特位
			}
		}
		for (int n=0;n<64;++n)	{	//按开关的方式，因为一共6个数，每盏灯有2种状态，则一共有64种状态
			int switchs = n;
			memcpy(lights,oriLights,sizeof(oriLights));		//将原始灯开关数组复制一份到lights，在其中做接下来的操作
			for (int i=0;i<5;++i)	{		//对每一行灯执行操作 
				result[i]=switchs;			//将我们的开关方法输入字符型数据中，以二进制形式存储。等到条件妈满足，作为结果输出 
				for (int j=0;j<6;j++)	{	//对每一行上的每一盏灯
					if (GetBit(switchs,j))	{		//获取这一种操作时，第j位上灯的状况。如果它亮着，就执行下面的模拟，将它“按灭” 
						if (j>0)					//j>0，就将它的前一位反转 
							FlipBit(lights[i],j-1);
						FlipBit(lights[i],j);		//一定会执行的操作，将第j位反转 
						if (j<5)
							FlipBit(lights[i],j+1);	//j<5，就将它的后一位反转
					}
				}
				if (i<4)			//数组未超限时，对i+1组数组进行操作，表示按了第i行的灯后对i+1行的影响。一个异或就行了，很神奇。 
					lights[i+1]^=switchs;
				switchs = lights[i];		//显然，按掉初始时的亮灯后，形成第i行灯亮灭的序列。此时对i行操作已完成，我们要想熄灭第i行剩余灯，就必须在第i+1行按同样位置的灯。 
			}
			if (lights[4]==0)	{		//如果遍历到此处，最后一行的灯都为0，也就是全灭时，就输出result。 
		 		OutPutResult(t,result);
				break;
			}
		}
	}
	return 0;
}
```
难点：

 1. 字符型数据存储布尔值。由于一个字符型数据只占1字节，1字节=8 bit，最大存2^8=256，所以用它可以直接存储少量布尔值数据，正好可以表示这里灯的亮灭。如果将整型数据赋给字符型，字符型的比特位会表示这个整型数的二进制形式。比如下面一段代码，输入一个小于256的正整数，就能输出它在字符型数据中的存储情况。
```C++
#include <iostream>
#include <algorithm>
using namespace std;
int GetBit(char a,int i);
int main()	{
	int n;
	scanf("%d",&n);
	char a;
	a=n;
	for (int i=7;i>=0;i--)	{
		printf("%d",GetBit(a,i));
		if (i!=0)	cout << " ";
	}
	cout << endl;
	return 0;
} 

int GetBit(char a,int i)	{
	return (a>>i)&1;
}
```
其中用到了位运算的知识，忘了的只能自行百度了，我也是这样现学的。
2. 位运算。其中数次用到了位运算，用来处理二进制数很方便，很快捷。如果不熟练，最好多做一些习题来巩固一下。