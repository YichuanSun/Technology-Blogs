# 大理石在哪儿？ Where is the Marble? UVA - 10474 

标签（空格分隔）： 算法竞赛

---

这道题比较简单，所以我详写方法。
如果用以前的C语言方法的话，做的很快，但是用C++但方法来做，我还没学会。用C++牵扯到了排序函数sort、查找函数lower_bound。

        sort(排序下界，排序上界(，排序函数))。上界和下界为地址，排序函数为排序规律。如果不写排序函数，默认升序排列。降序函数书写如下(a在b之前)。意思是b排在a之后。
下面是一段比较函数的内容。**注意，函数中的'<'和'>'表示了升降序；<为升，>为降。**
```C
bool cmp(int a,int b)
{
    return a>b;
}
```
        unique(下界，上界)。作用是去掉序列中相邻的重复数据,执行完毕后，返回最后一个元素的地址值。
unique有一个巧妙的用法，如果执行下面语句。

        int a[5]={1,1,2,2,3};
        int num=unique(a,a+5)-a;
        
输出的num值会是序列中重新排列的不重复数据的长度。但是数组后面的数据看不懂。

        lower_bound(检索下界，检索上界，检索元素x)
函数会返回大于等于x的第一个地址。结合unique的巧妙用法，可知这里也有同样的巧妙用法————减去数组首地址，就会得到检索元素的顺序数。**但是注意，两个巧妙用法，得到的顺序数都是从0开始的**

下面是错误代码
```C++
#include <cstdio>
#include <algorithm>
using namespace std;				//传统方法写的，re了。 
const int N = 1000;
int main()
{
	//freopen("example5-1.txt","wa",stdout);
	int a,b,k=1,ar[N],se;
	while (scanf("%d %d",&a,&b)&&a)
	{
		printf("CASE# %d:\n",k++); 
		for (int i=0;i<a;i++)
			scanf("%d",&ar[i]);
		sort(ar,ar+a);
		while (b--)
		{
			scanf("%d",&se);
			int j=lower_bound(ar,ar+a,se)-ar;
			if (ar[j]==se)
				printf("%d found at %d\n",se,j+1);
			else
				printf("%d not found\n",se);
		}
	}
	return 0;
} 
```
我照着答案检查了好几遍算法，除了变量名不同，就是找不出错误来。但是错误提示总是RE。到最后我才发现，原来是数组开小了，我开了100的，1000的都错，开10000就对了。**这说明，数组开小了，可能会报错RE，而不是因为算法缺陷**