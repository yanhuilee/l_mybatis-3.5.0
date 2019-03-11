MyBatis SQL Mapper Framework for Java
=====================================

![mybatis](http://mybatis.github.io/images/mybatis-logo.png)

The MyBatis SQL mapper framework makes it easier to use a relational database with object-oriented applications.
MyBatis couples objects with stored procedures or SQL statements using a XML descriptor or annotations.
Simplicity is the biggest advantage of the MyBatis data mapper over object relational mapping tools.

Essentials
----------

* [See the docs](http://mybatis.github.io/mybatis-3)
* [Download Latest](https://github.com/mybatis/mybatis-3/releases)
* [Download Snapshot](https://oss.sonatype.org/content/repositories/snapshots/org/mybatis/mybatis/)

#### parsing 解析器模块
主要提供了两个功能:

- 对 XPath 进行封装，为 MyBatis 初始化时解析 `mybatis-config.xml` 以及映射配置文件提供支持。
- 另一个功能，为处理动态 SQL 语句中的占位符提供支持。

XPathParser: 基于 Java XPath 解析器

#### transaction 事务模块
JdbcTransaction

ManagedTransaction: 由容器管理

```
Transaction newTransaction(DataSource dataSource, TransactionIsolationLevel level, boolean autoCommit);
```

#### cache 缓存模块
> MyBatis 中提供了一级缓存和二级缓存，而这两级缓存都是依赖于基础支持层中的缓存模块实现的。
这里需要读者注意的是，MyBatis 中自带的这两级缓存与 MyBatis 以及整个应用是运行在同一个 JVM 中的，共享同一块堆内存。
如果这两级缓存中的数据量较大， 则可能影响系统中其他功能的运行

> http://svip.iocoder.cn/MyBatis/cache-package/

```
void putObject(Object key, Object value);
```

### MyBatis 初始化
- 1、加载 mybatis-config: 保存到 Configuration 对象
```
SqlSessionFactoryBuilder
    build(Reader reader, String environment, Properties properties)
```

- 2、加载 Mapper 映射配置
```
XMLMapperBuilder
```

- 3、加载 Statement 配置
XMLStatementBuilder

- 4、加载注解配置
```
// Configuration.java
    
public <T> void addMapper(Class<T> type) {
    mapperRegistry.addMapper(type);
}
```

### SQL 初始化（上）
- scripting 脚本模块
动态 SQL

- SqlNode: 每个 XML Node 会解析成对应的 SQL Node 对象

### SQL 执行
> Executor、StatementHandler、ParameterHandler 和 ResultSetHandler

- 1、Executor 缓存，事务管理
- 2、StatementHandler、ParameterHandler: 实参绑定
- 3、ResultSetHandler

### SqlSession
Executor 是不适合直接暴露给用户使用的，而是需要通过 SqlSession