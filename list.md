# 至今所学总结

## 写md本身也是一种能力

## 机器学习平台相关
[parameter_server] 就像它的名字一样，参数服务器。对一个key，存一段值，然后允许更新、访问、扩容、淘汰、快照备份。业务上，会training跟online分离，training侧需要支持更快的插入、更新，online侧因为需要稳定性，频繁的fetch，会考虑多副本/与training的一致性/内存的压缩和节省等等。

[运维/部署/上线/同步系统] 作为一个服务，不应该给ps过多职责和压力，专注于存储服务。相应的，外围需要一整套的运维系统来定时的发起扩容、淘汰、快照备份、上线、同步之类的事情，包括机器的故障报警和自动处理，出现问题的自动归因等等。

## cpp通用知识

### 双buffer
其实就是一个锁在切换的时候锁一下，一个buffer在使用，第二个在更新，到切换阶段锁一下第二个切至使用，第一个切至更新

### lock小技巧
```
  if (need_lock_) {
    lock_.lock();
    if (need_lock_) {
        // do something
    }
  }
```
lock前check一次，lock后再check一次，可以大幅度减少lock次数的同时保证正确性。

### 宏小技巧

将宏内容包裹在do{ ... } while(0)，可以有效隔离逻辑，保证正确性

### hash

murmur64hash，没学原理，但是好用

### hash_set 结构

hash来存储key，然后内存池存储val，这个结构我觉得很不错，包括hopscotch的置换操作
```
// thread safe hopscotch hash set (insert only)
// paper:
//http://people.csail.mit.edu/shanir/publications/disc2008_submission_98.pdf
```

### 流水线+SafeQueue设计

这个不好具体讲。但是大致来说其实就是用函数指针把task包一包，然后做一个任务dag，队列用线程安全的随插随取。

### clang-format

这个不是很了解具体原理和配置方法，但是可以用来规范化代码格式，后面需要时候百度咋配置就完事了

### asm pause

神必的汇编代码插入，参考
```
__asm volatile ("pause" ::: "memory"); 
https://stackoverflow.com/questions/50428450/what-does-asm-volatile-pause-memory-do
```

### 其他的想起来再说吧

