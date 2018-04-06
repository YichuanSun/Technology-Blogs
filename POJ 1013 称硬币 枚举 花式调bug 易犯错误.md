# POJ 1013 称硬币 枚举 花式调bug 易犯错误

标签（空格分隔）： 算法竞赛 算法

---

哇，这道鬼题我居然调了两天，严重耽误了我的学习进度。但是发现的问题也很重要，一定要引以为戒！！！！
问题：

        Sally Jones has a dozen Voyageur silver dollars. However, only eleven of the coins are true silver dollars; one coin is counterfeit even though its color and size make it indistinguishable from the real silver dollars. The counterfeit coin has a different weight from the other coins but Sally does not know if it is heavier or lighter than the real coins. 
        Happily, Sally has a friend who loans her a very accurate balance scale. The friend will permit Sally three weighings to find the counterfeit coin. For instance, if Sally weighs two coins against each other and the scales balance then she knows these two coins are true. Now if Sally weighs 
        one of the true coins against a third coin and the scales do not balance then Sally knows the third coin is counterfeit and she can tell whether it is light or heavy depending on whether the balance on which it is placed goes up or down, respectively. 
        By choosing her weighings carefully, Sally is able to ensure that she will find the counterfeit coin with exactly three weighings.
        
        Input
        The first line of input is an integer n (n > 0) specifying the number of cases to follow. Each case consists of three lines of input, one for each weighing. Sally has identified each of the coins with the letters A--L. Information on a weighing will be given by two strings of letters and then one of the words ``up'', ``down'', or ``even''. The first string of letters will represent the coins on the left balance; the second string, the coins on the right balance. (Sally will always place the same number of coins on the right balance as on the left balance.) The word in the third position will tell whether the right side of the balance goes up, down, or remains even.
        
        Output
        For each case, the output will identify the counterfeit coin by its letter and tell whether it is heavy or light. The solution will always be uniquely determined.
        
        Sample Input
        1 
        ABCD EFGH even 
        ABCI EFJK up 
        ABIJ EFGH even 
        
        Sample Output
        K is the counterfeit coin and it is light. 

先上AC代码
```C++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
char Left[3][7];
char Right[3][7];
char result[3][7];
bool IsLight(char aim);
bool IsHeavy(char aim);
int main()	{
	int n;
	scanf("%d",&n);
	while (n--)	{
		for (int i=0;i<3;i++)
			cin >> Left[i] >> Right[i] >> result[i];
		for (char aim='A';aim<='L';aim++)	{
			if (IsLight(aim))	{
				printf("%c is the counterfeit coin and it is light.\n",aim);
			}
			else if (IsHeavy(aim))	{
				printf("%c is the counterfeit coin and it is heavy.\n",aim);
			}
		}
	}
	return 0;
}

bool IsLight(char c){       //假设假币为轻的
	for (int i=0;i<3;i++)	{
		char *pLeft=Left[i];
		char *pRight=Right[i];
		switch(result[i][0])	{
			case 'u':
				if (strchr(pRight,c)==NULL)
					return false;
				break; 
			case 'e':
				if (strchr(pLeft,c)||strchr(pRight,c))
					return false;
				break;
			case 'd':
				if (strchr(pLeft,c)==NULL)
					return false;
				break;
		}
	}
	return true;
}

bool IsHeavy(char c){       //假设假币为重的
	for (int i=0;i<3;i++)	{
		char *pLeft=Right[i];
		char *pRight=Left[i];
		switch(result[i][0])	{
			case 'u':
				if (strchr(pRight,c)==NULL)
					return false;
				break; 
			case 'e':
				if (strchr(pLeft,c)||strchr(pRight,c))
					return false;
				break;
			case 'd':
				if (strchr(pLeft,c)==NULL)
					return false;
				break;
		}
	}
	return true;
}
```
我改了不知道多少遍才AC的。下面先说一说我的错解。
我的第一个思路是遍历每一次称量，如果这次称量结果不是even,就将两个盘中的每一个字母标记都+1,表示这个字符可能是假币的怀疑次数。重复这个过程，最后比较每一个字母的标记数，被怀疑次数最多的那个就认为是假币。之后再寻找它出现不平的那次称量，输出“up”或者“down”对应的假币的轻重情况，就行了。这个思路不知道为什么，总是WA。我觉得是算法的缺陷，但是我找不出来。下面附上错解，盼望有大神帮我看看为什么错了。
```C++
#include <iostream>
#include <cstdio>
#include <cstring>
using namespace std;
void judge(char *st);
char coins[3][12];
int a[27];
int main()	{
	int n;
	scanf("%d",&n);
	while (n--)		{
		for(int i=0;i<3;i++)	{
			for (int j=0;j<12;j+=4){
				scanf("%s",&coins[i][j]);
			}
		}
		for (int i=0;i<3;i++)
			judge(coins[i]);
		int max=-10;
		for (int i=0;i<27;i++){
			if (a[i]>max)
				max=i;
		}
		char aim='A'+max;
		int i,j;
		for (i=0;i<3;i++)
			for (j=0;j<8;j++)
				if (coins[i][j]==aim)
					goto end;
		end:;
		if (j<=3&&coins[i][8]=='d')	printf("%c is the counterfeit coin and it is light.\n",aim);
		else if (j>3&&coins[i][8]=='d')	printf("%c is the counterfeit coin and it is heavy.\n",aim);
		else if (j<=3&&coins[i][8]=='u')	printf("%c is the counterfeit coin and it is heavy.\n",aim);
		else if (j>3&&coins[i][8]=='u')	printf("%c is the counterfeit coin and it is light.\n",aim);
		memset(coins,'\0',sizeof(coins));
		memset(a,0,sizeof(a));
	}
	return 0;
} 

void judge(char *st)	{
	int flag;
	if (*(st+8)=='e')
		flag=0;
	else if (*(st+8)=='u')
		flag=1;
	else if (*(st+8)=='d')
		flag=-1;
	switch(flag)	{
		case 0:	{
			for (int i=0;i<8;i++)
				a[*(st+i)-'A']--;
			break;
		}
		case 1:	{
			for (int i=0;i<8;i++)
				a[*(st+i)-'A']++;
			break;
		}
		case -1:	{
			for (int i=0;i<8;i++)
				a[*(st+i)-'A']++;
			break;
		}
	}
}
```
这种思路无效后我，我又换了一种，就是最多人用的方法，假设它是轻的和重的，挨个字母枚举。然后的问题就是如何分支了。我们面临：
 1. 每个字母枚举 12
 2. 每个字母假设 2
 3. 每行称量情况遍历 3

 这个分支循环结构太麻烦了，我不知道如何在一个函数内嵌套，应该如何嵌套，于是陷入僵局。等我看了几个题解，我发现我们将假设写成函数，然后在函数内遍历每一行的称量情况，就将问题在逻辑上简化了
