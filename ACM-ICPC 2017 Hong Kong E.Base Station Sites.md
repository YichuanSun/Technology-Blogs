# ACM-ICPC 2017 Hong Kong E.Base Station Sites

标签（空格分隔）： 算法竞赛

---
当时傻乎乎的取点，还讨论了半天，看着榜上刷出这道题的人越来越多，知道是道水题却做不出来，好尴尬。最后才想到是二分法枚举可能的长度。
```C++
#include <iostream>
#include <cstdio>
#include <algorithm>
#define N 100050
using namespace std;
int a[N];
int main()	{
	int n,m;
	while (scanf("%d %d",&n,&m)&&n&&m)	{
		for (int i=0;i<n;i++)	scanf("%d",&a[i]);
		sort(a,a+n);
		int left=0,right=a[n-1]-a[0];
		while (left<=right)	{
			int mid=(left+right)/2;
			int now=0,cnt=0;
			for (int i=0;i<n;i++)	{
				if (a[i]-a[now]>=mid)	{
					cnt++;
					now=i;
				}
			}
			if (cnt>=m-1)	left=mid+1;
			else if (cnt<m-1)	right=mid-1;
		}
		printf("%d\n",left-1);
	}
	return 0;
}
```




