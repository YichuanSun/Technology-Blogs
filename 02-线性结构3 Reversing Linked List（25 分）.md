# 02-线性结构3 Reversing Linked List（25 分）

标签（空格分隔）： 数据结构 算法竞赛

---

    02-线性结构3 Reversing Linked List（25 分）
    Given a constant K and a singly linked list L, you are supposed to reverse the links of every K elements on L. For example, given L being 1→2→3→4→5→6, if K=3, then you must output 3→2→1→6→5→4; if K=4, you must output 4→3→2→1→5→6.
    
    Input Specification:
    Each input file contains one test case. For each case, the first line contains the address of the first node, a positive N (≤10
    ​5
    ​​ ) which is the total number of nodes, and a positive K (≤N) which is the length of the sublist to be reversed. The address of a node is a 5-digit nonnegative integer, and NULL is represented by -1.
    
    Then N lines follow, each describes a node in the format:
    
    Address Data Next
    where Address is the position of the node, Data is an integer, and Next is the position of the next node.
    
    Output Specification:
    For each case, output the resulting ordered linked list. Each node occupies a line, and is printed in the same format as in the input.
    
    Sample Input:
    
    00100 6 4
    00000 4 99999
    00100 1 12309
    68237 6 -1
    33218 3 00000
    99999 5 68237
    12309 2 33218
    
    Sample Output:
    
    00000 4 33218
    33218 3 12309
    12309 2 00100
    00100 1 99999
    99999 5 68237
    68237 6 -1

哇，今天这道题真是恶心到我了。我最开始用的动态链表写的，但是我发现写到交换时就写不下去了，指针错综复杂，不可能指对，除非用递归，但是难度太大了。看了好几个答案后才知道，用静态链表做才是正确的思路。接下来就比较简单了。

但是要注意，会存在不在链表上的节点。这时候，我们就要找出主链的长度，以它作为for循环交换的界限。考虑到交换后地址的规律性，输出方式就简单多了。最后，别忘了还要将最后一个元素特殊考虑，分开输出。下面是AC的静态链表代码，以及写不下去的动态链表代码。
```C++
//下面是静态链表的正解
#include <iostream>
#include <algorithm>
using namespace std;
struct SL{
    int data;
    int nex;
}SL[100000];

int main()  {
    int address0,data0,k;
    cin >> address0 >> data0 >> k;
    int dat=data0;
    while (dat--) {
        int ad,da,ne;
        cin >> ad >> da >> ne;
        SL[ad].data=da;
        SL[ad].nex=ne;
    }
    int List[100000];
    int p=address0,i=1;
    while (p!=-1) {
        List[i++]=p;
        p=SL[p].nex;
    }
    for (int j=1;j+k<=i;j+=k)
        reverse(List+j,List+j+k);
    int j=1;
    for (j=1;j<i-1;j++)
        printf("%05d %d %05d\n",List[j],SL[List[j]].data,List[j+1]);
    printf("%05d %d -1\n",List[j],SL[List[j]].data);
    return 0;
}

//下面这段代码思路是错的，我想用动态链表做，但是太难了，而且可能直接就写不出来。用静态链表就简单多了
/*
#include <iostream>
#include <algorithm>
using namespace std;
typedef int first;
typedef int next;
typedef struct LL *PtoLL;
typedef struct LL   {
    int des;
    int data;
    int nextdes;
    PtoLL Front;
    PtoLL Behind;
}LL;

int cmp(const void *a,const void *b){
    PtoLL aa=(PtoLL)a;
    PtoLL bb=(PtoLL)b;
    return (aa->data-bb->data);
}

int main()  {
    int des0,data0,nextdes0;
    scanf("%d %d %d",&des0,&data0,&nextdes0);
    PtoLL newL=(PtoLL)malloc(sizeof(LL)),head;
    newL->Front=NULL;
    head=newL;
    while (data0--) {
        int dest,datat,nextdest;
        scanf("%d %d %d",&dest,&datat,&nextdest);
        PtoLL newLt=(PtoLL)malloc(sizeof(LL));
        newLt->des=dest;
        newLt->data=datat;
        newLt->nextdes=nextdest;
        newLt->Front=newL;
        newL->Behind=newLt;
        newL=newL->Behind;
    }
    newL->Behind=NULL;
    newL=head->Behind;
    qsort(head,data0,sizeof(LL),cmp);
    while (newL!=NULL)
    {
        printf("%d %d %d\n",newL->des,newL->data,newL->nextdes);
        newL=newL->Behind;
    }
    return 0;
}
*/
```