# POJ 2386 Lake Counting DFS初步

标签（空格分隔）： 算法 算法竞赛

---

        Description
        
        Due to recent rains, water has pooled in various places in Farmer John's field, which is represented by a rectangle of N x M (1 <= N <= 100; 1 <= M <= 100) squares. Each square contains either water ('W') or dry land ('.'). Farmer John would like to figure out how many ponds have formed in his field. A pond is a connected set of squares with water in them, where a square is considered adjacent to all eight of its neighbors. 
        
        Given a diagram of Farmer John's field, determine how many ponds he has.
        Input
        
        * Line 1: Two space-separated integers: N and M 
        
        * Lines 2..N+1: M characters per line representing one row of Farmer John's field. Each character is either 'W' or '.'. The characters do not have spaces between them.
        Output
        
        * Line 1: The number of ponds in Farmer John's field.
        Sample Input
        
        10 12
        W........WW.
        .WWW.....WWW
        ....WW...WW.
        .........WW.
        .........W..
        ..W......W..
        .W.W.....WW.
        W.W.W.....W.
        .W.W......W.
        ..W.......W.
        Sample Output
        
        3
        Hint
        
        OUTPUT DETAILS: 
        
        There are three ponds: one in the upper left, one in the lower left,and one along the right side.

一个入门的深度优先搜索题。在紫书（《算法竞赛入门经典》）和日本书（《挑战程序设计竞赛》）上有两种不同的方法，区别就在，紫书上又设置了一个标记数组，存放水坑（油田）是否已被遍历的标记，而日本书上将所有已经遍历过的水坑（油田）都替换为了土地。两种方法本质上并无不同。

紫书上的类似题目是UVA 572，原理一模一样，只是这道题是找水坑，uva572是找油田。找油田的题我做过一次，但是当时既不关键步骤直接看了提示写出来了，没弄明白为什么那样做，所以现在这道题写的时候一直错，这种大水题，还错了大半个晚上。

觉得这道题是个套路，于是看了提示写出来了，就以为自己会了，但是实际上几个关键步骤仍然搞不懂原因，所以下次还是写不出来，就开始在那里按照记忆瞎蒙。简直是在用屁股写代码，不长脑子**。隔了一天之后，如果自己还能写的出来那些代码的话，我觉得自己才算是真会了，否则当时写的再明白，之后忘了，都是废话。**

