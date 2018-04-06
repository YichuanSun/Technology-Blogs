# vector（向量类型）算法竞赛知识点

标签（空格分隔）： C++

---

vector是C++模板库里面的一种数据类型，用起来方便得很。
初始化方式主要有三种
        
        1.初始化少量数据：列表初始化vector<type>a={1,2,3};等价于vector<type>a{1,2,3};
        2.向量复制：vector<type>a=b;等价于vector<type>a(b);
        3.初始化重复数据：vector<type>a(10,1);意为创建一个含有10个整型元素1的向量类型数据。
由上可见，vector可以看做是一种不定长元素集合。
除此之外，我们更常用vector自带函数**push_back(value)**来向vector末尾添加数据。由于vector模板自带很多函数，所以会让解决问题方便很多。

|代码|解释|
| --------   | -----  :|
| v.empty()     | 如果v中不含有任何元素，就返回真，否则返回假 |
| v.size() |   返回v中元素个数   |
| v.push_back(value) | 向v末尾添加元素value |
|v[n]|v上第n个元素的引用|


