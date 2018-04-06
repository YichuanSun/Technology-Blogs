# UVA 514 Rails 铁轨 栈的模拟
标签（空格分隔）： 算法竞赛 C++ 算法 紫书

---
这道题第一次见是在acm俱乐部寒假训练赛中，但是当时太菜了，即便知道是用栈来做，也想不出怎么模拟。过了一个寒假，学了些C++，感觉能做了，但是找不到用栈模拟火车还是车站，所以还是做不出。直到看了提示，知道是用栈模拟车站后，这才做出来的。

原题不打了，在紫书140页。我只写解题过程。

为了达到重组的要求，首先，我们要用数组记录重组后火车的车厢次序（刚开始做这第一步我就错了，我不知道这里应不应该用栈来做，或者说，我不知道栈应该用在哪里）。之后，要分别比较车厢进站次序j和车厢出站次序train[i]，以及车站中的火车。算法如代码中line20~line30所示。
```C++
#include <iostream>
#include <stack>
#include <algorithm>
#include <cstring>
#define N 1005
using namespace std;
int t[N];
int main()
{
    int n;
    while (scanf("%d",&n)==1&&n)
    {
          begins:int p;
          scanf("%d",&p);
          if (p==0) {printf("\n");continue;}
          else  t[1]=p;
          for (int q=2;q<=n;q++)    scanf("%d",t+q);
          stack<int> st;
          int i=1,j=1,flag=1;
          while (i<=n)
          {
              if (j==t[i])  {i++;j++;}
              else if (!st.empty()&&st.top()==t[i])
              {
                  st.pop();
                  i++;
              }
              else if (j<=n)    {st.push(j);j++;}
              else  {flag=0;break;}
          }
          printf("%s\n",flag?"Yes":"No");
          memset(t,0,sizeof(t));
          goto begins;
    }
    return 0;
}
```
这个模拟过程我想了很久都没想明白，直到用了几个实例，在纸上写写画画才搞明白的。还需要注意的是这道题的输入输出格式比较奇葩，一不小心就PE，逼得我用了goto。




