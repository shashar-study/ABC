# 从Hotspot 6种GC实现来看GC的发展趋势



![image-20210726201120846](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210726201120846.png)

- 吞吐量和应用效率直接相关
- 。
- 。
- 。



![image-20210726201623536](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210726201623536.png)



![image-20210726203451692](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210726203451692.png)





![image-20210726204204040](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210726204204040.png)

新生代和老生代的边界固定，即，两者的最大内存固定

新老生代的回收都是单线程的，因此停顿时间长

额外内存（overhead）较少

使用广度而不是用深度是因为后者需要额外的内存空间

注意点：1. 标记在哪里？





![image-20210726204737466](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210726204737466.png)



