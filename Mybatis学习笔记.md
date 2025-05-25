# Mybatis学习笔记

## 1.Mybatis简介

**什么是 MyBatis？**

MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

## 2.Mybatis快速入门

步骤：

### 1.创建user表，添加数据

```sql
CREATE TABLE tb_user (
    id INT AUTO_INCREMENT PRIMARY KEY COMMENT '主键，自增ID',
    username VARCHAR(50) NOT NULL COMMENT '用户名',
    password VARCHAR(100) NOT NULL COMMENT '密码（加密存储）',
    gender CHAR(1) COMMENT '性别（M表示男，F表示女）',
    addr VARCHAR(255) COMMENT '地址',
    create_time DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    update_time DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间'
) COMMENT='用户信息表';

INSERT INTO tb_user (username, password, gender, addr)
VALUES
('alice', 'alice123', 'F', '北京市海淀区'),
('bob', 'bob456', 'M', '上海市浦东新区'),
('charlie', 'charlie789', 'M', '广州市天河区');

```

### 2.创建模块，导入坐标

```xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.2</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.21</version>
</dependency>
```

### 3.编写Mybatis核心配置文件

src/main/resources/mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <!-- 配置数据库连接信息 -->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://ip:port/tsTest?useSSl=false"/>
                <property name="username" value="root"/>
                <property name="password" value="password"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="UserMapper.xml"/>
    </mappers>
</configuration>
```

### 4.编写SQL映射文件

src/main/resources/UserMapper.xml

```xml
xml<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="test">
    <select id="selectAll" resultType="com.ts.pojo.User">
        select * from tb_user;
    </select>
</mapper>
```

#### 5.编码

##### 1.定义POJO类

```java
@Data
public class User {
    private Integer id;               // 主键
    private String username;          // 用户名
    private String password;          // 密码
    private String gender;            // 性别（M/F）
    private String addr;              // 地址
    private LocalDateTime createTime; // 创建时间
    private LocalDateTime updateTime; // 更新时间

}
```

##### 2.加载核心配置文件，获取SqlSessionFactory对象

```java
//1. 创建 SqlSessionFactory
String resource = "mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```

##### 3.获取SqlSession对象，执行SQL语句

```java
//2. 创建 SqlSession
SqlSession sqlSession = sqlSessionFactory.openSession();
List<User> users = sqlSession.selectList("test.selectAll");
for (User user : users) {
    System.out.println(user);
}
```

##### 4.释放资源

```java
//3. 关闭 SqlSession
sqlSession.close();
```

## 3.Mapper代理开发

