# Java 项目 尚筹网

## 项目架构

![image-20211108221043803](C:/Users/dell/AppData/Roaming/Typora/typora-user-images/image-20211108221043803.png)

## 课程学习方法

着眼重点从学习具体技术的语法转变为思考如何实现业务功能需求。

- 点：具体技术点
- 线：每个请求的处理过程是一条线，对应Tomcat的线程池中的一个线程
- 面：多个请求组织在一起完成一个特定模块的功能
- 体：多个模块组合在一起构成一个完整的项目

为了能够更清楚的看清代码的层次

- 目标：**聚焦**当前要完成的任务，目标明确后才能分析实现的思路，甚至有的时候需要对大目标进行拆解，把很多小目标各个击破
- 思路：针对目标的达成进行分析。具体到项目功能的实际开发中，体现为**流程图**
- 代码：参照思路分析时绘制的流程图，把步骤翻译成写代码时的注释，再对照注释编写代码
  



## 环境搭建



![image-20211109213132116](Java%20%E9%A1%B9%E7%9B%AE%20%E5%B0%9A%E7%AD%B9%E7%BD%91.assets/image-20211109213132116.png)

### 项目架构图

![image-20211109213616039](Java%20%E9%A1%B9%E7%9B%AE%20%E5%B0%9A%E7%AD%B9%E7%BD%91.assets/image-20211109213616039.png)

### 项目结构

![image-20211109213903146](Java%20%E9%A1%B9%E7%9B%AE%20%E5%B0%9A%E7%AD%B9%E7%BD%91.assets/image-20211109213903146.png)

![image-20211109213927409](Java%20%E9%A1%B9%E7%9B%AE%20%E5%B0%9A%E7%AD%B9%E7%BD%91.assets/image-20211109213927409.png)

```
- atcrowdfunding01
      - atcrowdfunding02
      - atcrowdfunding03
      - atcrowdfunding04
- atcrowdfunding05
- atcrowdfunding06
```

### 创建数据库和数据库表

- 第一范式：数据库中每一列都不可再分
- 第二范式：在满足第一范式的前提下，每个字段都要和主键完整相关（如果是联合主键，就要和每一个主键相关）
- 第三范式：字段要和主键直接相关

1. 创建`project_crowd`数据库：众筹项目专属数据库

2. 建立**后台管理员表**：`t_admin`，创建表的`sql`语句如下

```
use project_crowd;
drop table if exists t_admin;
create table t_admin
(
  id int not null auto_increment, 		# 主键
  login_acct varchar(255) not null, 	# 登录账号
  user_pswd char(32) not null, 			# 登录密码
  user_name varchar(255) not null, 		# 昵称
  email varchar(255) not null, 			# 邮件地址
  create_time char(19), 				# 创建时间
  primary key (id)						# 主键
);
```

### 基于Maven的MyBatis逆向工程

#### reverse逆向工程配置

MyBatis Generator：https://mybatis.org/generator/index.html

1. `pom` 配置，其主要作用就是：使用`maven`构建（`build`）工程时，插件会连接数据库，并根据数据库中指定的表生成`java`实体类和`Mapper`映射文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.wenjie.crowd</groupId>
    <artifactId>atcrowdfunding06-common-reverse</artifactId>
    <version>1.0-SNAPSHOT</version>

    <!-- 依赖 MyBatis 核心包 -->
    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.2.8</version>
        </dependency>
    </dependencies>

    <!-- 控制 Maven 在构建过程中相关配置 -->
    <build>
        <!-- 构建过程中用到的插件 -->
        <plugins>
            <!-- 具体插件， 逆向工程的操作是以构建过程中插件形式出现的 -->
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.0</version>
                <!-- 插件的依赖 -->
                <dependencies>
                    <!-- 逆向工程的核心依赖 -->
                    <dependency>
                        <groupId>org.mybatis.generator</groupId>
                        <artifactId>mybatis-generator-core</artifactId>
                        <version>1.3.2</version>
                    </dependency>
                    <!-- 数据库连接池 -->
                    <dependency>
                        <groupId>com.mchange</groupId>
                        <artifactId>c3p0</artifactId>
                        <version>0.9.2</version>
                    </dependency>
                    <!-- MySQL 驱动 -->
                    <dependency>
                        <groupId>mysql</groupId>
                        <artifactId>mysql-connector-java</artifactId>
                        <version>5.1.8</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>

