突破了硬件限制, 使用户态能访问内核态地址

假设一个数组 a[256], 每个成员有4096字节, 即4KB, 如果比较小的话, 很容易cache命中, 达不到效果

meltdown攻击:

假设内核空间有一个地址k

在用户态:

c = \*k; 由于k是内核态地址, 所以内存管理会拦截, 导致page fault

a[c]   CPU的乱序执行, 所以这一步会提前执行, a[c]已经开始访问第c个成员, 会导致后续cache命中, 后续再访问第c个成员, 速度会快很多

for(...)    遍历, 看那个读取更快, 由于已经命中, 所以读第c个成员会更快, 其他都比较慢, 所以可以推导出k的内容是c

这种旁路攻击即side\-channel, 基于timing时间原理