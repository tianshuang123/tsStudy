什么是SPI，他有什么用？

# 1.什么是SPI

SPI全称Service Provider Interface，是Java提供的一套用来被第三方实现或者扩展的接口，它可以用来启用框架扩展和替换组件。 SPI的作用就是为这些被扩展的API寻找服务实现。

# 2.SPI和API的使用场景

 API （Application Programming Interface）在大多数情况下，都是实现方制定接口并完成对接口的实现，调用方仅仅依赖接口调用，且无权选择不同实现。 从使用人员上来说，API 直接被应用开发人员使用。

SPI （Service Provider Interface）是调用方来制定接口规范，提供给外部来实现，调用方在调用时则选择自己需要的外部实现。  从使用人员上来说，SPI 被框架扩展人员使用

# 3.SPI的简单使用

简单来说，我们可以定义一个标准接口，然后第三方的库可以实现这个接口。那么，程序在运行的时候会根据配置信息动态的加载第三方实现的类，从而完成功能的动态扩展，如下图所示

![image-20240716224017397](http://117.72.43.140:9000/weblog/image-20240716224017397.png)

举个例子

第一步，定义一组接口

```java
public interface People {
    String speak();
}
```

实现类A

```java
public class Chinese implements People{
    @Override
    public String speak() {
        return "Chinese speak Chinese!";
    }

    public Chinese(){

    }
}
```

实现类B

```java
public class American implements People {
    @Override
    public String speak() {
        return "American speak English!";
    }

    public American(){

    }
}
```

 然后需要在resources目录下新建META-INF/services目录，并且在这个目录下新建一个与上述接口的全限定名一致的文件，在这个文件中写入接口的实现类的全限定名：

![image-20240716224725822](http://117.72.43.140:9000/weblog/image-20240716224725822.png)

META-INF/services为固定写法。

com.ts.People为接口全路径

里面内容为实现类全路径

![image-20240716224929237](http://117.72.43.140:9000/weblog/image-20240716224929237.png)

测试一下

```java
public class Test {
    public static void main(String[] args) {
        ServiceLoader<People> peoples = ServiceLoader.load(People.class);
        for (People people : peoples) {
            String speak = people.speak();
            System.out.println(speak);
        }
    }
}
```

![image-20240716224956516](http://117.72.43.140:9000/weblog/image-20240716224956516.png)

这样一个简单的spi的demo就完成了。可以看到其中最为核心的就是通过ServiceLoader这个类来加载具体的实现类的。

在Java中，SPI有许多非常典型的实现案例，数据库驱动java.jdbc.Driver；Dubbo定义了ExtensionLoader，实现功能的扩展；Spring提供了SpringFactoriesLoader，实现外部功能的集成。Java SPI机制的主要思想是将装配的控制权迁移到程序之外，做到标准和实现的解耦，以及提供动态可插拔的能力。

# 4.注意事项

实现JavaSPI需要满足几个基本格式：

1.需要定义一个接口，作为扩展的标准。

2.在classpath目录下创建META-INF/service目录。

3.在创建的目录下，以接口的全限定命名配置文件，文件内容是这个接口的实现类。

3.在应用程序里，使用ServiceLoad就可以根据接口名称找到classpath所有的扩展实现，然后根据上下文场景选择实现类完成功能的调用。

JavaSPI有一定的不足之处，比如，不能根据需求加载需要的扩展实现，每次都会加载扩展接口的所有实现类并进行实例化，实例化会造成性能开销，并且加载一些不需要用到的实现类会造成内存资源的浪费。