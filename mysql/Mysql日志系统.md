

### 日志系统

##### redo log

- redo log 是InnoDB特有的日志，属于引擎层面的，保存的是对数据页的什么更改

- 为了解决update语句，每次执行的时候先要找到那条记录，然后更新它，整个过程IO成本和查找成本都很高，为了解决这个问题，引入的redo log
- 当有条记录需要更新的时候，InnoDB会先把记录写到redo log中，然后更新内存，这个就给客户端返回完成了。等到系统闲下来的时候，InnoDB引擎会把这些记录更新到磁盘中去。
- InnoDB中red log 是固定的，一组4个文件，每个文件1GB
- red log的格式是这样的，checkpoint向右移动在擦除数据，writepos向右移动在写数据，write pos -> check point之间的位置是redo log 还空的位置
- 当write pos追上check point的时候，说明redo log 文件满了，应该停止更新操作，先来清楚一点日志内容
- 出发redo log 日志文件更新到磁盘的事件是什么？
  - 1. 系统空闲的时候
    2. redo log日志满了的时候（两个指针相遇）
- ![image-20210314012613869](/Users/zhw/Library/Application Support/typora-user-images/image-20210314012613869.png)

##### binlog（归档日志）

- binlog是service层面的日志
- 保存的更像一个SQL语句的逻辑信息





### redo log和binlog的区别

- 1. redolog 是引擎层面的，binlog是server层面的
  2. redolog是物理日志，binlog是逻辑日志
  3. redolog会循环写，会覆盖，空间固定会用完。binlog是追加写，一个文件用完了生成新文件继续追加，不会覆盖



### 两阶段提交

- 为什么需要两阶段提交
  - 为了保证redolog和binlog数据在逻辑上是一致的

