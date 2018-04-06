C++栈
---
刚刚cmd markdown炸了，先用markdowneditor编辑。
C++ STL中提供了栈(stack)的模板。

		#include <stack>
		...
		stack<type> stk;
这就定义了一个栈stk。
栈支持下面的基本操作：

		stk.empty():stk为空则返回true，stk中存在元素则返回false
		stk.size():返回stk中的元素个数
		stk.pop():删除栈顶元素，但是不返回其值
		stk.top():返回栈顶元素的值，但不删除它
		stk.push():在栈顶压入一个新元素
看下面这个示例小程序
```C
#include <cstdio>
#include <stack>
#include <string>
#include <iostream>
using namespace std;
stack<char>ss;
int main()
{
	string s;
	getline(cin,s);
	for (unsigned int i=0;i<s.size();i++)
		ss.push(s[i]);
	while (!ss.empty())
	{
		cout << ss.top() << endl;
		ss.pop();
	}
	return 0;
}
```
程序会把输入的一行字符串从下向上倒序输出，每个字符占一行。


C++队列
---
队列(queue)在C++中的定义代码如下

		#include <queue>
		...
		queue<type> q;

其实队列和栈的操作特别像，只不过由于栈是“先进后出”，队列是“先进先出”，所以有相应的不同。队列支持如下操作

		q.empty():如果q为空，返回true;队列中存在元素，返回false
		q.size():返回q中元素个数
		q.pop():删除q首元素，但是不返回其值
		q.front():返回q队首元素的值，但不删除它
		q.push():在q队尾压入新元素
		q.back():返回队尾元素的值，但不删除它

如果在上面栈的示例程序中，将stack变成queue，其他的都不变，输出就会变成从上向下的正序的字符串。