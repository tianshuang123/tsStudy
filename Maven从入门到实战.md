# Maven从入门到实战

## 一、Maven简介

### 1.为什么学习maven

#### 1.1Maven是一个依赖管理工具

由于Java的生态非常丰富，无论想实现什么功能，都能找到对应的工具类，这些工具类通常用是以jar包的形式出现的，随着我们项目越来越庞大，需要用到的工具类jar包也就越来越多。项目中一个模块用到上百个jar包是非常正常的。而这些jar包通常是有关联的，即一个jar包通常会依赖另一个jar包才能实现，这样也带来了另一个问题，这么多的jar包，需要去哪里获取，一顿百度、csdn、chatGPT，然后去对应的官网去下载，这就导致我们手动梳理下载jar包，成本大大增高。而使用Maven则几乎不用管理这些，我们只需要告诉Maven需要什么，Maven会自动帮我们下载好jar包以及jar包所依赖的jar包。这就是Maven解决的第一个问题，用来依赖管理。

#### 1.2Maven是一个构建工具

简单的项目，单模块分包处理即可，如果项目比较复杂，要做成多模块项目，例如一个电商项目有订单模块、会员模块、物流模块、商品模块、支付模块等等，一般来说，多模块项目，每一个模块无法独立运行，要多个模块合在一起项目才能运行，这个时候借助Maven工具，可以实现项目的一键打包。

### 2.Maven介绍

官网地址：https://maven.apache.org/

Maven是一款为Java项目管理构建、依赖管理的工具（软件），使用Maven可以自动化构建、测试、打包和发布项目，大大提升了开发效率和质量。

Maven有两大核心：

- 依赖管理：对jar的统一管理（Maven提供了一个Maven的中央仓库，https://mvnrepository.com/，当我们在项目中添加完依赖之后，Maven会自动去中央仓库下载相关的依赖，并且解决依赖的依赖问题）
- 项目构建：对项目进行编译、测试、打包、部署、上传到私服等

### 3.Maven软件工作原理模型图

