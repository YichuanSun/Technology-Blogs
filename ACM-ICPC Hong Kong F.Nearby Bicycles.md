# ACM-ICPC Hong Kong F.Nearby Bicycles

标签（空格分隔）： 算法竞赛

---
水题，但是中间好像有错误数据，就是中间多个空格或者换行的那种。所以我的输入部分代码过不了，但是读走前面的空格或者换行的代码就可以。下面注释部分是我的结构体和输入操作，本机检验输入没问题，提交上去就WA。我用的是gyx的结构体和输入部分代码，就过了。算法很简单，暴力就行了。不提了。
但是要注意，这里出现了一个注意点：**能用整数几次方进行比较，就别用开方来做，因为浮点数存在精确度，并且也有格式问题。**
```C++
#include <iostream>
#include <algorithm>
#include <cmath>
#define N 1005		
using namespace std;
/*
typedef struct poin{
	double x;
	double y;
}point;
*/
double rad[N];
double db(double x)	{
	return x*x;
}
struct point{
    double x,y;
    void write(){
        char c;
        while ((c=getchar())==' '||c=='\n');
        scanf("%lf,%lf)",&x,&y);
        getchar();
    }
};
int main()	{
	int n,m;
	while (scanf("%d %d",&n,&m)==2&&n&&m)	{
		int cnt[N];
		point bic[N],peo[N];
		for (int i=0;i<m;i++)	cnt[i]=0;
		getchar();
        for (int i = 0; i < n; i++) {
            bic[i].write();
           // bic[i].show();
        }
        for (int i = 0; i < m; i++) {
            peo[i].write();
            //per[i].show();
        }
		/*
		for (int i=0;i<n;i++)	{
			getchar();
			getchar();
			scanf("%lf,%lf",&bic[i].x,&bic[i].y);
			getchar();			
		}
		for (int i=0;i<m;i++)	{
			getchar();
			getchar();
			scanf("%lf,%lf",&peo[i].x,&peo[i].y);
			getchar();
		}
		*/
		for (int i=0;i<m;i++)	scanf("%lf",&rad[i]);
		for (int i=0;i<m;i++)	{
			for (int j=0;j<n;j++)	{
				double t1=bic[j].x-peo[i].x;
				double xx=db(t1);
				double t2=bic[j].y-peo[i].y;
				double yy=db(t2);
				double distance=(xx+yy);
				if (distance<=rad[i]*rad[i])	cnt[i]++;
			}
		}
		for (int i=0;i<m;i++)	{
			if (i!=m-1)	printf("%d ",cnt[i]);
			else	printf("%d\n",cnt[i]);
		}
	}
	return 0;
}
```




