## Garbage Collection
### Automatic Memory Management
#### A Simple Example
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544160115(1).png)
#### Mark and Sweep
 A serious problem with the mark phase
– it is invoked when we are out of space
– yet it needs space to construct the todo list
– the size of the todo list is unbounded so we cannot
reserve space for it a priori
#### Stop and Copy
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544162715(1).png)
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544162986(1).png)
#### Conservative Garbage Collection
- If a memory word looks like a pointer it is considered a pointer
• it must be aligned
• it must point to a valid address in the data segment
- All such pointers are followed and we overestimate the reachable objects
### Reference Counting
- Rather that wait for memory to be exhausted, try to collect an object when there are no more pointers to it
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544163512(1).png)
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544163617(1).png)
### Garbage Collection Evaluation
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544163679(1).png)
