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

## 3.Mapper代理开发步骤

### 1.创建Mapper接口

定义与SQL映射文件同名的Mapper接口，并且将Mapper接口和SQL映射文件放在同一目录下,路径具体为

```java
src/main/resources/com/ts/mapper/UserMapper.xml
```

### 2.设置SQL映射文件的namespace的属性能Mapper接口全限定名

```xml
<select id="selectAll" resultType="com.ts.pojo.User">
    select * from tb_user;
</select>
```

3.在Mapper接口中定义方法，方法名就是SQL映射文件中sql语句的id，并保持参数类型和返回值类型一致

```java
public interface UserMapper {
    List<User> selectAll();
}
```

### 4.编码

通过SqlSession的getMapper方法获取Mapper接口的代理对象

调用方对应方法完成sql的执行

```java
public class MybatisDemo2 {
    public static void main(String[] args) throws IOException {
        //1. 创建 SqlSessionFactory
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        //2. 创建 SqlSession
        SqlSession sqlSession = sqlSessionFactory.openSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> users = mapper.selectAll();
        for (User user : users) {
            System.out.println(user);
        }
        //3. 关闭 SqlSession
        sqlSession.close();
    }
}
```

### 5.注意

如果Mapper接口名称和SQL映射问价你名称相同，并在同一目录下，则可以使用包扫描的方式简化SQL映射文件的加载

```xml
 <mappers>
     <package name="com.ts.mapper"/>
 </mappers>
```

## 4.配置文件完成增删改查

### 1.创建数据库表tb_brand

```sql
CREATE TABLE tb_brand (
    id INT AUTO_INCREMENT PRIMARY KEY COMMENT '主键，自增ID',
    brand_name VARCHAR(20) NOT NULL COMMENT '品牌名称',
    company_name VARCHAR(20) NOT NULL COMMENT '企业名称',
    ordered int COMMENT '排序字段',
    description VARCHAR(255) COMMENT '描述信息',
    status int COMMENT '状态 0禁用 1启动'
) COMMENT='产品信息表';
INSERT INTO tb_user (brand_name, company_name, ordered, description, status)
VALUES
('三只松鼠', '三只松鼠股份有限公司', 5,'好吃不上火', 0),
('华为', '华为科技', 100,'万物互联', 1),
('小米', '小米科技',50,'ARE you OK', 1);
```

### 2.实体类Brand

```java
@Data
public class Brand {
    private Integer id;          // 主键
    private String brandName;    // 品牌名称
    private String companyName; // 公司名称
    private String description;  // 品牌描述
    private Integer ordered;     // 排序
    private Integer status;      // 状态（0-禁用，1-启用）
}
```

### 3.创建BrandMapper接口

```java
public interface BrandMapper {
    List<Brand> selectAll();
}
```

### 4.创建BrandMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ts.mapper.BrandMapper">
    <resultMap id="brandResultMap" type="com.ts.pojo.Brand">
        <result column="brand_name" property="brandName"/>
        <result column="company_name" property="companyName"/>
    </resultMap>
   <select id="selectAll" resultMap="brandResultMap">
           select * from tb_brand
    </select>
</mapper>
```

### 5.测试用例

```java
public class MybatisTest {
    SqlSession sqlSession = null;
    @Before
    public void before() throws IOException {
        //1. 创建 SqlSessionFactory
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        //2. 创建 SqlSession
        sqlSession = sqlSessionFactory.openSession();
    }
    
    @After
    public void after(){
        //3. 关闭 SqlSession
        sqlSession.close();
    }
    
    @Test
    public void selectAllTest(){
        BrandMapper mapper = sqlSession.getMapper(BrandMapper.class);
        List<Brand> brands = mapper.selectAll();
        for (Brand brand : brands) {
            System.out.println(brand);
        }
    }
}
```

#### 1.查询所有

```java
@Test
public void selectAllTest(){
	BrandMapper mapper = sqlSession.getMapper(BrandMapper.class);
	List<Brand> brands = mapper.selectAll();
	for (Brand brand : brands) {
		System.out.println(brand);
	}
}
```

#### 2.根据条件查询

```java
User selectUserById(int id);

@Test
public void selectById(){
    BrandMapper mapper = sqlSession.getMapper(BrandMapper.class);
    Brand brand = mapper.selectBrandById(1);
    System.out.println(brand);
}
```

```xml
<select id="selectBrandById" parameterType="int" resultMap="brandResultMap">
        select * from tb_brand where id = #{id}
    </select>
```

```
#{}:会将其替换为？，为了防止sql注入
${}:拼接sql
parameterType可以省略
特殊字符处理：
 1.转义字符
 2.<![CDATA[ < ]]>
```

#### 3.多条件查询