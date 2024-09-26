# SpringCloud微服务第三季

[TOC]

## 1、从Boot和Cloud版本选型开始说起

### 1.1 详细推演版

#### （1）Springboot版本选择

+ git源码地址

  ```htaccess
  https://github.com/spring-projects/spring-boot/releases/
  ```

+ 官网版本

![image-20240926104836072](images\image-20240926104836072.png)

#### （2）Springcloud版本选择

+ git源码地址

```htaccess
https://github.com/spring-cloud
```

+ 官网版本

![image-20240926105130148](images\image-20240926105130148.png)

+ 官网看Cloud版本

  + Cloud命名规则

    + ```xaml
      SpringCloud的版本关系
      
      Spring Cloud 采用了英国伦敦地铁站的名称来命名，并由地铁站名称字母A-Z依次类推的形式来发布迭代版本
      SpringCloud是一个由许多子项目组成的综合项目，各子项目有不同的发布节奏。为了管理SpringCloud与各子项目的版本依赖关系，发布了一个清单，其中包括了某个SpringCloud版本对应的子项目版本。为了避免SpringCloud版本号与子项目版本号混淆，SpringCloud版本采用了名称而非版本号的命名，这些版本的名字采用了伦敦地铁站的名字，根据字母表的顺序来对应版本时间顺序。例如Angel是第一个版本, Brixton是第二个版本。
      当SpringCloud的发布内容积累到临界点或者一个重大BUG被解决后，会发布一个"service releases"版本，简称SRX版本，比如Greenwich.SR2就是SpringCloud发布的Greenwich版本的第2个SRX版本。
      ```

    + ```htaccess
      https://spring.io/projects/spring-cloud#overview
      ```

## 2、微服务架构编码Base工程模块构建

### （1）订单→支付，业务需求说明

```xml
约定 > 配置 > 编码
```

![image-20240926105807749](images\image-20240926105740023.png)

### （2）IDEA新建Project和Maven父工程

#### <1> 微服务cloud整体聚合Maven父工程Project

##### 1、Maven父工程步骤

###### （1）新建父工程

<img src="images\image-20240926111759589.png" alt="image-20240926111759589" style="zoom:50%;" />

###### （2）聚合总父工程名字

<img src="images\image-20240926111936361.png" alt="image-20240926111936361" style="zoom:50%;" />

###### （3）字符编码

<img src="images\image-20240926112011098.png" alt="image-20240926112011098" style="zoom:50%;" />

###### （4）注解生效激活

<img src="images\image-20240926112053087.png" alt="image-20240926112053087" style="zoom:50%;" />

###### （5）java编译版本选17

<img src="images\image-20240926112207398.png" alt="image-20240926112207398" style="zoom:50%;" />

###### （6）File Type过滤

<img src="images\image-20240926112344054.png" alt="image-20240926112344054" style="zoom:50%;" />

