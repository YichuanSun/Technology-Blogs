# Ants   poj 1852

标签（空格分隔）： 算法竞赛

---

        An army of ants walk on a horizontal pole of length l cm, each with a constant speed of 1 cm/s. When a walking ant reaches an end of the pole, it immediatelly falls off it. When two ants meet they turn back and start walking in opposite directions. We know the original positions of ants on the pole, unfortunately, we do not know the directions in which the ants are walking. Your task is to compute the earliest and the latest possible times needed for all ants to fall off the pole.
        
        Input
        The first line of input contains one integer giving the number of cases that follow. The data for each case start with two integer numbers: the length of the pole (in cm) and n, the number of ants residing on the pole. These two numbers are followed by n integers giving the position of each ant on the pole as the distance measured from the left end of the pole, in no particular order. All input integers are not bigger than 1000000 and they are separated by whitespace.
        
        Output
        For each case of input, output two numbers separated by a single space. The first number is the earliest possible time when all ants fall off the pole (if the directions of their walks are chosen appropriately) and the second number is the latest possible such time. 
        
        Sample Input
        2
        10 3
        2 6 7
        214 7
        11 12 7 13 176 23 191
        
        Sample Output
        4 8
        38 207

这是一道众所周知的大水题，最初是在ACM寒假集训赛里见到它，后来在《挑战程序设计竞赛》这本书里也见到了。思路如果不说的话，还觉得很难，说了就简单得不行。

其实也没什么好藏着掖着的。对两个蚂蚁相碰的情况，他们相碰后再沿原路返回，和他们相碰后交错而过的情况一模一样。为什么，一想就懂，不再赘述。上AC代码。

```C++
#include <iostream>
#include <algorithm>
#include <cmath>
#define N 1000003
using namespace std;
int a[N];
int main()	{
	int m;
	scanf("%d",&m);
	while (m--)	{
		int l,n,i=0,near=0,far=0,inear,ifar;
		scanf("%d %d",&l,&n);
		while (i<n)	scanf("%d",&a[i++]);
		i=0;
		while (i<n)	{
			inear=min(a[i],abs(l-a[i]));
			ifar=max(a[i],abs(l-a[i]));
			if (inear>near)	near=inear;
			if (ifar>far)	far=ifar;
			i++;
		}
		printf("%d %d\n",near,far);
	}
	return 0;
}
```