</project>
```

2. `generatorConfig.xml`配置文件：在`resources`文件夹下创建`generatorConfig.xml`配置文件，并添加如下内容

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!-- mybatis-generator:generate -->
    <context id="atguiguTables" targetRuntime="MyBatis3">
        <commentGenerator>
            <!-- 是否去除自动生成的注释 true:是;false:否 -->
            <property name="suppressAllComments" value="true" />
        </commentGenerator>

        <!--数据库连接的信息： 驱动类、 连接地址、 用户名、 密码 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/project_crowd"
                        userId="root" password="1243">
        </jdbcConnection>

        <!-- 默认 false， 把 JDBC DECIMAL 和 NUMERIC 类型解析为 Integer， 为 true 时把 JDBC DECIMAL
            和 NUMERIC 类型解析为 java.math.BigDecimal -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>

        <!-- targetProject:生成 Entity 类的路径 -->
        <javaModelGenerator targetProject=".\src\main\java"
                            targetPackage="com.wenjie.crowd.entity">
            <!-- enableSubPackages:是否让 schema 作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true" />
        </javaModelGenerator>
        <!-- targetProject:XxxMapper.xml 映射文件生成的路径 -->
        <sqlMapGenerator targetProject=".\src\main\java"
                         targetPackage="com.wenjie.crowd.mapper">
            <!-- enableSubPackages:是否让 schema 作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>
        <!-- targetPackage： Mapper 接口生成的位置 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetProject=".\src\main\java"
                             targetPackage="com.wenjie.crowd.mapper">
            <!-- enableSubPackages:是否让 schema 作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </javaClientGenerator>
        <!-- 数据库表名字和我们的 entity 类对应的映射指定 -->
        <table tableName="t_admin" domainObjectName="Admin" />
    </context>
</generatorConfiguration>

```

#### Maven执行逆向生成

```
D:\development_tools\Java\jdk1.8.0_131\bin\java.exe -Dmaven.multiModuleProjectDirectory=D:\Developer\Workspace_IDEA\com.wenjie.crowd\atcrowdfunding06-common-reverse -DarchetypeCatalog=internal "-Dmaven.home=C:\development_tools\IntelliJ IDEA 2020.2.3\plugins\maven\lib\maven3" "-Dclassworlds.conf=C:\development_tools\IntelliJ IDEA 2020.2.3\plugins\maven\lib\maven3\bin\m2.conf" "-Dmaven.ext.class.path=C:\development_tools\IntelliJ IDEA 2020.2.3\plugins\maven\lib\maven-event-listener.jar" "-javaagent:C:\development_tools\IntelliJ IDEA 2020.2.3\lib\idea_rt.jar=63908:C:\development_tools\IntelliJ IDEA 2020.2.3\bin" -Dfile.encoding=UTF-8 -classpath "C:\development_tools\Intelli                                                                                           J IDEA 2020.2.3\plugins\maven\lib\maven3\boot\plexus-classworlds-2.6.0.jar;C:\development_tools\IntelliJ IDEA 2020.2.3\plugins\maven\lib\maven3\boot\plexus-classworlds.license" org.codehaus.classworlds.Launcher -Didea.version=2020.2.3 -s D:\development_tools\Maven\apache-maven-3.3.9\conf\settings.xml org.mybatis.generator:mybatis-generator-maven-plugin:1.3.0:generate
[INFO] Scanning for projects...
[INFO] 
[INFO] ----------< com.wenjie.crowd:atcrowdfunding06-common-reverse >----------
[INFO] Building atcrowdfunding06-common-reverse 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- mybatis-generator-maven-plugin:1.3.0:generate (default-cli) @ atcrowdfunding06-common-reverse ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.645 s
[INFO] Finished at: 2021-11-14T17:35:57+01:00
[INFO] ------------------------------------------------------------------------

```

#### 分配逆向生成文件

AminExample 提供了QBC查询：QBC查询就是通过使用Hibernate提供的Query By Criteria API来查询对象，这种API封装了SQL语句的动态拼装，对查询提供了更加面向对象的功能接口

