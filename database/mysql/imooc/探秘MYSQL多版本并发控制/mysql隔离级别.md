- 隔离级别和问题

|隔离级别|问题|
|:---:|:---:|
|读未提交(read uncommitted)|脏读、不可重复读、幻读|
|读已提交(read committed)|不可重复读、幻读|
|可重复读(repeatalbe read)|幻读(MVCC、next-key-lock解决幻读)|
|串行化(serializable)||

- 脏读: 当前事务读取到了并行的其他事务的一些数据，而这些事务的数据还没有提交，这些事务可能回滚
- 不可重复读: 同一事务中读取到的两次值不一致 其他并行事务修改后又回滚
- 幻读: 同一事务中读取到的数据行数和条数不一致
- mysql默认隔离级别是可重复读


- 如何查看、设置MySQL事务隔离级别
  - 查看系统隔离级别: `select @@global.tx_isolation`;
  - 查看会话隔离级别(版本5): `select @@tx_isolation`;
  - 查看会话隔离级别(版本8): `select @@transaction_isolation`;
  - 设置会话隔离级别: `set session transaction isolation level (隔离级别)`;
  - 设置全局隔离级别: session改为global


- 如何选择隔离级别
  - 上松下严。上游业务尽量适配数据库的隔离级别，有些公司用的是读已提交
  - spring事务传播属性要了解。当Spring开启了事务并设置了传播机制，那么会覆盖Mysql已有的事务隔离级别, 如果Mysql不支持该隔离级别，Spring的事务就也不会生效
  - RR(可重复读)、RD(读已提交)是常态。经常搭配锁


- 其他知识
  - `select version()` 查看mysql当前版本