。下面是初步代码
```C++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
char Left[3][5];
char Right[3][5];
char result[3][5];
bool IsLight(char aim);
bool IsHeavy(char aim);
int main()	{
	int n;
	scanf("%d",&n);
	while (n--)	{
		for (int i=0;i<3;i++)
			cin >> Left[i] >> Right[i] >> result[i];
		for (char aim='A';aim<='L';aim++)	{
			if (IsLight(aim))	{
				printf("%c is the counterfeit coin and it is light.\n",aim);
				break; 
			}
			else if (IsHeavy(aim))	{
				printf("%c is the counterfeit coin and it is heavy.\n",aim);
				break;
			}
		}
	}
	return 0;
}

bool IsLight(char aim){
	for (int i=0;i<3;i++)	{
		char *pleft=Left[i];
		char *pright=Right[i];
		if (result[i][0]=='e')
			if (strchr(pleft,aim)||strchr(pright,aim))
				return false;
		else if (result[i][0]=='u')
			if (strchr(pleft,aim)||strchr(pright,aim)==NULL)
				return false;
		else if (result[i][0]=='d')	
			if (strchr(pright,aim)||strchr(pleft,aim)==NULL)
				return false;
		else continue;
	}
	return true;
}

bool IsHeavy(char aim){
	for (int i=0;i<3;i++)	{
		char *pleft=Left[i];
		char *pright=Right[i];
		if (result[i][0]=='e')
			if (strchr(pleft,aim)||strchr(pright,aim))
				return false;
		else if (result[i][0]=='u')
			if (strchr(pleft,aim)==NULL||strchr(pright,aim))
				return false;
		else if (result[i][0]=='d')	
			if (strchr(pright,aim)==NULL||strchr(pleft,aim))
				return false;
	}
	return true;
}
```

但是调试过程又花了我一个晚上，老是WA或者RE。最后我终于找到了所有原因，现在分析如下。

 1. **if分支和switch，到底用那种好**.上面我为了模拟每一行的轻重状况，用了几个很麻烦的if-else if结构，判断条件之间的逻辑关系不太清楚，可能有重叠和漏选。这个地方也的确是我WA的一个原因——用switch就能过，if-else if就过不了。
 
 2. **switch在分函数中break与return的关系**。用了switch之后，我发现如果判断条件成立，就会return，可能不加break也可以。但是事实是，如果我不加break，还是会WA。这是因为如果没有break，条件不成立时，不会return，那就会继续执行下面的几段case，不再管是否是'u'、'e'、还是'd'。这显然与我们的愿望不同。所以说，处=出于安全考虑，switch的每一个case后面，都必须要加break。
```C++
switch(result[i][0])	{
			case 'u':
				if (strchr(pRight,c)==NULL)
					return false;
				break; 
			case 'e':
				if (strchr(pLeft,c)||strchr(pRight,c))
					return false;
				break;
			case 'd':
				if (strchr(pLeft,c)==NULL)
					return false;
				break;
		}
```
```
 3. **数组范围的设定，比要求大1还是大的多一些。**要大的多一些，比如说题中存储左右托盘和结果的字符数组，理论上说容量为4就够了。但是结果是如果我设容量为5，都不行，会报RE。所以说，设置数组空间时，最好要多一些，比规定的长度大5-6个长度单位，不要吝啬的用多少就写多少，不然就会出现这种很隐蔽的错误，找好久才能找得到。
 
 4.**参量大小的设定**.比如说我们要寻找最大值max，就要设置一个尽量小的数作为max的初值。找最小值min，就设一个尽量大的数作为min的初值。这个尽量小，尽量大，一定要是接近int（以此为例）型极限的值，不要设10000，-10000这种，有些奇葩的用例也过不了。到时候一切都没错，就这个地方被坑了，这个错误也特别不好改。

