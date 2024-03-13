实现即时通信用的什么协议？http吗？

WebSocket（[深入理解WebSocket协议](#深入理解WebSocket协议)）为每一个客户端长连接开启两个协程，一个接收一个发送。

粘包现象是什么？怎么处理？

接收方怎么知道长度，哪些字段可以知道？

UDP会粘包吗？

UDP传输的最大长度？

进程和线程的区别？

了解select和epoll吗，epoll的底层是什么？

红黑树和B+树有什么区别？B+树的应用场景有什么？

（区间查找）

MySQL的底层实现？

MySQL的索引？

（不是指应用层面的索引）

题目

1. 链表，奇节点全放前面，偶节点全放后面（https://leetcode.cn/problems/odd-even-linked-list/description/）

   双指针

2. 第k大的数，几种做法？时间复杂度呢？（https://leetcode.cn/problems/kth-largest-element-in-an-array/description/）

   1. 排序 O(nlogn + k)
   2. 堆排序（大根堆） O(n + klogn)
   3. 维护大小为k的小根堆，堆顶即为答案 O((n - k)logk)

3. 斐波那契数列？（https://leetcode.cn/problems/fibonacci-number/description/）

   1. 递归
   2. 动态规划

###### 深入理解WebSocket协议

WebSocket：全双工、双向通信。使用自定义协议，客户端和服务端之间发送非常少量数据。

http：请求-相应模式，半双工通信。字节级开销。
