# MyBatis

MyBatis是一个持久层框架，支持定制化SQL、存储过程以及高级映射。使开发人员无需编写JDBC代码，以及手动设置参数，获取结果集。MyBatis 可以使用简单的 XML 或注解来配置和映射原生类型、接口和 Java 的 POJO为数据库中的记录。

## MyBatis执行SQL的流程

![流程图](execute-sql-flow.png)

1. 客户端根据Mapper Class调用`SqlSession`获取Mapper对象；
   
   1. `SqlSession`根据Mapper Class和自身实例从`Configuration`中获取Mapper对象；
   
   2. `Configuration`调用`MapperRegistry`创建`Mapper`。被创建的Mapper对象实际是`MapperProxy`对象。

2. 客户端获取Mapper并调用Mapper方法，将被`MapperProxy`代理；

3. `MapperProxy`创建`MapperMethod`来执行Mapper方法对应的Sql；

4. `MapperMethod`根据配置的sql类型，来调用`SqlSession`的方法；

5. `SqlSession`调用`Executor`生成动态SQL和查询缓存。如果缓存没有，则调用`StatementHandler`；

6. `StatementHandler`首先调用`ParameterHandler`进行组装参数；接着调用JDBC API；最后调用`ResultSetHandler`处理结果集；最后将执行结果返回。



## 组件

- Configuration：存储所有的MyBatis配置

- SqlSession：MyBatis主要接口。可以用于执行SQL，获取Mapper，管理事务。

- MapperRegistry：用于管理Mapper接口对应代理对象MapperProxy。

- MapperProxy：用于对Mapper接口进行代理。

- MapperMethod：存储Mapper接口的方法与sql的关联

- Executor：

- StatementHandler

- ParameterHandler

- ResultSetHandler
