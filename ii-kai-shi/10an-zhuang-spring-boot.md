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
> `spring-boot-starter-parent` 是使用Spring Boot的绝好方式，但不是任何时候都合适。有时候你需要继承不同的父POM文件，或许你不喜欢默认设置。在这些情况下，请查看章节 **the section called “Using Spring Boot without the Parent POM”**

### Gradle安装

Spring Boot兼容Gradle 4。如果你还未安装Gradle，可以查看官网说明 [gradle.org](gradle.org)

Spring Boot的依赖库使用 `org.springframework.boot` 作为groupId. 通常来说，你的项目会声明一个或多个**Starters** 。Spring Boot提供了一个很有用的插件Gradle Plugin，能够简化依赖库声明和创建可执行jar文件的过程。

> **Gradle Wrapper**  
> Gradle Wrapper提供了一种很好的方式来获取包，当你需要编译项目的时候。你只需要提交很简短的脚本和库声明，连同你的代码一起即可构建编译处理。详情见[docs.gradle.org/4.2.1/userguide/gradle\_wrapper.html](docs.gradle.org/4.2.1/userguide/gradle_wrapper.html)

下面的例子展示了一个典型的 `build.gradle`文件：

```bash
plugins {
    id 'org.springframework.boot' version '2.0.0.RELEASE' id 'java'
}
jar {
    baseName = 'myproject' version = '0.0.1-SNAPSHOT'
}
repositories {
     jcenter()
}
dependencies { 
    compile("org.springframework.boot:spring-boot-starter-web")     testCompile("org.springframework.boot:spring-boot-starter-test")
}
```

## 10.2 Spring Boot CLI安装

Spring Boot CLI\(Command Line Interface\)是一个命令行工具，通过它你能够很快使用Spring生成原型。该工具允许你运行Groovy脚本，意味着你无需太多的样本文件代码即可拥有一个类Java语法的语言。--Not good

你不需在Spring Boot中使用CLI，但是它的确是初始生成Spring应用最快捷的方式。

### 手动安装

你可以从Spring软件仓库中下载Spring CLI的分发包：

* [spring-boot-cli-2.0.0.RELEASE-bin.zip](https://repo.spring.io/release/org/springframework/boot/spring-boot-cli/2.0.0.RELEASE/spring-boot-cli-2.0.0.RELEASE-bin.zip)

* [spring-boot-cli-2.0.0.RELEASE-bin.tar.gz](https://repo.spring.io/release/org/springframework/boot/spring-boot-cli/2.0.0.RELEASE/spring-boot-cli-2.0.0.RELEASE-bin.tar.gz)

快照版本 [snapshot distributions](https://repo.spring.io/snapshot/org/springframework/boot/spring-boot-cli/)也可使用

一旦下载完成，请阅读解压文件夹中的[INSTALL.txt](https://raw.github.com/spring-projects/spring-boot/v2.0.0.RELEASE/spring-boot-project/spring-boot-cli/src/main/content/INSTALL.txt)安装说明。概括来讲，zip压缩包解压开后的bin/目录下，有一个spring脚本（Windows下为spring.bat）。或者，你可以使用 `java -jar`命令来编译`.jar`文件（这个脚本可以判别你的环境变量classpath是否设置正确）

### 使用SDKMAN安装

SDKMAN!\(The Software Development Kit Manager\)能够管理不同版本的二进制SDK包，包括Groovy和Spring Boot CLI。通过 [sdkman.io](sdkman.io)即可获取，安装Spring Boot命令如下：

```bash
$ sdk install springboot
$ spring --version
Spring Boot v2.0.0.RELEASE
```

如果你希望使用CLI的新特性并且希望读取你编译的版本，使用以下命令：

```bash
$ sdk install springboot dev /path/to/spring-boot/spring-boot-cli/target/spring-boot-cli-2.0.0.RELEASE-
bin/spring-2.0.0.RELEASE/
$ sdk default springboot dev
$ spring --version
Spring CLI v2.0.0.RELEASE
```

以上的命令安装了一个Spring本地实例，叫做 `dev` 实例。该实例指向了你的目标编译地址，所以每次你重新编译Spring Boot时候，`spring`都会是最新的.

你可以运行以下命令：

```bash
$ sdk ls springboot
================================================================================
Available Springboot Versions
================================================================================
> + dev
* 2.0.0.RELEASE
================================================================================
+ - local version
* - installed
> - currently in use
================================================================================
```

### OSX Homebrew安装

如果你使用的Mac电脑并使用[Homebrew](http://brew.sh/)，你可以通过以下命令安装Spring CLI：

```bash
$ brew tap pivotal/tap
$ brew install springboot
```

Homebrew将把spring安装至 `/user/local/bin`目录下

> **注意**  
> 如果你没有看到spring程序，你的brew程序可能版本落后了，出现这种情况，请运行 `brew update`并再次尝试。

### MacPorts安装

如果你是Mac电脑并使用[MacPorts](http://www.macports.org/)，你可以通过以下命令安装Spring CLI：

```bash
sudo port install spring-boot-cli
```

### 命令行自动完成功能

Spring Boot CLI脚本包含了BASH和zsh shell终端的命令行自动完成功能。你可以在任意shell终端中`source`脚本（也叫做`spring`），或者将其放置在全局bash完成初始化工作。--Not good。在Debian系统中，全局脚本存放在`/shell-completion/bash`下，该目录下所有的脚本都是可执行脚本。例如，如果你安装过SDKMAN后可手工执行以下命令：

```bash
 $ . ~/.sdkman/candidates/springboot/current/shell-completion/bash/spring
$ spring <HIT TAB HERE>
  grab  help  jar  run  test  version
```

> **注意**  
> 如果你已经通过Homebrew或MacPorts安装过了Spring Boot CLI,命令行自动完成功能就自动注册在你的shell中了。

### Spring CLI 样例快速上手

你可以使用以下Web应用代码来测试你的安装是否成功。请根据以下代码，创建一个名称为`app.groovy`的文件：

```java
@RestController
class ThisWillActuallyRun {
     @RequestMapping("/")
     String home() {
          "Hello World!"
    } 
}
```

在Shell中运行以下命令：

```bash
spring run app.groovy
```

> **注意**  
> 第一次运行该应用程序会比较慢，因为需要下载依赖库。随后的运行就会快很多了。

在你喜欢的浏览器中打开[localhost:8080](localhost:8080)。你应该可以看到以下输出：

```bash
Hello World!
```

## 10.3 从Spring Boot早期版本升级

如果你需要从早期的Spring Boot发行版升级，请查阅[“migration guide” on the project wiki](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide),哪里提供了详细的升级说明。也可以查阅[release notes](https://github.com/spring-projects/spring-boot/wiki)来获取每个发行版值得注意的地方和新特性列表。

升级一个已存在的CLI安装包，请使用合适的包管理器命令（例如，`brew update`）或者，如果你手动安装的CLI，请查阅[standard instructions](#102-spring-boot-cli安装)，记得更新你的 `PATH` 环境变量，删除任何旧的无效引用。

