# SSM项目实战-CRUD实现

# 介绍

## 功能点

1. 分页
2. 数据校验:jquery前端校验+JSR303后端校验
3. ajax
4. Rest风格的URI，即使用HTTP协议请求方式的动词，来表示对资源的操作（GET，POST，PUT，DELETE）

## 技术点

1. 基础框架：ssm
2. 数据库：mysql
3. 前端框架：bootstrap快速搭建简洁美观的界面
4. 项目的依赖管理：Maven
5. 分页：pagehelper
6. 逆向工程-MyBatis Generator



## 基础环境搭建

### Maven环境

1. 创建Maven工程

2. 引入项目依赖的jar包
   - spring
   - springMVC
   - mybatis
   - 数据库连接池，驱动包
   - 其他:jstl, servlet-api, junit
   
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     <modelVersion>4.0.0</modelVersion>
   
     <groupId>com.wenjie</groupId>
     <artifactId>ssmcrud</artifactId>
     <version>1.0-SNAPSHOT</version>
     <packaging>war</packaging>
   
     <name>ssmcrud Maven Webapp</name>
     <!-- FIXME change it to the project's website -->
     <url>http://www.example.com</url>
   
     <properties>
       <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
       <maven.compiler.source>1.7</maven.compiler.source>
       <maven.compiler.target>1.7</maven.compiler.target>
     </properties>
   
   <!--  引入项目依赖的包-->
     <dependencies>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.12</version>
         <scope>test</scope>
       </dependency>
   
       <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>4.3.7.RELEASE</version>
       </dependency>
   
       <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-jdbc</artifactId>
         <version>4.3.7.RELEASE</version>
       </dependency>
   
       <!-- https://mvnrepository.com/artifact/org.springframework/spring-aspects -->
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-aspects</artifactId>
         <version>4.3.7.RELEASE</version>
       </dependency>
   
       <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
       <dependency>
         <groupId>org.mybatis</groupId>
         <artifactId>mybatis</artifactId>
         <version>3.4.2</version>
       </dependency>
   
   <!--   Mybais整合Spring的适配包 -->
       <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
       <dependency>
         <groupId>org.mybatis</groupId>
         <artifactId>mybatis-spring</artifactId>
         <version>1.3.1</version>
       </dependency>
   
   <!--    数据库连接池和驱动-->
       <!-- https://mvnrepository.com/artifact/c3p0/c3p0 -->
       <dependency>
         <groupId>c3p0</groupId>
         <artifactId>c3p0</artifactId>
         <version>0.9.0.2</version>
       </dependency>
   
       <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
       <dependency>
         <groupId>mysql</groupId>
         <artifactId>mysql-connector-java</artifactId>
         <version>5.1.49</version>
       </dependency>
   <!--jstl, servlet-api, junit-->
       <!-- https://mvnrepository.com/artifact/jstl/jstl -->
       <dependency>
         <groupId>jstl</groupId>
         <artifactId>jstl</artifactId>
         <version>1.2</version>
       </dependency>
   
       <!-- https://mvnrepository.com/artifact/javax.servlet/servlet-api -->
       <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>servlet-api</artifactId>
         <version>2.5</version>
         <scope>provided</scope>
       </dependency>
   
   
   
     </dependencies>
   
     <build>
       <finalName>ssmcrud</finalName>
       <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
         <plugins>
           <plugin>
             <artifactId>maven-clean-plugin</artifactId>
             <version>3.1.0</version>
           </plugin>
           <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
           <plugin>
             <artifactId>maven-resources-plugin</artifactId>
             <version>3.0.2</version>
           </plugin>
           <plugin>
             <artifactId>maven-compiler-plugin</artifactId>
             <version>3.8.0</version>
           </plugin>
           <plugin>
             <artifactId>maven-surefire-plugin</artifactId>
             <version>2.22.1</version>
           </plugin>
           <plugin>
             <artifactId>maven-war-plugin</artifactId>
             <version>3.2.2</version>
           </plugin>
           <plugin>
             <artifactId>maven-install-plugin</artifactId>
             <version>2.5.2</version>
           </plugin>
           <plugin>
             <artifactId>maven-deploy-plugin</artifactId>
             <version>2.8.2</version>
           </plugin>
         </plugins>
       </pluginManagement>
     </build>
   </project>
   
   ```
   
   

### 引入bootstrap前端框架

[BootStrap官网]: https://getbootstrap.com/

1. 从官网上下载Bootstrap压缩包，解压后引入的webapp/static文件夹下
2. 顺便引入jquery
3. 以index.jsp为示例演示如何在jsp页面中添加Bootstrap和jquery依赖

![image-20211009111916701](SSM%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%88%98-CRUD%E5%AE%9E%E7%8E%B0.assets/image-20211009111916701.png)

```html
<%--
  Created by IntelliJ IDEA.
  User: dell
  Date: 2021/10/9
  Time: 10:53
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
<%--    引入jquery--%>
    <script type="text/javascript" src="static/js/jquery-1.7.2.min.js"></script>
<%--    引入bootstrap样式--%>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-uWxY/CJNBR+1zjPWmfnSnVxwRheevXITnMqoEIeG1LJrdI0GlVs/9cVSyPYXdcSF" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-kQtW33rZJAHjgefvhyyzcGF3C5TFyBQBA13V1RKPf4uH+bwyzQxZ6CmMZHmNBEfJ" crossorigin="anonymous"></script>

</head>
<body>
<button class="btn-success">按钮</button>

</body>
</html>

```

### 编写ssm整合关键配置文件

#### web.xml文件配置

```xml
<!DOCTYPE web-app PUBLIC
        "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
        "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>

    <display-name>Archetype Created Web Application</display-name>
    <!--    1. Spring 的配置文件-->
    <!--  启动spring容器-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>


    <!--   监听器 -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!--    2. SpringMVC的前端控制器，拦截所有请求-->

    <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springMVC.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!--    3. 字符编码过滤器,一定要放在所有过滤器之前-->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>

        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>

        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>

    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!-- 4、使用Rest风格的URI，将页面普通的post请求转为指定的delete或者put请求 -->
    <filter>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>

```

#### SpringMVC配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.3.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">
    
<!--    SpringMVC的配置文件-->
<!--需要禁用默认的过滤器使得自定义的过滤器能够生效-->
    <context:component-scan base-package="com.wenjie.crud.controller">
        <!--        只扫描控制器-->
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

<!--    配置视图解析器，方便页面返回-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>

    <!--两个标准配置-->
    <!--将SpringMVC不能够处理的请求交给Tomcat-->
    <!--    我们在配置dispatchServlet时配置<url-pattern>/</url-pattern>拦截所有请求，这时候dispatchServlet完全取代了default servlet，将不会再访问容器中原始默认的servlet，而对静态资源的访问就是通过容器默认servlet处理的，故而这时候静态资源将不可访问。

    如果想要解决访问静态资源问题，通常会使用默认handler：

    <mvc:default-servlet-handler/>
    -->
    <mvc:default-servlet-handler/>
    <!-- 开启mvc注解驱动 -->
    <mvc:annotation-driven/>
</beans>

```

#### Spring配置文件

数据库propeties文件

```
jdbc.jdbcUrl=jdbc:mysql://localhost:3306/ssm_crud
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.user=root
jdbc.password=1243
```