![image-20231215104649130](http://117.72.43.140:9000/weblog/image-20231215104649130.png)



## 二、Maven安装和配置

### 1.Maven安装

**Maven是Java项目，因此必须安装JDK**

![image-20231215105111818](http://117.72.43.140:9000/weblog/image-20231215105111818.png)

- 下载Maven

下载地址：https://maven.apache.org/download.cgi

![image-20231215105232623](http://117.72.43.140:9000/weblog/image-20231215105232623.png)

右键解压即可（绿色免安装），建议为无空格无中文目录

软件结构：

![image-20231215105518234](http://117.72.43.140:9000/weblog/image-20231215105518234.png)



bin：含有Mavnen的运行脚本

boot：含有plexus-classworlds类加载框架

conf：Maven的核心配置文件

lib：含有Maven运行时所需要的java类库

### 2.Maven环境配置

只需要配置环境变量即可：

- 首先配置MAVEN_HOME

![image-20231215110031611](http://117.72.43.140:9000/weblog/image-20231215110031611.png)

- 然后编辑PATH

![image-20231215131728584](http://117.72.43.140:9000/weblog/image-20231215131728584.png)

- 检验安装

```
mvn --version
```

![image-20231215131857011](http://117.72.43.140:9000/weblog/image-20231215131857011.png)

### 3.Maven功能配置

> 修改maven/conf/settings.xml配置文件，来修改maven的一些默认配置。我们主要需要修改的有两个配置：1.依赖本地缓存位置（本地仓库位置）
>
> 2.maven下载镜像

1.配置本地仓库地址

```xml
  <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->
	<localRepository>e://java/soft/maven3.6/ali_repo</localRepository>
```

2.配置国内阿里镜像

```xml
    <!-- mirror
     | Specifies a repository mirror site to use instead of a given repository. The repository that
     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
     |
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
     -->
	<mirror>
		<id>nexus-aliyun</id>
		<mirrorOf>central</mirrorOf>
		<name>Nexus aliyun</name>
		<url>http://maven.aliyun.com/nexus/content/groups/public</url>
	</mirror>
  </mirrors>		
```

### 4.idea配置本地maven

> 我们需要将配置好的maven软件，配置到idea开发工具中即可！ 注意：idea工具默认自带maven配置软件，但是因为没有修改配置，建议替换成本地配置好的maven！

进入idea后依次点击

file->settings->build ->build tool ->maven

![image-20231215133845071](C:\Users\ts\AppData\Roaming\Typora\typora-user-images\image-20231215133845071.png)



## 三、基于IDEA创建Maven工程

### 1.梳理Maven工程的GAVP属性

> Maven工程相对之前的工程，多出一组gavp属性，gav需要我们在创建项目的时指定，p有默认值，后期通过配置文件修改。既然要填写的属性，我们先行了解下这组属性的含义!

Maven 中的 GAVP 是指 GroupId、ArtifactId、Version、Packaging 等四个属性的缩写，其中前三个是必要的，而 Packaging 属性为可选项。这四个属性主要为每个项目在maven仓库总做一个标识，类似人的《姓-名》。有了具体标识，方便maven软件对项目进行管理和互相引用！

**GAV遵循一下规则：**

  1） **GroupID 格式**：com.{公司/BU }.业务线.[子业务线]，最多 4 级。

    说明：{公司/BU} 例如：alibaba/taobao/tmall/aliexpress 等 BU 一级；子业务线可选。
    
    正例：com.taobao.tddl 或 com.alibaba.sourcing.multilang  com.ts.java

  2） **ArtifactID 格式**：产品线名-模块名。语义不重复不遗漏，先到仓库中心去查证一下。

    正例：tc-client / uic-api / tair-tool / bookstore

  3） **Version版本号格式推荐**：主版本号.次版本号.修订号 1.0.0

    1） 主版本号：当做了不兼容的 API 修改，或者增加了能改变产品方向的新功能。
    
    2） 次版本号：当做了向下兼容的功能性新增（新增类、接口等）。
    
    3） 修订号：修复 bug，没有修改方法签名的功能加强，保持 API 兼容性。
    
    例如： 初始→1.0.0  修改bug → 1.0.1  功能调整 → 1.1.1等

**Packaging定义规则：**

  指示将项目打包为什么类型的文件，idea根据packaging值，识别maven项目类型！

  packaging 属性为 jar（默认值），代表普通的Java工程，打包以后是.jar结尾的文件。

  packaging 属性为 war，代表Java的web工程，打包以后.war结尾的文件。

  packaging 属性为 pom，代表不会打包，用来做继承的父工程。

### 2.idea创建maven工程

File->New->Project

![image-20231215135932781]http://117.72.43.140:9000/weblog/image-20231215135932781.png)

点击Maven->Project SDK->Next

![image-20231215135850754](http://117.72.43.140:9000/weblog/image-20231215135850754.png)

输入项目名及GAV

![image-20231215140725774](http://117.72.43.140:9000/weblog/image-20231215140725774.png)

这样就创建了一个maven工程，观察一下项目结构

![image-20231215140951421](C:\Users\ts\Desktop\image-20231215140951421.png)

### 3.Maven工程项目结构说明

Maven 是一个强大的构建工具，它提供一种标准化的项目结构，可以帮助开发者更容易地管理项目的依赖、构建、测试和发布等任务。以下是 Maven Web 程序的文件结构及每个文件的作用：

```XML
|-- pom.xml                               # Maven 项目管理文件 
|-- src
    |-- main                              # 项目主要代码
    |   |-- java                          # Java 源代码目录
    |   |   `-- com/ts/myapp              # 开发者代码主目录
    |   |       |-- controller            # 存放 Controller 层代码的目录
    |   |       |-- service               # 存放 Service 层代码的目录
    |   |       |-- dao                   # 存放 DAO 层代码的目录
    |   |       `-- model                 # 存放数据模型的目录
    |   |-- resources                     # 资源目录，存放配置文件、静态资源等
    |   |   |-- log4j.properties          # 日志配置文件
    |   |   |-- spring-mybatis.xml        # Spring Mybatis 配置文件
    |   |   `-- static                    # 存放静态资源的目录
    |   |       |-- css                   # 存放 CSS 文件的目录
    |   |       |-- js                    # 存放 JavaScript 文件的目录
    |   |       `-- images                # 存放图片资源的目录
    |   `-- webapp                        # 存放 WEB 相关配置和资源
    |       |-- WEB-INF                   # 存放 WEB 应用配置文件
    |       |   |-- web.xml               # Web 应用的部署描述文件
    |       |   `-- classes               # 存放编译后的 class 文件
    |       `-- index.html                # Web 应用入口页面
    `-- test                              # 项目测试代码
        |-- java                          # 单元测试目录
        `-- resources                     # 测试资源目录
```

- pom.xml：Maven 项目管理文件，用于描述项目的依赖和构建配置等信息。
- src/main/java：存放项目的 Java 源代码。
- src/main/resources：存放项目的资源文件，如配置文件、静态资源等。
- src/main/webapp/WEB-INF：存放 Web 应用的配置文件。
- src/main/webapp/index.html：Web 应用的入口页面。
- src/test/java：存放项目的测试代码。
- src/test/resources：存放测试相关的资源文件，如测试配置文件等。

## 四、基于IDEA进行Maven工程构建

### 1.构建概念和构建过程

项目构建是指将源代码、依赖和资源文件等转换成可执行或者可部署的应用程序的过程，在这个过程中包括编译源代码、链接依赖库、打包和部署等多个步骤。

项目构建是软件开发过程中至关重要的一部分，它能够大大提高软件开发效率，使得开发人员能够更加专注于应用程序的开发和维护，而不用关心应用程序的构建细节。

同时，项目构建还能够将多个开发人员的代码汇在一起，并能够自动化项目的构建和部署，大大降低了项目的出错风险和提高开发效率。常见的构建工具包括Maven、Gradle、Ant等

![image-20231215145054991](C:\Users\ts\AppData\Roaming\Typora\typora-user-images\image-20231215145054991.png)

### 2.命令方式构建项目

| 命令        | 描述                         |
| ----------- | ---------------------------- |
| mvn compile | 编译项目，生成target文件     |
| mvn package | 打包项目，生成tar包或者war包 |
| mvn clean   | 清理编译或打包后的项目结构   |
| mvn install | 打包后上传到maven本地仓库    |
| mvn deploy  | 只打包，上传到maven私服仓库  |
| mvn site    | 生成站点                     |
| mvn test    | 执行测试源码                 |

![image-20231215151752810](http://117.72.43.140:9000/weblog/image-20231215151752810.png)

其余就不在这展示了，更多的时候直接用idea的点点点就可以了。

### 3.可视化方式项目构建

![image-20231215152152174](http://117.72.43.140:9000/weblog/image-20231215152152174.png)

注意：打包（package）和安装（install）的区别是什么

打包是将工程打成jar包或者war包，保存在target目录下

安装是将当前工程所生成的jar或者war文件，安装到本地仓库，会按照坐标保存到指定位置

### 4.构建插、命令、生命周期命令之间的关系

**构建命令周期:**

构建生命周期可以理解成是一组固定构建命令的有序集合，触发周期后的命令，会自动触发周期前的命令！也是一种简化构建的思路!

- 清理周期：主要是对项目编译生成文件进行清理

  包含命令：clean

- 默认周期：定义了真正构件时所需要执行的所有步骤，它是生命周期中最核心的部分

  包含命令：compile - test - package - install / deploy

**周期，命令和插件:**

周期→包含若干命令→包含若干插件!

使用周期命令构建，简化构建过程！

最终进行构建的是插件！

## 五、基于IDEA进行Maven依赖管理

### 1.依赖管理概念

Maven 依赖管理是 Maven 软件中最重要的功能之一。Maven 的依赖管理能够帮助开发人员自动解决软件包依赖问题，使得开发人员能够轻松地将其他开发人员开发的模块或第三方框架集成到自己的应用程序或模块中，避免出现版本冲突和依赖缺失等问题。

我们通过定义 POM 文件，Maven 能够自动解析项目的依赖关系，并通过 Maven **仓库自动**下载和管理依赖，从而避免了手动下载和管理依赖的繁琐工作和可能引发的版本冲突问题。

总之，Maven的依赖管理是Maven软件的一个核心功能之一，使得软件包依赖的管理和使用更加智能方便，简化了开发过程中的工作，并提高了软件质量和可维护性。

重点: 编写pom.xml文件!

### 2.maven项目信息属性配置和解读

```XML
<!-- 模型版本 -->
<modelVersion>4.0.0</modelVersion>
<!-- 公司或者组织的唯一标志，并且配置时生成的路径也是由此生成， 如com.companyname.project-group，maven会将该项目打成的jar包放本地路径：/com/companyname/project-group -->
<groupId>com.ts</groupId>
<!-- 项目的唯一ID，一个groupId下面可能多个项目，就是靠artifactId来区分的 -->
<artifactId>mavenDemo</artifactId>
<!-- 版本号 -->
<version>1.0-SNAPSHOT</version>

<!--打包方式
    默认：jar
    jar指的是普通的java项目打包方式！ 项目打成jar包！
    war指的是web项目打包方式！项目打成war包！
    pom不会讲项目打包！这个项目作为父工程，被其他工程聚合或者继承！后面会讲解两个概念
-->
<packaging>jar/pom/war</packaging>
```

### 3.Maven工程依赖管理配置

**依赖管理和依赖添加**

```XML
<!-- 
   通过编写依赖jar包的gav必要属性，引入第三方依赖！
   scope属性是可选的，可以指定依赖生效范围！
   依赖信息查询方式：
      1. maven仓库信息官网 https://mvnrepository.com/
      2. mavensearch插件搜索
 -->
<dependencies>
    <!-- 引入具体的依赖包 -->
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
        <!--
            生效范围
            - compile ：main目录 test目录  打包打包 [默认]
            - provided：main目录 test目录  Servlet
            - runtime： 打包运行           MySQL
            - test:    test目录           junit
         -->
        <scope>runtime</scope>
    </dependency>

</dependencies>
```

**依赖版本统一管理和维护**

通过<properties>标签将版本提取，用${}引用

```xml
<properties>
    <junit.version>1.2.17</junit.version>
</properties>

<dependencies>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>${junit.version}</version>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

### 4.依赖范围

通过设置坐标的依赖范围（scope），可以设置对应jar包的作用范围：编译环境、测试环境、运行环境

Maven 具有以下 6 中常见的依赖范围，如下表所示。

| 依赖范围 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| compile  | 编译依赖范围，scope 元素的缺省值。使用此依赖范围的 Maven 依赖，对于三种 classpath 均有效，即该 Maven 依赖在上述三种 classpath 均会被引入。例如，log4j 在编译、测试、运行过程都是必须的。 |
| test     | 测试依赖范围。使用此依赖范围的 Maven 依赖，只对测试 classpath 有效。例如，Junit 依赖只有在测试阶段才需要。 |
| provided | 已提供依赖范围。使用此依赖范围的 Maven 依赖，只对编译 classpath 和测试 classpath 有效。例如，servlet-api 依赖对于编译、测试阶段而言是需要的，但是运行阶段，由于外部容器已经提供，故不需要 Maven 重复引入该依赖。 |
| runtime  | 运行时依赖范围。使用此依赖范围的 Maven 依赖，只对测试 classpath、运行 classpath 有效。例如，JDBC 驱动实现依赖，其在编译时只需 JDK 提供的 JDBC 接口即可，只有测试、运行阶段才需要实现了 JDBC 接口的驱动。 |
| system   | 系统依赖范围，其效果与 provided 的依赖范围一致。其用于添加非 Maven 仓库的本地依赖，通过依赖元素 dependency 中的 systemPath 元素指定本地依赖的路径。鉴于使用其会导致项目的可移植性降低，一般不推荐使用。 |
| import   | 导入依赖范围，该依赖范围只能与 dependencyManagement 元素配合使用，其功能是将目标 pom.xml 文件中 dependencyManagement 的配置导入合并到当前 pom.xml 的 dependencyManagement 中。 |

依赖范围与三种 classpath 的关系一览表，如下所示。

| 依赖范围 | 编译 classpath | 测试 classpath | 运行 classpath | 例子                    |
| -------- | -------------- | -------------- | -------------- | ----------------------- |
| compile  | √              | √              | √              | log4j                   |
| test     | -              | √              | -              | junit                   |
| provided | √              | √              | -              | servlet-api             |
| runtime  | -              | -              | √              | JDBC-driver             |
| system   | √              | √              | -              | 非 Maven 仓库的本地依赖 |

### 5.Maven工程依赖下载失败错误解决

在使用 Maven 构建项目时，可能会发生依赖项下载错误的情况，主要原因有以下几种：

1. 下载依赖时出现网络故障或仓库服务器宕机等原因，导致无法连接至 Maven 仓库，从而无法下载依赖。
2. 依赖项的版本号或配置文件中的版本号错误，或者依赖项没有正确定义，导致 Maven 下载的依赖项与实际需要的不一致，从而引发错误。
3. 本地 Maven 仓库或缓存被污染或损坏，导致 Maven 无法正确地使用现有的依赖项，并且也无法重新下载！

解决方案：

1. 检查网络连接和 Maven 仓库服务器状态。
2. 确保依赖项的版本号与项目对应的版本号匹配，并检查 POM 文件中的依赖项是否正确。
3. 清除本地 Maven 仓库缓存（lastUpdated 文件），因为只要存在lastupdated缓存文件，刷新也不会重新下载。本地仓库中，根据依赖的gav属性依次向下查找文件夹，最终删除内部的文件，刷新重新下载即可！

脚本使用：

````shell
@echo off
rem 这里写你的仓库路径
set REPOSITORY_PATH=E:\JAVA\soft\maven3.6\ali_repo
rem 正在搜索...
for /f "delims=" %%i in ('dir /b /s "%REPOSITORY_PATH%\*lastUpdated*"') do (
    del /s /q %%i
)
rem 搜索完毕
pause	
````

```XML
使用记事本打开
set REPOSITORY_PATH=D:\repository  改成你本地仓库地址即可！
点击运行脚本，即可自动清理本地错误缓存文件！！
```

### 6.Maven工程Build构建配置

`build`部分也是 Maven POM 中非常重要的部分。它提供有关默认 Maven*目标*、已编译项目的目录和应用程序的最终名称的信息。默认`build`如下所示：

```xml
<build>
    <!--当项目没有规定目标（Maven2叫做阶段（phase））时的默认值， -->
    <!--必须跟命令行上的参数相同例如jar:jar，或者与某个阶段（phase）相同例如install、compile等 -->
    <defaultGoal>install</defaultGoal>
    <!-- 构建产生的所有文件存放的目录,默认为${basedir}/target，即项目根目录下的target -->
    <directory>${basedir}/target</directory>
    <!-- 产生的构件的文件名，默认值是${artifactId}-${version}-->
    <finalName>${artifactId}-${version}</finalName>
    <!--当filtering开关打开时，使用到的过滤器属性文件列表。 -->
    <!--项目配置信息中诸如${spring.version}之类的占位符会被属性文件中的实际值替换掉 -->
    <filters>
      <filter>filters/filter1.properties</filter>
    </filters>
     <resources>
    <!--设置要打包的资源位置 -->
         <resource>
     <!--资源所在目录 -->         
             <directory>src/main/java</directory>
      <!--设置包含的资源类型 -->
             <includes>
                 <include>**/*.xml</include>
             </includes>
         </resource>
    </resources>
```

**配置依赖插件**

````xml
<build>
    <!--设置打包插件，常用的jdk版本、tomcat插件、mybatis分页插件、mybatis逆向工程插件 -->
    <plugins>
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.1</version>
        <configuration>
            <port>80</port>
            <path>/</path>
      </configuration>
      </plugin>
    </plugins>
</build>
````

## 六、Maven依赖传递特性和依赖冲突

### 1.Maven依赖性传递

**概念**

Maven 的依赖传递机制是指：不管 Maven 项目存在多少间接依赖，POM 中都只需要定义其直接依赖，不必定义任何间接依赖，Maven 会动读取当前项目各个直接依赖的 POM，将那些必要的间接依赖以传递性依赖的形式引入到当前项目中。Maven 的依赖传递机制能够帮助用户一定程度上简化 POM 的配置。

**作用**

- 减少重复依赖：当多个项目依赖同一个库时，Maven 可以自动下载并且只下载一次该库。这样可以减少项目的构建时间和磁盘空间。
- 自动管理依赖: Maven 可以自动管理依赖项，使用依赖传递，简化了依赖项的管理，使项目构建更加可靠和一致。
- 确保依赖版本正确性：通过依赖传递的依赖，之间都不会存在版本兼容性问题，确实依赖的版本正确性！

**传递的原则**

在A依赖B，B依赖C的情况下，C是否能够传递到A，取决于B依赖于C时使用的依赖范围以及配置

- B依赖C时使用compile范围，可以传递
- B依赖C时使用test或provided范围：不能传递，需要在需要的地方明确配置才可以
- B配置C时，若配置以下标签，则不能传递

```xml
<!-- 免写冗余的 Java 样板式代码 -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>
```

**依赖传递终止**

- 非compile范围进行依赖传递
- 使用optionl配置终止传递
- 依赖冲突（传递的依赖已经存在)

**依赖冲突演示：**

当直接引用或者间接引用出现了相同的jar包! 这时呢，一个项目就会出现相同的重复jar包，这就算作冲突！依赖冲突避免出现重复依赖，并且终止依赖传递！

![image-20231215165428595](C:\Users\ts\AppData\Roaming\Typora\typora-user-images\image-20231215165428595.png)

maven自动解决依赖冲突问题能力，会按照自己的原则，进行重复依赖选择。同时也提供了手动解决的冲突的方式，不过不推荐！

解决依赖冲突（如何选择重复依赖）方式：

  1. 自动选择原则

     - 短路优先原则（第一原则）

       A—>B—>C—>D—>E—>X(version 0.0.1)

       A—>F—>X(version 0.0.2)

       则A依赖于X(version 0.0.2)。

     - 依赖路径长度相同情况下，则“先声明优先”（第二原则）

       A—>E—>X(version 0.0.1)

       A—>F—>X(version 0.0.2)

       在<depencies></depencies>中，先声明的，路径相同，会优先选择！

     2.手动排除

     ````xml
      <exclusions>
      	<exclusion>
              <groupId></groupId>
              <artifactId></artifactId>
     	 </exclusion>
      </exclusions>
     ````

## 七、Maven继承和聚合特性

### 1.Maven工程继承关系

1.继承概念

Maven 继承是指在 Maven 的项目中，让一个项目从另一个项目中继承配置信息的机制。继承可以让我们在多个项目中共享同一配置信息，简化项目的管理和维护工作。

![image-20231215170557861](C:\Users\ts\AppData\Roaming\Typora\typora-user-images\image-20231215170557861.png)

2.继承作用

作用：在父工程中统一管理项目中的依赖信息,进行统一版本管理!

它的背景是：

- 对一个比较大型的项目进行了模块拆分。
- 一个 project 下面，创建了很多个 module。
- 每一个 module 都需要配置自己的依赖信息。

它背后的需求是：

- 多个模块要使用同一个框架，它们应该是同一个版本，所以整个项目中使用的框架版本需要统一管理。
- 使用框架时所需要的 jar 包组合（或者说依赖信息组合）需要经过长期摸索和反复调试，最终确定一个可用组合。这个耗费很大精力总结出来的方案不应该在新的项目中重新摸索。

通过在父工程中为整个项目维护依赖信息的组合既保证了整个项目使用规范、准确的 jar 包；又能够将以往的经验沉淀下来，节约时间和精力。

继承语法

- 父工程

```xml
<groupId>com.ts</groupId>
<artifactId>mavenDemo</artifactId>
<version>1.0-SNAPSHOT</version>
<!-- 当前工程作为父工程，它要去管理子工程，所以打包方式必须是 pom -->
<packaging>pom</packaging>
```

- 子工程

````xml
<!-- 使用parent标签指定当前工程的父工程 -->
<parent>
  <!-- 父工程的坐标 -->
    <artifactId>mavenDemo</artifactId>
    <groupId>com.ts</groupId>
    <version>1.0-SNAPSHOT</version>
</parent>

<!-- 子工程的坐标 -->
<!-- 如果子工程坐标中的groupId和version与父工程一致，那么可以省略 -->
<!-- <groupId>com.atguigu.maven</groupId> -->
<artifactId>mavenA</artifactId>
<!-- <version>1.0-SNAPSHOT</version> -->
````

1. 父工程依赖统一管理

- 父工程声明版本

```xml
<!-- 使用dependencyManagement标签配置对依赖的管理 -->
<!-- 被管理的依赖并没有真正被引入到工程 -->
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>4.0.0.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId>
      <version>4.0.0.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>4.0.0.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-expression</artifactId>
      <version>4.0.0.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aop</artifactId>
      <version>4.0.0.RELEASE</version>
    </dependency>
  </dependencies>
</dependencyManagement>
```

- 子工程引用版本

```XML
<!-- 子工程引用父工程中的依赖信息时，可以把版本号去掉。  -->
<!-- 把版本号去掉就表示子工程中这个依赖的版本由父工程决定。 -->
<!-- 具体来说是由父工程的dependencyManagement来决定。 -->
<dependencies>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-expression</artifactId>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
  </dependency>
</dependencies>
```

### 2.Maven工程聚合关系

1.聚合概念

Maven 聚合是指将多个项目组织到一个父级项目中，通过触发父工程的构建,统一按顺序触发子工程构建的过程!!

2.聚合作用

1. 统一管理子项目构建：通过聚合，可以将多个子项目组织在一起，方便管理和维护。
2. 优化构建顺序：通过聚合，可以对多个项目进行顺序控制，避免出现构建依赖混乱导致构建失败的情况。

3.聚合语法

父项目中包含的子项目列表。

```XML
<project>
 <!-- 子模块管理 -->
    <modules>
        <!-- 入口模块 -->
        <module>weblog-web</module>
        <!-- 管理后台 -->
        <module>weblog-module-admin</module>
        <!-- 通用模块 -->
        <module>weblog-module-common</module>
        <module>weblog-module-jwt</module>
    </modules>
</project>
```

4.聚合演示

通过触发父工程构建命令、引发所有子模块构建！产生反应堆！

![image-20231215172021128](http://117.72.43.140:9000/weblog/image-20231215172021128.png)



## 八、Maven私服

### 1.Maven私服简介

①Maven简介

Maven私服是一种特殊的Maven的仓库，他是架设在局域网内的仓库服务，用来代理位于外部的远程仓库（中央仓库、其他远程公共仓库）

> 当然也并不是说私服就只能建立在局域网，也有很多公司直接把私服部署到公网，具体还是得看送死业务的性质是否保密等等，因为局域网的话只能在公司用，部署到公网的话员工在家里也可以办公使用。

建立了Maven私服后，当局域网内的用户需要某个构件时，会按照如顺序就行请求和下载。

1.请求本地仓库，若本地仓库不存在所需构件，则跳转到第2步；

2.请求Maven私服，将所需构件下载到本地仓库，若私服中不存在所需构件，则跳转到第3步。

3.请求外部的远程仓库，将所需构件下载并缓存在Maven私服，若外部远程仓库不存在所需构件，则Maven直接报错。

此外，一些无法从外部仓库下载到的构件，也能从本地上传到私服供其他人使用。

![image-20231217003613145](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20231217003613145.png)



②Maven私服的优势

1.节省外网宽带

​    消除对外部远程仓库大量的重复请求（会消耗很大量的宽带），降低外网的宽带压力。

2.下载速度更快

​    Maven私服位于局域网内，从私服下载构件更快更稳定。

3.便于部署第三方构件

​	有些构件无法从任何一个远程仓库中获得（如：公司或者组织内部的私有构件、Oracle的JDBC驱动等），建立私服之后，就可以将这些构件部署到私服中，供内部Maven项目使用。

4.提高项目的稳定性，增强对项目的控制

​	如果不建立私服，那么Maven项目的构件就高度依赖外部的远程仓库，若外部的网络不稳定，则项目的构件过程也会变得不稳定。建立私服后，即使外部网络不佳甚至中断，只要私服中缓存了所需要的构件，Maven也能正常的运行。私服软件（Nexus）提供了很多控制功能（如：权限管理、RELEASE/SNAPSHOT版本控制等），可以对仓库进行一些更加高级的控制

5.降低中央仓库的负荷能力

​	由于私服会缓存中央仓库的构件，避免了很多对中央仓库的重复下载，降低了中央仓库的负荷

③常见的Maven私服产品

1. Apache的Archiva
2. JFrog的Artifactory
3. Sonatype的Nexus

### 2.Nexus下载安装

下载地址：https://help.sonatype.com/repomanager3/product-information/download

![image-20231217203434738](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20231217203434738.png)

解压，以管理员身份打开CMD，进入bin目录下，执行./nexus /run 命令启动

![image-20231217203752002](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20231217203752002.png)

访问首页：http://localhost:8081/，8081为默认端口号

![image-20231217203856557](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20231217203856557.png)

### 3.初始设置

![image-20231217204048346](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20231217204048346.png)



默认用户名为admin

密码会在F:\javaSoft\nexus-3.63.0-01-win64\sonatype-work\nexus3的admin.password文件中

进入之后会强制修改密码

![image-20231217204548735](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20231217204548735.png)

选择禁止匿名访问

### 4.Nexus上的各种仓库

![image-20231217205856089](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20231217205856089.png)

| 仓库类型 | 说明                                       |
| -------- | ------------------------------------------ |
| proxy    | 某个远程仓库的代理                         |
| group    | 存放：通过Nexus回去的第三方jar包           |
| hosted   | 存放：本团队其他开发人员部署到Nexus的jar包 |

| 仓库名称        | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| maven-central   | Nexus对Maven中央仓库的代理                                   |
| maven-public    | Nexus默认创建，供开发人员下载使用的组仓库                    |
| maven-releases  | Nexus默认创建，供开发人员部署自己的jar包的宿主仓库，要求releases版本 |
| maven-snapshots | Nexus默认创建，供开发人员部署自己的jar包的宿主仓库，要求snapshots版本 |

初始状态下，这几个仓库都没有内容：

![image-20231217211309680](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20231217211309680.png)

### 5.通过Nexus下载jar包

修改本地maven的核心配置文件的settings.xml，设置新的本地仓库地址

```xml
<localRepository>D://ali_repo</localRepository>
```

把原来的配置阿里云仓库地址的mirror标签改成下面这样：

```
<mirror>
    <id>nexus-mine</id>
    <mirrorOf>central</mirrorOf>
    <name>Nexus mine</name>
    <url>http://localhost:8081/repository/maven-central/</url>
</mirror>
```

这里的url是点击这里的copy

![image-20231217212152938](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20231217212152938.png)

把上图中看到的地址复制出来即可。如果我们在前面允许了匿名访问，到这里就够了。但是如果我们禁止了匿名访问，那么接下来我们还需要继续配置settings.xml：

```xml
<server>
    <id>nexus-mine</id>
    <username>admin</username>
    <password>admin</password>
</server>
```

这里需要格外注意：server标签内的id标签值必须和mirror标签中的id值一样。

找到一个用到框架的Maven的工程，执行命令：

````shell
mvn clean compile
````

下载过程日志：

> Downloading from nexus-mine: http://localhost:8081/repository/maven-public/com/google/guava/guava-parent/10.0.1/guava-parent-10.0.1.pom
> Downloaded from nexus-mine: http://localhost:8081/repository/maven-public/com/google/guava/guava-parent/10.0.1/guava-parent-10.0.1.pom (2.0 kB at 3.3 kB/s)
> Downloading from nexus-mine: http://localhost:8081/repository/maven-public/com/google/code/findbugs/jsr305/1.3.9/jsr305-1.3.9.pom
> Downloaded from nexus-mine: http://localhost:8081/repository/maven-public/com/google/code/findbugs/jsr305/1.3.9/jsr305-1.3.9.pom (965 B at 1.5 kB/s)
> Downloading from nexus-mine: http://localhost:8081/repository/maven-public/org/sonatype/sisu/sisu-guice/3.1.0/sisu-guice-3.1.0.pom
> Downloaded from nexus-mine: http://localhost:8081/repository/maven-public/org/sonatype/sisu/sisu-guice/3.1.0/sisu-guice-3.1.0.pom (10 kB at 15 kB/s)
> Downloading from nexus-mine: http://localhost:8081/repository/maven-public/org/sonatype/sisu/inject/guice-parent/3.1.0/guice-parent-3.1.0.pom
> Downloaded from nexus-mine: http://localhost:8081/repository/maven-public/org/sonatype/sisu/inject/guice-parent/3.1.0/guice-parent-3.1.0.pom (11 kB at 8.8 kB/s)
> Downloading from nexus-mine: http://localhost:8081/repository/maven-public/aopalliance/aopalliance/1.0/aopalliance-1.0.pom
> Downloaded from nexus-mine: http://localhost:8081/repository/maven-public/aopalliance/aopalliance/1.0/aopalliance-1.0.pom (363 B at 578 B/s)
> Downloading from nexus-mine: http://localhost:8081/repository/maven-public/org/eclipse/sisu/org.eclipse.sisu.inject/0.0.0.M2a/org.eclipse.sisu.inject-0.0.0.M2a.pom
> Downloaded from nexus-mine: http://localhost:8081/repository/maven-public/org/eclipse/sisu/org.eclipse.sisu.inject/0.0.0.M2a/org.eclipse.sisu.inject-0.0.0.M2a.pom (5.0 kB at 5.0 kB/s)
> Downloading from nexus-mine: http://localhost:8081/repository/maven-public/org/eclipse/sisu/sisu-inject/0.0.0.M2a/sisu-inject-0.0.0.M2a.pom
> Downloaded from nexus-mine: http://localhost:8081/repository/maven-public/org/eclipse/sisu/sisu-inject/0.0.0.M2a/sisu-inject-0.0.0.M2a.pom (7.8 kB at 11 kB/s)
> Downloading from nexus-mine: http://localhost:8081/repository/maven-public/asm/asm/3.3.1/asm-3.3.1.pom
> Downloaded from nexus-mine: http://localhost:8081/repository/maven-public/asm/asm/3.3.1/asm-3.3.1.pom (266 B at 409 B/s)
> Downloading from nexus-mine: http://localhost:8081/repository/maven-public/asm/asm-parent/3.3.1/asm-parent-3.3.1.pom
> Downloaded from nexus-mine: http://localhost:8081/repository/maven-public/asm/asm-parent/3.3.1/asm-parent-3.3.1.pom (4.3 kB at 3.5 kB/s)
> Downloading from nexus-mine: http://localhost:8081/repository/maven-public/org/codehaus/plexus/plexus-component-annotations/2.0.0/plexus-component-annotations-2.0.0.pom
> Downloaded from nexus-mine: http://localhost:8081/repository/maven-public/org/codehaus/plexus/plexus-component-annotations/2.0.0/plexus-component-annotations-2.0.0.pom (750 B at 753 B/s)
> Downloading from nexus-mine: http://localhost:8081/repository/maven-public/org/codehaus/plexus/plexus-containers/2.0.0/plexus-containers-2.0.0.pom

下载后，Nexus服务器上就有了jar包：

![image-20231217213022319](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20231217213022319.png)



若下载速度太慢，可以设置私服中中央仓库的地址为阿里云仓库地址

![image-20231217213204447](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20231217213204447.png)

修改为：http://maven.aliyun.com/nexus/content/groups/public/

![image-20231217213426308](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20231217213426308.png)

### 6.将jar包部署到Nexus

Maven工程中配置：

```
<distributionManagement>
    <snapshotRepository>
        <id>nexus-mine</id>
        <name>Nexus Snapshot</name>
        <url>http://localhost:8081/repository/maven-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```

注意：这里的snapshotRepository的id标签必须和settings.xml中指定mirror标签中的id属性一致。执行部署命令：

```shell
mvn deploy
```

> ```
> Uploading to nexus-mine: http://localhost:8081/repository/maven-snapshots/com/ts/weblog-web/0.0.1-SNAPSHOT/weblog-web-0.0.1-20231217.135758-1.jar
> Uploaded to nexus-mine: http://localhost:8081/repository/maven-snapshots/com/ts/weblog-web/0.0.1-SNAPSHOT/weblog-web-0.0.1-20231217.135758-1.jar (49 MB at 47 MB/s)
> Uploading to nexus-mine: http://localhost:8081/repository/maven-snapshots/com/ts/weblog-web/0.0.1-SNAPSHOT/weblog-web-0.0.1-20231217.135758-1.pom
> Uploaded to nexus-mine: http://localhost:8081/repository/maven-snapshots/com/ts/weblog-web/0.0.1-SNAPSHOT/weblog-web-0.0.1-20231217.135758-1.pom (3.2 kB at 124 kB/s)
> Downloading from nexus-mine: http://localhost:8081/repository/maven-snapshots/com/ts/weblog-web/maven-metadata.xml
> Uploading to nexus-mine: http://localhost:8081/repository/maven-snapshots/com/ts/weblog-web/0.0.1-SNAPSHOT/maven-metadata.xml
> Uploaded to nexus-mine: http://localhost:8081/repository/maven-snapshots/com/ts/weblog-web/0.0.1-SNAPSHOT/maven-metadata.xml (766 B at 29 kB/s)
> Uploading to nexus-mine: http://localhost:8081/repository/maven-snapshots/com/ts/weblog-web/maven-metadata.xml
> Uploaded to nexus-mine: http://localhost:8081/repository/maven-snapshots/com/ts/weblog-web/maven-metadata.xml (276 B at 12 kB/s)
> [INFO] ------------------------------------------------------------------------
> [INFO] Reactor Summary for weblog-springboot 0.0.1-SNAPSHOT:
> [INFO] 
> [INFO] weblog-springboot .................................. SUCCESS [  0.628 s]
> [INFO] weblog-module-common ............................... SUCCESS [  0.765 s]
> [INFO] weblog-module-jwt .................................. SUCCESS [  0.198 s]
> [INFO] weblog-module-admin ................................ SUCCESS [  0.266 s]
> [INFO] weblog-web ......................................... SUCCESS [  1.353 s]
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD SUCCESS
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time:  3.436 s
> [INFO] Finished at: 2023-12-17T21:57:59+08:00
> [INFO] ------------------------------------------------------------------------
> 
> Process finished with exit code 0
> 
> ```

![image-20231217220154367](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20231217220154367.png)

### 7.引用别人部署的jar包

```
<repositories>
    <repository>
        <id>nexus-mine</id>
        <name>nexus-mine</name>
        <url>http://localhost:8081/repository/maven-snapshots/</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
        <releases>
            <enabled>true</enabled>
        </releases>
    </repository>
</repositories>
```

