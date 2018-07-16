# Interview-Process

## Personal

#### 1. 乐云付公司

##### 1.1 配置JDK环境的Linux命令

* 下载安装包
  * 移动安装包下载目录命令：cd /opt/setups
  * 下载命令：sudo wget http://211.138.156.198:82/1Q2W3E4R5T6Y7U8I9O0P1Z2X3C4V5B/download.oracle.com/otn-pub/java/jdk/8u72-b15/jdk-8u72-linux-x64.tar.gz

* 默认CentOS安装openJDK，建议应该先卸载掉。
  * 查看JDK命令：java -version
  * 查询本地JDK安装程序情况命令：rpm -qa|grep java
  * 删除OpenJDK命令：sudo rpm -e --nodeps java-1.6.0-openjdk-1.6.0.38-1.13.10.0.el6_7.x86_64

* JDK安装
  * 解压安装包命令：sudo tar -zxvf jdk-8u72-linux-x64.tar.gz
  * 移动压缩包命令：mv jdk1.8.0_72/ /usr/program/
  * 配置环境变量命令：sudo vim /etc/profile
    ```
    # JDK
    JAVA_HOME=/usr/program/jdk1.8.0_72
    JRE_HOME=$JAVA_HOME/jre
    PATH=$PATH:$JAVA_HOME/bin
    CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
    export JAVA_HOME
    export JRE_HOME
    export PATH
    export CLASSPATH
    ```
  * 刷新新配置命令：source /etc/profile
  * 检查是否使用最新JDK：java -version

##### 1.2 查看日志的Linux命令

* [cat xxx.log](https://www.cnblogs.com/zdz8207/p/linux-log-tail-cat-tac.html)
* [日常维护命令](http://zhanjia.iteye.com/blog/1797788)

##### 1.3 删除某个字段具有相同值的记录的SQL语句，保留其中一条。
```
--
USE interview_process;
DROP TABLE IF EXISTS tb_lecloudpay;
CREATE TABLE tb_lecloudpay_1(
	f_id INT PRIMARY KEY AUTO_INCREMENT ,
	f_name VARCHAR(255)
);

--
INSERT INTO tb_lecloudpay_1 (f_name) VALUES('a');
INSERT INTO tb_lecloudpay_1 (f_name) VALUES('b');
INSERT INTO tb_lecloudpay_1 (f_name) VALUES('b');
INSERT INTO tb_lecloudpay_1 (f_name) VALUES('c');
INSERT INTO tb_lecloudpay_1 (f_name) VALUES('c');
INSERT INTO tb_lecloudpay_1 (f_name) VALUES('c');

--
USE interview_process;
CREATE TABLE tb_lecloudpay_1_tmp AS SELECT * FROM tb_lecloudpay_1 WHERE f_name IN (SELECT f_name FROM tb_lecloudpay_1 GROUP BY f_name HAVING COUNT(f_name) > 1)
    AND f_id NOT IN (SELECT MIN(f_id) FROM tb_lecloudpay_1 GROUP BY f_name HAVING COUNT(f_name) > 1);
DELETE FROM tb_lecloudpay_1 WHERE f_id IN (SELECT f_id FROM tb_lecloudpay_1_tmp);

--
SHOW VARIABLES LIKE 'SQL_SAFE_UPDATES';
SET SQL_SAFE_UPDATES = 0;
```

##### 1.4 MySQL的存储引擎有几种？区别是什么？

* InnoDB
* MyISAM
* 区别：
  * 事务：InnoDB支持事务，可以使用Commit和Rollback语句。MyISAM不支持事务。
  * 并发：InnoDB既支持行锁也支持表锁。MyISAM只支持表锁。
  * 外键：InnoDB支持外键。MyISAM不支持外键。
  * 备份：InnoDB支持热备份。MyISAM不支持热备份。
  * 系统奔溃：MyISAM奔溃后发生损坏的概率比InnoDB高得多，而且恢复速度慢。
  * 其他特性：MyISAM支持压缩表和空间数据索引。

##### 1.5 HashMap与LinkedList区别？

* HashMap实现Map接口，底层使用的是Hash算法存储数据。HashMap将数据以键值对的方式存储。
* LinkedList实现List接口，底层使用的是使用双向链表的形式，存储的数据在空间上很可能不相邻，但是他们都有一个引用连在一起，所以增删起来会很方便。

##### 1.6 乐观锁与悲观锁的区别？MySQL怎么实现乐观锁与悲观锁？

* 乐观锁：是指操作数据库时(更新操作)，想法很乐观，认为这次的操作不会导致冲突，在操作数据时，并不进行任何其他的特殊处理（也就是不加锁），而在进行更新后，再去判断是否有冲突。使用检查版本号或时间戳确认是否发生冲突（版本号或时间戳与第一次获取时是否相同），若发生冲突则事务回滚。适用于事务间很少冲突的场景下（事务回滚的成本低于锁机制成本）。
* 悲观锁：悲观锁就是在操作数据时，认为此操作会出现数据冲突，所以在进行每次操作时都要通过获取锁才能进行对相同数据的操作。update,insert,delete语句自动实现锁机制，select语句末尾添加for update则实现锁机制。适用于事务间容易发生冲突的场景下（锁机制成本低于事务回滚成本）。

##### 1.7 SpringMVC与Spring的区别？

* Spring是用于解决企业应用程序开发，替代EJB的一个轻量级的容器框架。
* SpringMVC是一个Web框架，是Spring框架模块的一部分。

##### 1.8 实现线程的几种方式？Callable与Runnable的区别？

* 实现Runnable接口
* 实现Callable接口
* 继承Thread类
* Callable与Runnable的区别：
  * 与Runnable相比，Callable可以有返回值，返回值通过 FutureTask 进行封装，常用于多线程计算结果。
  * Callable接口的call()方法允许抛出异常；而Runnable接口的run()方法的异常只能在内部消化，不能继续上抛。

##### 1.9 synchronized与ReentranLock的区别？

* 锁的实现：synchronized基于JVM实现，JVM原生支持它；ReentranLock基于JDK实现，并不是所有版本的JDK都适用。
* 锁的释放：synchronized在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；而Lock在发生异常时，如果没有主动通过unLock()去释放锁，则很可能造成死锁现象，因此使用Lock时需要在finally块中释放锁。
* 性能：新版本Java对synchronized进行了许多优化如自旋锁，性能上基本与ReentranLock持平，但是synchronized具有更大的优化空间。
* 功能：ReentranLock具有更多高级功能，如等待可中断，可实现公平锁，锁绑定多个条件。

##### 2.0 将读取列的值横向输出的SQL语句，如a,b,c,
```
SELECT GROUP_CONCAT(f_name) FROM tb_lecloudpay_1;
```

##### 2.1 swicth..case..支持的基本数据类型

* byte,char,short,int,enum,String

## Others
