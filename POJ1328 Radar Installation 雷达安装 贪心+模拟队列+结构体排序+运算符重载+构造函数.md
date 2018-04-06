# POJ1328 Radar Installation 雷达安装 贪心+模拟队列+结构体排序+运算符重载+构造函数

标签（空格分隔）： 算法竞赛 C++ 算法

---

这道题用到的是贪心算法，但是牵扯到了很多c++语法知识，所以我花了很长时间去学习相关语法。
题目中用到的语法有（包括但不限于）：结构体的构造函数、运算符重载、结构体的排序函数、队列数据结构的模拟。
一开始我想的很简单，只需要每次考虑所在位置最高（纵坐标最大）的点的坐标，以它在x轴的横坐标点为圆心花一个个圆即可；如果y轴坐标相同，就从x轴坐标小的开始。但是这样做不对，数量会偏大，画个图就可以明白过来（但我当时也没想明白，直到看了别人的答案才搞明白）。后来看到别人的解析，才发现这是一个区间取点问题，这才做出来的。

**具体思路是这样的**：既然我们要找到最少的雷达安装数，也就是找到圆心在x轴，半径固定为常数d的圆的最小数目，那么我们先以每一个小岛为圆心，d为半径做圆，找到每一个圆与x轴相交形成的长度区间，将这些区间构成一个集合。之后，再找出最小数目的点，使这些点恰好被所有的区间包含。找出最少点这一步，我采用了区间重排后取并集的算法。按区间的上界非降排序后，answer+1，从第一个区间a1和第二个区间a2开始取交集。如果有交集，就将交集赋给原先的排头a1，a1出队，即排头后移一位，再对a1和a3取交集，直到没有交集，就跳出循环。answer+1，再从这时候的排头开始这种循环。

题目我就不打了，直接上代码。
```C++
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;
typedef struct _pair    //构造结构体_pair，存储每一个小岛形成的区间
{
    double a;
    double b;
    _pair(double x=0,double y=0) {a=x,b=y;};        //定义构造函数，方便变量初始化结构体
    //bool operator < (const _pair c)const {return b<c.b;}        //在结构体内重载运算符“<”，使得sort函数排序时能按照结构体的元素排序。
}_pair;
_pair que[1000+5];      //存储每一个长度区间的集合
int head,tail;          //顾名思义，分别为队列的头部和尾部下标
bool cmp(_pair a1,_pair a2)     //定义结构体的排序函数。如果line10的运算符重载没有被注释，那么可以去掉下面line54中的cmp，此时这一步就不需要了。重载被注释时，就需要这个比较函数了。若区间为[a,b)的形式，它意为先按b从小到大排列，b相同的区间，按a从大到小排列。这样的话，长度较短、位置靠前的区间就始终在队列的前部。
{
    if (a1.b==a2.b)
    {
        if (a1.a==a2.a)
            return true;
        else
            return a1.a>a2.a;
    }
    else
        return a1.b<a2.b;
}
bool judge(_pair &t,_pair v)        //此处为最重要的区间取交集函数，但是原理不复杂。
{
    if (t.b>=v.a&&t.a<=v.b)
    {
        t.a=max(t.a,v.a);
        t.b=min(t.b,v.b);
        return true;
    }
    return false;
}
//inline 是“内联命令”，相当于对函数的宏定义，下次每次碰到被内联定义的该函数，编译器都会用这里的函数替换它，而不是发生控制转移。
inline void _push(_pair c)  {que[tail]=c;tail++;}       //模拟入队操作
inline void _pop()  {head++;}           //模拟出队操作
int main()
{
    int n,d,t=0,ans=0;
    while (scanf("%d%d",&n,&d)&&(n||d))
    {
        int n0=n,flag=0;
        t++;
        ans=head=tail=0;
        while (n0--)
        {
            int x,y;
            scanf("%d%d",&x,&y);
            if (y>d)    flag=1;
            _push(_pair(x-sqrt((double)d*d-y*y),x+sqrt((double)d*d-y*y)));  //这一步当时我看了很久，才明白这是用构造函数初始化了一个_pair型数据，将它入队。当该循环执行完成后，tail就到了队尾。
        }
        if (flag==1)    {printf("Case %d: -1\n",t);continue;}
        sort(que,que+n,cmp);        
        while (head!=tail)
        {
            ans++;
            _pair t=que[head];
            while (head!=tail)
            {
                _pair v=que[head];
                if (judge(t,v)) _pop();     //显然，此处第一次执行是对同一个区间求交集，但是并不影响最终结果
                else break;
            }
        }
        printf("Case %d: %d\n",t,ans);
    }
    return 0;
}

```
刚开始以为是很简单的一道题，结果一做做了一天。期间也确实学到了很多有用的东西，收获很大。




