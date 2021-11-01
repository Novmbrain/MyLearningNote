# Maven学习笔记

# 1、Introduction



## 1.1、 软件是个工程

**完成一个Java项目，需要哪些步骤**

1. 分析项目要做什么，知道项目有哪些组成成分
2. 设计项目，需要哪些步骤，使用哪些技术，需要多少人，多长时间
3. 组建团队，招人，购置设备，服务器，软件
4. 开发人员写代码，开发人员需要测试自己写的代码，重复到OK
5. 测试人员，测试项目功能是否符合功能->开发修改-->测试-->修改.......完成

**传统的开发项目的问题，也就是没有使用maven管理的项目**

1. 有非常多的模块，模块之间有关系，手工管理关系，比较繁琐
2. 项目需要非常多的第三方的功能，也就是很多jar文件，需要手工从网络中获取各个jar包
3. 需要管理jar的版本。
4. 管理jar文件之间的依赖

**Maven是如果改进项目开发和管理的**

1. Maven可以管理jar文件
2. 自动下载jar和他的文档，源代码
3. 管理jar的直接一脸，a.jar 需要b.jar，maven会自动下载b.jar
4. 管理你需要的jar版本
5. 帮你编译程序，把java编译为class
6. 帮你测试你的代码是否正确
7. 帮你打包文件，形成jar文件，或者war文件
8. 帮你部署项目

**项目的构建**

构建是面向过程的，就是一些步骤，完成项目代码的编译，测试，运行，打包和部署等等。maven支持的构建包括

1. 清理，把之前项目编译的东西删除掉，为新的编译代码做准备
2. 编译，把程序源代码编译为执行代码，java-class文件。批量的把所有的文件编译为class文件。
3. 测试，Maven可以批量的执行程序代码，验证功能是否正确
4. 报告，生成测试文件，测试通过了没有
5. 打包，把项目中所有的class文件，配置文件等所有资源放到一个压缩文件中。这个压缩文件就是项目的结果文件。通常Java程序压缩文件是jar扩展名。对于Web应用，压缩文件扩展名是.war
6. 安装，把5中生成的文件安装到本地仓库之中
7. 部署，把程序安装好，可以执行

## 1.2、Maven的核心概念

1. POM：项目对象模型，Maven把一个项目看做一个模型，控制Maven构建项目的过程，管理jar依赖
2. 约定的目录结构：Maven项目的目录和文件的位置都是规定的
3. 坐标：是一个唯一的字符串，用来表示资源的
4. 依赖管理：管理你的项目中可以使用jar文件
5. 仓库管理：你的资源存放的位置
6. 生命周期：Maven工具构建项目的过程
7. 插件和目标：执行Maven构建时候构建的工具是插件

## 1.3、Maven的安装步骤

1. 官网下载，解压到一个非中文的目录

- bin：执行程序，主要是mvn.cmd
- cof：maven工具本身的配置文件 settings.xml

2. 配置环境变量

   在系统的环境变量中，指定一个M2_HOME的名称，指定它的值是Maven工具安装目录，bin之前的目录

   ```
   D:\development_tools\Maven\apache-maven-3.3.9
   把M2_HOME加入到path之中
   ```

# 2、Maven的核心概念



## 2.1、Maven约定的目录结构

每个Maven项目在磁盘中都是一个文件夹

```
Hello/
	|---/src
	|---|---/main 			#放着主程序的java代码和配置文件
	|---|---|---/java   	#放你的程序包和包中的java文件
	|---|---|---/resources 	 #你的java程序中要使用的配置文件
	|---|---/test 		     #放测试程序代码和文件
 	|---|---|---/java    	 #放你的测试程序包和包中的java文件
	|---|---|---/resources 	 #你的测试程序中要使用的配置文件
	
	|---/pom.xml 			#Maven的核心文件，Maven项目必须有
```

在src目录下执行mvn  compile可以编译整个项目，然后生成target文件

```
D:\developer\MavenTest>mvn compile
```

## 2.2、修改本机资源的存放位置

1. 修改maven的配置文件，maven安装目录下的/conf/settings.xml。首先我们需要泵费setting.xml

2. 修改local_repository指定你的目录

   ```
     <!-- localRepository
      | The path to the local repository maven will use to store artifacts.
      |
      | Default: ${user.home}/.m2/repository
     <localRepository>/path/to/local/repo</localRepository>
     -->
     <localRepository>D:/development_tools/Maven/maven_repository</localRepository>
   ```

## 2.3、仓库

仓库是什么：仓库里存放着maven使用的jar和我们项目使用的jar

- maven使用的插件（各种jar）
- 我门项目使用的jar（第三方的工具）

仓库的分类：

- 本地仓库，个人计算机上的文件夹，存放这各种jar
- 远程仓库，在互联网上，连接网络才能使用的仓库
  - 中央仓库
  - 中央仓库的镜像

仓库的使用：maven仓库的使用不需要认为参与，假设开发人员需要使用mysql驱动，maven首先查看本地仓库-->私人服务器-->镜像-->中央仓库



## 2.4、POM文件

POM( Project Object Model，项目对象模型 ) 是 Maven 工程的基本工作单元，是一个XML文件，包含了项目的基本信息，用于描述项目如何构建，声明项目依赖，等等。

执行任务或目标时，Maven 会在当前目录中查找 POM。它读取 POM，获取所需的配置信息，然后执行目标。

所有 POM 文件都需要 project 元素和三个必需字段：groupId，artifactId，version。

