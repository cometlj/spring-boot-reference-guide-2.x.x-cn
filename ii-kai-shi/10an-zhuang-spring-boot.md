# 10.安装Spring Boot

Spring Boot能够通过传统的Java开发者工具或者命令行工具安装。两者均需要Java SDK v1.8版本以上。在开始安装前，你应该通过以下命令检查现有安装的Java版本：

> $ java -version

如果是一个新手或者你只想实验性质使用下Spring Boot，你可以先尝试使用Spring Boot CLI（命令行界面）。否则，请阅读传统安装说明。

## 10.1 Java开发者安装说明

你能像其他Java标准库一样使用Spring Boot，即在classpath中导入合适的 spring-boot-\*.jar文件。Spring Boot不需要其他的工具集成，所以你能够使用各种IDE或者文本编辑器。同时，Spring Boot应用也没什么特别的，所以你可以和其他Java程序一样运行和调试Spring Boot。

### Maven安装

Spring Boot和Apache Maven 3.2或者以上版本兼容。如果你还没有安装Maven，你可以按照maven.apache.org的说明进行安装。

> **小贴士**
>
> Maven在不同操作系统中都能以安装包管理器形式安装。如果你使用的是OSX的Homebrew，尝试用`brew install maven`  Ubuntu用户可以运行 `sudo apt-get install maven` . 使用Chocolatey 的Windows用户可以用管理员权限运行`choco install maven` 命令即可。

Spring Boot的依赖库使用 `org.springframework.boot` 作为groupId. 通常来说，你的Maven POM文件中会继承`spring-boot-starter-parent` 项目，并声明一个和多个的**Starters **依赖。Spring Boot也提供一个可选的 Maven Plugin 来创建可执行jar文件。

下面的列表展示了一个典型的 pom.xml文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"> 
 <modelVersion>4.0.0</modelVersion>
 <groupId>com.example</groupId> <artifactId>myproject</artifactId> <version>0.0.1-SNAPSHOT</version>
 <!-- Inherit defaults from Spring Boot -->
 <parent> 
   <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-parent</artifactId> <version>2.0.0.RELEASE</version>
 </parent>
<!-- Add typical dependencies for a web application -->
<dependencies>
  <dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId> </dependency>
 </dependencies>
 <!-- Package as an executable jar -->
 <build>
  <plugins>
<plugin> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-maven-plugin</artifactId>
   </plugin>
  </plugins>
</build>
</project>
```

> **小贴士**
>
> **`spring-boot-starter-parent 是使用Spring Boot的绝好方式，`**

















