#### <2> 父工程POM文件内容

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.admin.wjk</groupId>
    <artifactId>SpringCloud</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <hutool.version>5.8.22</hutool.version>
        <lombok.version>1.18.26</lombok.version>
        <druid.version>1.1.20</druid.version>
        <mybatis.springboot.version>3.0.2</mybatis.springboot.version>
        <mysql.version>8.0.11</mysql.version>
        <swagger3.version>2.2.0</swagger3.version>
        <mapper.version>4.2.3</mapper.version>
        <fastjson2.version>2.0.40</fastjson2.version>
        <persistence-api.version>1.0.2</persistence-api.version>
        <spring.boot.test.version>3.1.5</spring.boot.test.version>
        <spring.boot.version>3.2.0</spring.boot.version>
        <spring.cloud.version>2023.0.0</spring.cloud.version>
        <spring.cloud.alibaba.version>2022.0.0.0-RC2</spring.cloud.alibaba.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <!--springboot 3.2.0-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-parent</artifactId>
                <version>${spring.boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--springcloud 2023.0.0-->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring.cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--springcloud alibaba 2022.0.0.0-RC2-->
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>${spring.cloud.alibaba.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--SpringBoot集成mybatis-->
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>${mybatis.springboot.version}</version>
            </dependency>
            <!--Mysql数据库驱动8 -->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
            </dependency>
            <!--SpringBoot集成druid连接池-->
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid-spring-boot-starter</artifactId>
                <version>${druid.version}</version>
            </dependency>
            <!--通用Mapper4之tk.mybatis-->
            <dependency>
                <groupId>tk.mybatis</groupId>
                <artifactId>mapper</artifactId>
                <version>${mapper.version}</version>
            </dependency>
            <!--persistence-->
            <dependency>
                <groupId>javax.persistence</groupId>
                <artifactId>persistence-api</artifactId>
                <version>${persistence-api.version}</version>
            </dependency>
            <!-- fastjson2 -->
            <dependency>
                <groupId>com.alibaba.fastjson2</groupId>
                <artifactId>fastjson2</artifactId>
                <version>${fastjson2.version}</version>
            </dependency>
            <!-- swagger3 调用方式 http://你的主机IP地址:5555/swagger-ui/index.html -->
            <dependency>
                <groupId>org.springdoc</groupId>
                <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
                <version>${swagger3.version}</version>
            </dependency>
            <!--hutool-->
            <dependency>
                <groupId>cn.hutool</groupId>
                <artifactId>hutool-all</artifactId>
                <version>${hutool.version}</version>
            </dependency>
            <!--lombok-->
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>${lombok.version}</version>
                <optional>true</optional>
            </dependency>
            <!-- spring-boot-starter-test -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-test</artifactId>
                <version>${spring.boot.test.version}</version>
                <scope>test</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

</project>
```

#### <3> Maven工程落地细节复习

> Maven中的DependencyManagement和Dependencies

```html
dependencyManagement
Maven 使用dependencyManagement 元素来提供了一种管理依赖版本号的方式。
通常会在一个组织或者项目的最顶层的父POM 中看到dependencyManagement 元素。
使用pom.xml 中的dependencyManagement 元素能让所有在子项目中引用一个依赖而不用显式的列出版本号。
Maven会沿着父子层次向上走，直到找到一个拥有dependencyManagement元素的项目，然后它就会使用这个dependencyManagement元素中指定的版本号。
```

<img src="images\image-20240926112826670.png" alt="image-20240926112826670" style="zoom: 50%;" />

+ 这样做的好处就是：如果有多个子项目都引用同一样依赖，则可以避免在每个使用的子项目里都声明一个版本号，优势：

+ 这样当想升级或切换到另一个版本时，只需要在顶层父容器里更新，而不需要一个一个子项目的修改 ；
+ 另外如果某个子项目需要另外的一个版本，只需要声明version就可。
+ dependencyManagement里只是声明依赖，并不实现引入，因此子项目需要显示的声明需要用的依赖。
+ 如果不在子项目中声明依赖，是不会从父项目中继承下来的，只有在子项目中写了该依赖项并且没有指定具体版本，才会从父项目中继承该项,且version和scope都读取自父pom;
+ 如果子项目中指定了版本号，那么会使用子项目中指定的jar版本

> maven中跳过单元测试

+ 1 配置

```xml
<build><!-- maven中跳过单元测试 -->
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <configuration>
                <skip>true</skip>
            </configuration>
        </plugin>
    </plugins>
</build>
```

+ 2 IDEA工具支持(推荐)

<img src="images\image-20240926113232280.png" alt="image-20240926113232280" style="zoom:50%;" />

#### <4> mysql驱动说明

> Mysql5

```yaml
# mysql5.7---JDBC四件套
jdbc.driverClass = com.mysql.jdbc.Driver
jdbc.url= jdbc:mysql://localhost:3306/db2024?useUnicode=true&characterEncoding=UTF-8&useSSL=false
jdbc.user = root
jdbc.password =123456

# Maven的POM文件处理
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
</dependency>
```



> Mysql8

```yaml
# mysql8.0---JDBC四件套
jdbc.driverClass = com.mysql.cj.jdbc.Driver
jdbc.url= jdbc:mysql://localhost:3306/db2024?characterEncoding=utf8&useSSL=false&serverTimezone=GMT%2B8&rewriteBatchedStatements=true&allowPublicKeyRetrieval=true
jdbc.user = root
jdbc.password =123456

# Maven的POM
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.11</version>
</dependency>
```

### （3）Mapper4之一键生成

### （4）Rest通用Base工程构建

### （5）引入微服务理念，从这里开始

















































































