# 11.开发你的第一个Spring Boot应用

本章描述了如何开发一个简单的“Hello World”Web应用，并着重展现出Spring Boot的主要特性。我们使用Maven来编译该项目，因为大多数IDE都支持。

> **小贴士**
> 
> [spring.io](spring.io)官网包括了很多Spring Bootd“如何上手”的[参考](https://spring.io/guides)指引。如果你需要解决特定问题，建议先查阅下。
> 
> 你可以按照[start.spring.io]下的快捷步骤入手，在依赖搜索框中选择“Web”启动器。这样会生成一个新的项目脚手架，如此便可以马上开始[编码工作11.3-anchor]()了。详情请查阅[Spring Initializr documentaion](https://github.com/spring-io/initializr)

在开始之前，打开一个终端控制台并且运行以下命令，以确保你已经安装了符合条件的Java及Maven版本：

```bash
$ java -version
java version "1.8.0_102"
Java(TM) SE Runtime Environment (build 1.8.0_102-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.102-b14, mixed mode)
```

```bash
$ java -version
java version "1.8.0_102"
Java(TM) SE Runtime Environment (build 1.8.0_102-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.102-b14, mixed mode)
```

> **小贴士**

> 该样例需要新建一个单独的项目文件夹。随后的步骤都假定你已经新建了一个合适的文件并且都在当前目录下操作。

### 11.1 生成一个POM文件

开始我们需要创建一个`pom.xml`文件。`pom.xml`是构建你的项目的依赖清单列表。打开你最喜欢的文本编辑器后添加一下内容：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"> <modelVersion>4.0.0</modelVersion>
	<groupId>com.example</groupId> 
	<artifactId>myproject</artifactId> 	
	<version>0.0.1-SNAPSHOT</version>
	<parent> 
	  <groupId>org.springframework.boot</groupId> 
	  <artifactId>spring-boot-starter-parent</artifactId> 		  
	  <version>2.0.0.RELEASE</version>
	</parent>
	 <!-- Additional lines to be added here... -->
</project>
```

之前的列表应该能够让你编译了。你可以通过运行 `mvn package`来测试（目前为止，你可以忽略"jar will be empty - no content was marked for inclusion!"的警告）

> **小贴士**
> 
> 此时，你就可以到向IDE中导入该项目了（大多数Java IDE都包含了对Maven的内置支持）为简化操作，我们在例子中仍然使用一个普通的文本编辑器。

### 11.2 添加Classpath依赖

Spring Boot提供了很多“Starters”启动器以便你添加jar包到你的项目路径下。我们的样例应用POM文件的`parent`区域中，已经使用了`spring-boot-starter-parent`。`spring-boot-starter-parent`是一个比较特殊的启动器，他提供了很多Maven默认的支持。同时它也提供了一个[depency-management-13.1-anchor]()的代码块，让你能够省略`version`标签在一些依赖中。

其他的“Starters”启动器提供了你可能在开发中会需要的一些特定应用类型依赖。因为我们在开发的是一个Web应用，所以只添加了`spring-boot-boot-starter-web`依赖。在此之前，我们通过运行程序

```bash
 $ mvn dependency:tree
[INFO] com.example:myproject:jar:0.0.1-SNAPSHOT
```

`mvn dependency:tree`命令会打印出你的项目内依赖的树形列表。你可以看到`spring-boot-starter-parent`是根节点，无需其他依赖。如果要添加必须的依赖，请编辑项目中的`pom.xml`并且在`parent`节点下方。添加`spring-boot-starter-web` 依赖即可。

```xml
<dependencies>
 <dependency>
	<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-web</artifactId> 
	</dependency>
</dependencies>
```

这时你再运行`dependency:tree`命令，你可以看到有一些其他的依赖库被引入了，包含Tomcat Web服务器和Spring Boot自身。

### 11.3 编写代码

为了完成该应用，我们还需要创建一个Java文件。默认Maven会从`src/main/java`目录进行编译源文件，所以你只需要创建该目录结构，并添加一个文件命名为`src/main/java/Example.java`即可，编码如下：

```java
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*; import org.springframework.web.bind.annotation.*;
@RestController @EnableAutoConfiguration public class Example {
 	@RequestMapping("/")
	String home() {
		return "Hello World!";
	}
	public static void main(String[] args) throws Exception { 
		SpringApplication.run(Example.class, args);
	}
}
```

虽然代码量不多，但是已经可以运行了。我们将在下章节继续讲解重要的部分。

#### @RestController and @RequestMapping注解

`Example`类中的第一个注解是@RestController。这被叫做标准化注解。它给编码者以提示该端代码是Spring相关的，并且该类充当的一个特定角色。这样，我们的类就变成一个Web @Controller 了，Spring会在处理接入web请求时候调用该类。

@RequestMapping注解提供了一个“路由”信息。它告诉Spring任何以 / 路径的HTTP请求，都会被映射到`home`方法。@RestController注解告诉Spring直接渲染结果字符串给调用者。

> **小贴士**
> 
> @RestController和@RequestMapping注解归属为Spring MVC的注解。（不是Spring Boot专属）。更多详情可以查阅Spring参考文档[MVC section](https://docs.spring.io/spring/docs/5.0.4.RELEASE/spring-framework-reference/web.html#mvc)

#### @EnableAutoConfiguration注解

第二级注解是 @EnableAutoConfiguration。这个注解告诉Spring Boot去“猜测”你想如何根据你添加的jar包一类来设置Spring。因为`spring-boot-starter-web`已经添加了Tomcat和Spring MVC，自动注解机制也假定你在开发一个web应用程序，并且由此设置了Spring的配置。

> **启动器和自动配置**
> 
> 自动配置的设计是为了更好的与“启动器”配合工作，但是这两个概念不是直接关联在一起的。你可以在starters启动器之外选择其他的jar包依赖。Spring Boot仍然会自动配置你的应用程序。

#### “main”方法

我们程序的最后一部分是一个`main`方法。这只是一个遵循Java约定的标准程序入口方法。main方法已经通过调用`run.SpringApplication`,将程序构建，启动Spring，自动配置的Tomcat服务器功能，都托管给Spring Boot的`SpringApplication`类了。我们需要将`Example.class`作为运行时参数传入，以告诉`SpringApplication`哪一个是Spring的主要部分。这个`args`数组也可以通过命令行参数来进行传入。

### 11.4 运行样例

这时候，你的应用程序应该能够运行了。因为引入了`spring-boot-starter-parent`的POM，你可以使用`run`作为启动项目的目标--Not good. 在项目根目录下输入`mvn spring-boot:run`即可启动该项目。然后就可以看到比较熟悉的输出了：

```bash
$ mvn spring-boot:run
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::  (v2.0.0.RELEASE)
....... . . .
....... . . . (log output here)
....... . . .
........ Started Example in 2.222 seconds (JVM running for 6.514)
```

如果你打开浏览器输入网址 [localhost:8080](localhost:8080)，即可看到以下输出：

```bash
Hello World!
```

退出程序，请按 `ctrl-c`

#### 11.5 创建可执行jar包

我们通过创建一个可以在生产环境独立运行的可执行jar包来完成我们的例子。可执行jar包（有时候会被叫做“fat jars”）是一种包括了编译类以及所有jar包依赖和你代码运行所必须的内容的文件。

> **可执行jars包和Java**
> 
> Java不提供加载内置jar文件的标准方法（jar本身包含在JAR中的JAR文件）。如果要分发自包含的应用程序，这可能会有些问题。
> 
> 为了解决该问题，很多开发者会使用“超级”jars。一个超级jar会将所有程序依赖中包含的类打包至一个单一文件。这种方法的问题是，很难看出应用程序中有哪些库。如果在多个容器中使用相同的文件名（但内容不同），也可能有问题。
> 
> Spring Boot使用了[不同的方法-375页]()来允许你直接内嵌jar包文件。

为创建一个可执行jar包，我们需要在`pom.xml`中添加`spring-boot-maven-plugin`依赖，在`dependencies`节点下方插入以下代码：

```xml
<build>
 <plugins>
	<plugin> 
		<groupId>org.springframework.boot</groupId> 			
		<artifactId>spring-boot-maven-plugin</artifactId>
  	</plugin>
 </plugins>
</build>
```

> **小贴士**
> 
> `spring-boot-starter-parent`POM节点包含了一个<executions>配置项来绑定`repackage`目标。如果你没有使用父级POM，则必须在配置项中声明自身。详情请查阅[plugin documentation](https://docs.spring.io/spring-boot/docs/2.0.0.RELEASE/maven-plugin/usage.html)

保存`pom.xml`并在命令行中执行 `mvn package`，代码如下：

```bash
$ mvn package
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building myproject 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] .... ..
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ myproject ---
[INFO] Building jar: /Users/developer/example/spring-boot-example/target/myproject-0.0.1-SNAPSHOT.jar
[INFO]
[INFO] --- spring-boot-maven-plugin:2.0.0.RELEASE:repackage (default) @ myproject ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
```

这时打开`target`目录，你应该可以看到`myproject-0.0.1-SNAPSHOT.jar`文件了。这个文件应该差不多10MB大小。如果你想预览下jar包内容，可以使用如下命令：

```bash
$ jar tvf target/myproject-0.0.1-SNAPSHOT.jar
```

你应该在target目录中也会看到一个比较小的文件，名称为`myproject-0.0.1-SNAPSHOT.jar.original`。这是Maven在Spring Boot重新打包前创建的一个文件。

运行该项目，可以使用如下代码：

```bash
$ java -jar target/myproject-0.0.1-SNAPSHOT.jar
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::  (v2.0.0.RELEASE)
....... . . .
....... . . . (log output here)
....... . . .
........ Started Example in 2.536 seconds (JVM running for 2.864)
```

同上，按`ctrl-c`即可退出应用程序