| 节点                      | 描述                                                         |
| :------------------------ | :----------------------------------------------------------- |
| project                   | 工程的根标签。                                               |
| modelVersion              | 目前模型版本需要设置为 4.0。                                 |
| groupId                   | 这是工程组的标识，一般是公司域名的倒写。它在一个组织或者项目中通常是唯一的。例如，一个银行组织 com.companyname.project-group 拥有所有的和银行相关的项目。 |
| artifactId                | 这是工程的标识。它通常是工程的名称。例如，消费者银行。groupId 和 artifactId 一起定义了 artifact 在仓库中的位置。 |
| version                   | 这是工程的版本号。在 artifact 的仓库中，它用来区分不同的版本。例如：`1.0-SNAPSHOT`，SNAPSHOT表示版本快照，不是稳定版本 |
| packaging                 | 打包后压缩文件的扩展名，默认是jar，web应用是war              |
| dependencies 和dependency | 你的项目中要使用的各种资源说明，也就是所要用的依赖的坐标。相当于java代码中的import |
| build                     | build表示与构建相关的配置，例如设置编译插件的jdk版本         |



```
这个三项称为坐标，在互联网中唯一标识一个项目
  <groupId>com.bjpowernode.maven</groupId> 公司域名倒写
  <artifactId>ch01-maven</artifactId> 自定义的项目名称
  <version>1.0-SNAPSHOT</version> 自定义的版本号
```

可以再maven搜索使用的中央仓库查找要用的依赖项目的坐标，将依赖放在dependency中

```
https://mvnrepository.com/

<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.26</version>
</dependency>
```

![image-20210801133421593](Maven%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210801133421593.png)

## 2.5、Maven生命周期，命令和插件

- Maven的生命周期就是Maven构建项目的过程，清理，编译 ，测试，报告，打包，安装，部署
- maven的命令：maven通过命令完成生命周期的执行
- maven的插件，maven命令执行时，真正完成功能的是插件，插件就是一些jar文件

1. 单元测试：junit是一个专门测试的框架，maven可以通过单元测试，批量测试类中的大量方法是否符合预期
   - 在pom.xm加入单元测试依赖
   - 在maven项目中的src/test/java目录下，创建测试程序。推荐的命名规则如下
     - 测试类名称是Test+你要测试的类名
     - 测试的方法名称 Test+方法名称



2. maven常见命令

   ### 常用命令

   | 命令                   | 描述                                                         |
   | ---------------------- | ------------------------------------------------------------ |
   | mvn –version           | 显示版本信息                                                 |
   | mvn clean              | 清理项目生产的临时文件,一般是模块下的target目录              |
   | mvn compile            | 编译源代码，一般编译模块下的src/main/java目录                |
   | mvn test-compile       | 测试文件的编译                                               |
   | mvn package            | 项目打包工具,只打包sr/main下的文件，会在模块下的target目录生成jar或war等文件 |
   | mvn test               | 测试命令,或执行src/test/java/下junit的测试用例.              |
   | mvn install            | 将打包的jar/war文件复制到你的本地仓库中,供其他模块使用       |
   | mvn deploy             | 将打包的文件发布到远程参考,提供其他人员进行下载依赖          |
   | mvn site               | 生成项目相关信息的网站                                       |
   | mvn eclipse:eclipse    | 将项目转化为Eclipse项目                                      |
   | mvn dependency:tree    | 打印出项目的整个依赖树                                       |
   | mvn archetype:generate | 创建Maven的普通java项目                                      |
   | mvn tomcat:run         | 在tomcat容器中运行web应用                                    |
   | mvn jetty:run          | 调用 Jetty 插件的 Run 目标在 Jetty Servlet 容器中启动 web 应用 |

3. mvn compile

   - 编译main/java/目录下的java文件为class文件，同时把class文件拷贝到target/classes目录下
   - 把main/resources目录下的所有文件都拷贝到target/classes目录下

   

4. mvn test-compile测试文件的编译
5. mvn test：这个命令会先执行mvn clean, mvn complie, mvn test-compile，最后再进行测试。所以可以直接运行这个命令。并且会在surefire-reports下生成报告

# 3、在IDEA中设置Maven

IDEA中内置了一个Maven，但是一般不使用内置的，因为内置的修改Maven参数不方便，而是使用自己安装的Maven。所以需要用自己安装的Maven覆盖内置Maven。让IDEA知道Maven安装位置的信息

## 3.1、Maven在IDEA中的配置

1. IDEA中file->setting是为当前项目设置，file->new projects settings->settings fornew projects是为所有项目进行设置

![image-20210801135510013](Maven%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210801135510013.png)

![image-20210801135305824](Maven%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210801135305824.png)

![image-20210801135400829](Maven%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210801135400829.png)

```
-DarchetypeCatalog=internal
```



## 3.2、IDEA创建Maven管理的项目

先创建一个Empyt Project，然后使用quickstart模板创建项目

![image-20210801141607917](Maven%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210801141607917.png)

![image-20210801141226115](Maven%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210801141226115.png)

![image-20210801140850521](Maven%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210801140850521.png)

![image-20210801142735849](Maven%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210801142735849.png)

## 3.3、Maven管理Web工程



![image-20210801163502423](Maven%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210801163502423.png)

# 4、 Maven技巧

## 4.1、遇到导入依赖没有及时刷新

![image-20210801164843211](Maven%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210801164843211.png)



## 4.2、依赖范围

![image-20210801172658389](Maven%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210801172658389.png)

依赖范围用scope标签表示，也就是确定依赖在maven构建项目的哪些阶段起作用

scope的值有compile，test，provided。 默认的值是compile，在所有阶段都会用到

provided：在编译，测试起作用，之后的打包，安装，部署不需要提供

```xml
Junit的依赖范围是test  
<dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
```