![image-20211115115550364](Java%20%E9%A1%B9%E7%9B%AE%20%E5%B0%9A%E7%AD%B9%E7%BD%91.assets/image-20211115115550364.png)

### 父工程依赖管理

在parent工程的pom文件中对依赖版本进行统一管理

- Spring 版本：4.3.20.RELEASE
- SpringBoot 版本：4.2.10.RELEASE

**版本声明和依赖管理**

- `parent`工程`pom`文件中添加如下依赖，在父工程中声明了依赖的版本（GAV 中的 V），在子工程中就不需要声明依赖的版本了，只需要声明 GA
- 在properties标签内可以把版本号作为变量进行声明，方便maven依赖标签用${变量名}的形式动态获取版本号。这样做的优点是当版本号发生改变时，仅仅需要更新properties标签中的变量值就行了，不用煞费心思更新所有依赖的版本号。
- dependencies相对于dependencyManagement，所有声明在dependencies里的依赖都会自动引入，并默认被所有的子项目继承
- dependencyManagement里只是声明依赖，并不自动实现引入，因此子项目需要显示的声明需要用的依赖。如果不在子项目中声明依赖，是不会从父项目中继承下来的；只有在子项目中写了该依赖项，并且没有指定具体版本，才会从父项目中继承该项，并且version和scope都读取自父pom;另外如果子项目中指定了版本号，那么会使用子项目中指定的jar版本。
  

```xml
<properties>
  <!-- 声明属性， 对 Spring 的版本进行统一管理 -->
  <atguigu.spring.version>4.3.20.RELEASE</atguigu.spring.version>
  <!-- 声明属性， 对 SpringSecurity 的版本进行统一管理 -->
  <atguigu.spring.security.version>4.2.10.RELEASE</atguigu.spring.security.version>
</properties>

<dependencyManagement>
	<dependencies>
		<!-- Spring 依赖 -->
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-orm -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-orm</artifactId>
			<version>${atguigu.spring.version}</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${atguigu.spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>${atguigu.spring.version}</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
			<version>1.9.2</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/cglib/cglib -->
		<dependency>
			<groupId>cglib</groupId>
			<artifactId>cglib</artifactId>
			<version>2.2</version>
		</dependency>
		<!-- 数据库依赖 -->
		<!-- MySQL 驱动 -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.3</version>
		</dependency>
		<!-- 数据源 -->
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid</artifactId>
			<version>1.0.31</version>
		</dependency>
		<!-- MyBatis -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.2.8</version>
		</dependency>
		<!-- MyBatis 与 Spring 整合 -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>1.2.2</version>
		</dependency>
		<!-- MyBatis 分页插件 -->
		<dependency>
			<groupId>com.github.pagehelper</groupId>
			<artifactId>pagehelper</artifactId>
			<version>4.0.0</version>
		</dependency>
		<!-- 日志 -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>1.7.7</version>
		</dependency>
		<dependency>
			<groupId>ch.qos.logback</groupId>
			<artifactId>logback-classic</artifactId>
			<version>1.2.3</version>
		</dependency>
		<!-- 其他日志框架的中间转换包 -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>jcl-over-slf4j</artifactId>
			<version>1.7.25</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>jul-to-slf4j</artifactId>
			<version>1.7.25</version>
		</dependency>
		<!-- Spring 进行 JSON 数据转换依赖 -->
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-core</artifactId>
			<version>2.9.8</version>
		</dependency>
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>2.9.8</version>
		</dependency>
		<!-- JSTL 标签库 -->
		<dependency>
			<groupId>jstl</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>
		<!-- junit 测试 -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.12</version>
			<scope>test</scope>
		</dependency>
		<!-- 引入 Servlet 容器中相关依赖 -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
			<scope>provided</scope>
		</dependency>
		<!-- JSP 页面使用的依赖 -->
		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.1.3-b06</version>
			<scope>provided</scope>
		</dependency>
		<!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
		<dependency>
			<groupId>com.google.code.gson</groupId>
			<artifactId>gson</artifactId>
			<version>2.8.5</version>
		</dependency>
		<!-- SpringSecurity 对 Web 应用进行权限管理 -->
		<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-web</artifactId>
			<version>${atguigu.spring.security.version}</version>
		</dependency>
		<!-- SpringSecurity 配置 -->
		<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-config</artifactId>
			<version>${atguigu.spring.security.version}</version>
		</dependency>
		<!-- SpringSecurity 标签库 -->
		<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-taglibs</artifactId>
			<version>${atguigu.spring.security.version}</version>
		</dependency>
	</dependencies>
</dependencyManagement>

```

