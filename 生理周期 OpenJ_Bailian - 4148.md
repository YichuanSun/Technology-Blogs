# 生理周期 OpenJ_Bailian - 4148

标签（空格分隔）： 算法竞赛

---
    生理周期 OpenJ_Bailian - 4148 
    
    人生来就有三个生理周期，分别为体力周期、感情周期和智力周期，它们的周期长度分别为23天、28天和33天。每一个周期中有一天是高峰。在高峰这天，人会在相应的方面表现出色。例如，在智力周期的高峰，人会思维敏捷，注意力容易高度集中。因为三个周期的长度不同，所以通常三个周期的高峰不会落在同一天。对于每个人，想知道何时三个高峰落在同一天。对于每个周期，会给出从当前年份的第一天开始，到出现高峰的天数（不一定是第一次高峰出现的时间）。给定一个从当年第一天开始的天数，你的任务是输出从给定时间开始（不包括给定时间），下一次三个高峰落在同一天的时间（距给定时间的天数）。例如：给定时间为10，下次出现三个高峰同一天的时间是12，则输出2（注意这里不是3）。
    
    Input
    输入包含多组数据，每一组数据由四个整数组成，数据以-1 -1 -1 -1 结束。 
    对于四个整数p, e, i和d，p, e, i分别表示体力、情感和智力高峰出现的时间（时间从当年的第一天开始计算）。d是给定的时间，可能小于p, e或i。所有给定时间是非负的并且小于或等于365，所求的时间小于或等于21252。
    Output
    从给定时间起，下一次三个高峰同一天的时间（距离给定时间的天数）。
    Sample Input
    0 0 0 0
    0 0 0 100
    5 20 34 325
    4 5 6 7
    283 102 23 320
    203 301 203 40
    -1 -1 -1 -1
    Sample Output
    Case 1: the next triple peak occurs in 21252 days.
    Case 2: the next triple peak occurs in 21152 days.
    Case 3: the next triple peak occurs in 19575 days.
    Case 4: the next triple peak occurs in 16994 days.
    Case 5: the next triple peak occurs in 8910 days.
    Case 6: the next triple peak occurs in 10789 days.
这道水题还是很坑的，由于说了所求结果小于等于N=21252，所以必须要设置line9的x<=N+d才行。因为即便x超了N，它最后也还是会x-d，只要x-d<=N就行了，所以写成x<=N+d.这个小错误改了好久。
```C++
#include <iostream>
#include <algorithm>
#define N 21252
using namespace std;

int main()	{
	int p,d,e,i,k=1;
	while (scanf("%d %d %d %d",&p,&e,&i,&d)==4&&p+1&&d+1&&e+1&&i+1)
		for (int x=d+1;x<=N+d;x++)
			if (((x-p)%23==0)&&((x-e)%28==0)&&((x-i)%33==0))	{
				printf("Case %d: the next triple peak occurs in %d days.\n",k++,x-d);
				break;
			}
	return 0;
}

```