### Spirng和MyBatis整合

#### 整合目标

目标：adminMapper通过IOC容器装配到当前组件中后，就可以直接调用它的方法，相关感受框架给我们提供的方便。

![image-20211115184510415](Java%20%E9%A1%B9%E7%9B%AE%20%E5%B0%9A%E7%AD%B9%E7%BD%91.assets/image-20211115184510415.png)

![image-20200919163202427](https://img-blog.csdnimg.cn/img_convert/77caa144e40e8c6a5d154bf036c729a0.png)

> Mybatis的Mapper类的自动装配不能够使用context:compoent-scan来进行扫描，必须使用MapperScannerConfigurer

首先在子工程中加入搭建环境所需要的具体依赖（jar包）

-  准备 jdbc.properties：存放数据库连接信息
- 创建 Spring 配置文件(spring-persist-mybatis.xml)，专门配置 Spring 和 MyBatis 整合相关
- 在 Spring 的配置文件中加载 jdbc.properties 属性文件
- 使用 ${} 表达式取出 jdbc.properties 中的配置信息，用于配置数据源
  测试从数据源中获取数据库连接，测试载当前的配置文件下，数据库是否能正常连接
- 配置 SqlSessionFactoryBean：SqlSessionFactoryBean 用于创建 SqlSession，对应于数据库的一条连接
  - 装配数据源
  - 指定 XxxMapper.xml 配置文件的位置
  - 指定 MyBatis 全局配置文件的位置（可选）
- 配置 MapperScannerConfigurer，用于将 Mapper 接口的代理对象扫描到 IOC 容器中
- 测试是否可以装配 XxxMapper 接口并通过这个接口操作数据库

#### 操作步骤详情

##### **在子工程加入搭建环境所需的具体依赖**

子工程，选择component工程，原因是具体依赖和compoent工程相关。

```
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-orm</artifactId>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-webmvc</artifactId>
</dependency>
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
  <groupId>org.aspectj</groupId>
  <artifactId>aspectjweaver</artifactId>
</dependency>
<!-- https://mvnrepository.com/artifact/cglib/cglib -->
<dependency>
  <groupId>cglib</groupId>
  <artifactId>cglib</artifactId>
</dependency>
<!-- MySQL 驱动 -->
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
</dependency>
<!-- 数据源 -->
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>druid</artifactId>
</dependency>
<!-- MyBatis -->
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
</dependency>
<!-- MyBatis 与 Spring 整合 -->
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis-spring</artifactId>
</dependency>
<!-- MyBatis 分页插件 -->
<dependency>
  <groupId>com.github.pagehelper</groupId>
  <artifactId>pagehelper</artifactId>
</dependency>
<!-- Spring 进行 JSON 数据转换依赖 -->
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-core</artifactId>
</dependency>
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
</dependency>
<!-- JSTL 标签库 -->
<dependency>
  <groupId>jstl</groupId>
  <artifactId>jstl</artifactId>
</dependency>
<!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
<dependency>
  <groupId>com.google.code.gson</groupId>
  <artifactId>gson</artifactId>
</dependency>

```

##### **jdbc.properties配置文件**

```
jdbc.user=root
jdbc.password=1243
jdbc.url=jdbc:mysql://localhost:3306/project_crowd?useUnicode=true&characterEncoding=UTF-8
jdbc.driver=com.mysql.jdbc.Driver
```

##### **名存实亡的mybatis-config.xml**

- 在`webui`工程的`resources`文件夹中添加`mybatis-config.xml` 配置文件
- `mybatis-config.xml`主要用于配置`mybatis`，但由于很多配置都在`Spring`配置文件中，所以该配置文件其实名存实亡

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

</configuration>
```

##### spring-persist-mybatis.xml

首先在`webui`工程的`resources` 根目录文件夹下添加`spring-persist-mybatis.xml` 配置文件



##### 测试数据库连接

添加测试依赖：由于单元测试的范围为 `scope` ，无法从父工程中传递至子工程，所以我们需要在`webui`工程中添加单元测试相关依赖

```
<!-- 添加junit依赖 -->
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <scope>test</scope>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-test</artifactId>
  <scope>test</scope>
</dependency>

```

```java
package com.wenjie.crowd.test;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

/**
 * @author Wenjie FU
 * @create 2021-11-15 21:16
 */
//指定 Spring 给 Junit 提供的运行器类
@RunWith(SpringJUnit4ClassRunner.class)
//加载 Spring 配置文件的注解
@ContextConfiguration(locations = { "classpath:spring-persist-mybatis.xml" })
public class CrowdTest {
    @Autowired
    private DataSource dataSource;

    @Test
    public void testDataSource() throws SQLException {
        // 1.通过数据源对象获取数据源连接
        Connection connection = dataSource.getConnection();
        // 2.打印数据库连接
        System.out.println(connection);
    }
}
```

##### **配置sqlSessionFactoryBean**

在spring-persisit-mybatis.xml中配置sqlSessionFactoryBeam

```
<!-- 配置 SqlSessionFactoryBean -->
<bean id="sqlSessionFactoryBean"
      class="org.mybatis.spring.SqlSessionFactoryBean">
  <!-- 装配数据源 -->
  <property name="dataSource" ref="dataSource" />
  <!-- 指定 MyBatis 全局配置文件位置 -->
  <property name="configLocation"
            value="classpath:mybatis-config.xml" />
  <!-- 指定 Mapper 配置文件位置 -->
  <property name="mapperLocations"
            value="classpath:mybatis/mapper/*Mapper.xml" />
</bean>

```

##### **配置MapperScannerConfigurer**

在spring-persisit-mybatis.xml中配置MapperScannerConfigurer

```
<!-- 配置 MapperScannerConfigurer -->
<!-- 把 MyBatis 创建的 Mapper 接口类型的代理对象扫描到 IOC 容器中 -->
<bean id="mapperScannerConfigurer"
      class="org.mybatis.spring.mapper.MapperScannerConfigurer">
  <!-- 使用 basePackage 属性指定 Mapper 接口所在包 -->
  <property name="basePackage" value="com.atguigu.crowd.mapper" />
</bean>

```

##### 最终测试

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">

    <!-- 加载 jdbc.properties -->
    <context:property-placeholder
            location="classpath:jdbc.properties" />
    <!-- 配置数据源 -->
    <bean id="dataSource"
          class="com.alibaba.druid.pool.DruidDataSource">
        <!-- 连接数据库的用户名 -->
        <property name="username" value="${jdbc.user}" />
        <!-- 连接数据库的密码 -->
        <property name="password" value="${jdbc.password}" />
        <!-- 目标数据库的 URL 地址 -->
        <property name="url" value="${jdbc.url}" />
        <!-- 数据库驱动全类名 -->
        <property name="driverClassName" value="${jdbc.driver}" />
    </bean>

    <!--配置sqlSessionFactoryBean整合MyBatis -->
    <bean id="sqlSessionFactoryBean"
          class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 装配数据源 -->
        <property name="dataSource" ref="dataSource" />
        <!-- 指定 MyBatis 全局配置文件位置 -->
        <property name="configLocation"
                  value="classpath:mybatis-config.xml" />
        <!-- 指定 Mapper 配置文件位置 -->
        <property name="mapperLocations"
                  value="classpath:mybatis/mapper/*Mapper.xml" />
    </bean>

    <!-- 配置 MapperScannerConfigurer -->
    <!-- 把 MyBatis 创建的 Mapper 接口类型的代理对象扫描到 IOC 容器中 -->
    <bean id="mapperScannerConfigurer"
          class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 使用 basePackage 属性指定 Mapper 接口所在包 -->
        <property name="basePackage" value="com.wenjie.crowd.mapper" />
    </bean>




</beans>

```

