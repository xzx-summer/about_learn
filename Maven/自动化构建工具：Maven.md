# 自动化构建工具：Maven



## 目前掌握的技术

![image-20210321204628082](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123553.png)



## 目前的技术在开发中存在的问题【why】

①一个项目就是一个工程

​	如果项目非常庞大，就不适合使用package来划分模块，最好是每个模块对应一个工程，利于分工协作。

​	借助一Maven就可以将一个项目拆分成多个工程。

②项目中需要的jar包必须手动”复制“、”粘贴“到WEB-INF/lib目录下

​	带来的问题是：同样的jar包文件重复出现在不同的项目工程中，一方面浪费存储空间，另外也让工程比较臃肿。

​	借助于Maven，可以将jar包仅仅保存在”仓库“中，有需要使用的工程”引用“这个文件接口，并不需要真的把jar包复制过来。

③jar包需要别人替我们准备好，或到官网下载

​	不同技术的官网提供jar包下载的形式是五花八门的。

​	有些技术的官网就是通过Maven或SVN等专门的工具来提供下载的。

​	如果是以非正规的方式下载的jar包，那么其中的内容很可能也是不规范的。

​	借助于Maven可以以一种规范的方式下载jar包，因为所有知名框架或第三方工具的jar包已经按照统一的规范存放在了Maven的中央仓库中。

​	以规范的方式下载的jar包，内容也是可靠的。

​	==Tips：==”统一的规范“不仅是对IT开发领域非常重要，对于整个人类社会都是非常重要的。

④一个jar包依赖的其他jar包需要自己手动加入到项目中

​	FileUpload组件$\rightarrow$IO组件。commons-fileupload-1.3.jar依赖于commons-io-2.0.1.jar。

​	如果所有的jar包之间的依赖关系都需要程序员自己非常清楚的了解，那么就会极大的增加学习成本。

​	Maven会自动将被依赖的jar包导入进来。



## Maven是什么【what】

①Maven是一款服务于Java平台的自动化构建工具。

​	Make$\rightarrow$Ant$\rightarrow$Maven$\rightarrow$Gradle

②构建

​	【1】概念：以”Java源文件“、”框架配置文件“、”JSP“、”HTML“、”图片“等资源为”原材料“，去”生产“一个可以运行的项目的过程。（生产有编译，部署，搭建的意思）

​	【2】编译：Java源文件$\rightarrow$编译$\rightarrow$Class字节码文件$\rightarrow$交给JVM去执行。

​	【3】部署：一个BS项目最终运行的并不是动态Web工程本身，而是这个动态Web工程”编译的结果“。

​			生的鸡$\rightarrow$处理$\rightarrow$熟的鸡

​			动态Web工程$\rightarrow$编译、部署$\rightarrow$编译结果

![image-20210327185624309](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123554.png)

开发过程中，所有的路径或配置文件中配置的类路径等都是以编译结果的目录结构为标准的。

​			==Tips：==运行时环境![image-20210321221228281](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123555.png)

​						其实是一组jar包的引用，并没有把jar包本身复制到工程中，所以并不是目录。

P5：再看一遍

③构建过程中的各个环节

​	【1】清理：将以前编译得到的旧的class字节码文件删除，为下一次编译做准备

​	【2】编译：将Java源程序编程成class字节码文件

​	【3】测试：自动测试，自动调用junit程序

​	【4】报告：测试程序执行的结果

​	【5】打包：动态Web工程打war包，Java工程打jar包

​	【6】安装：Maven特定的概念——将打包得到的文件复制到“仓库”中的指定位置

​	【7】部署：将动态Web工程生成的war包复制到Servlet容器的指定目录下，使其可以运行

④自动化构建



## 安装Maven核心程序



## Maven的核心概念

①约定的目录结构

②POM

③坐标

④依赖

⑤仓库

⑥生命周期/插件/目标

⑦继承

⑧聚合



## 第一个Maven工程

①创建约定的目录结构

​	【1】根目录：工程名

​	【2】src目录：源码

​	【3】pom.xml文件：Maven工程的核心配置文件

​	【4】main目录：存放主程序

​	【5】test目录：存放测试程序

​	【6】java目录：存放Java源文件

​	【7】resources目录：存放框架或其他工具的配置文件

![image-20210327193014821](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123556.png)

②为什么要遵守约定的目录结构呢？

* Maven要负责我们这个项目的自动化构建，以编译为例，Maven要想自动进行编译，那么它必须知道Java源文件保存在哪里

* 如果我们自己自定义的东西想要让框架或工具知道，有两种方法

  * 以配置的方式明确告诉框架

  * 遵守框架内部已经存在的约定

  log4j.properties

  log4j.xml

* 约定>配置>编码



## 常用Maven命令

①常用命令

​	【1】mvn clean：清理

​	【2】mvn compile：编译主程序

​	【3】mvn test-compile：编译测试程序

​	【4】mvn test：执行测试

​	【5】mvn package：打包

​	【6】mvn install：安装

​	【7】mvn site：生成站点



## 关于联网问题

①Maven的核心程序中仅仅定义了抽象的生命周期，但是具体的工作必须由特定的插件来完成，而插件本身并不包含在Maven的核心程序中。

②当我们执行的Maven命令需要用到某些插件时，Maven核心程序会首先到本地仓库中查找。

③本地仓库的默认位置：[系统中当前用户的家目录]\\.m2\\repository

④Maven核心程序如果在本地仓库中找不到需要的插件，那么它会自动连接外网，到中央仓库下载

⑤如果此时无法连接外网，则构建失败

⑥修改默认本地仓库的位置可以让Maven核心程序到我们事先准备好的目录下查找插件

	1. 找到Maven解压目录\conf\settings.xml
 	2. 在settings.xml文件中找到==localRepository==标签
 	3. 将$<localRepository>/path/to/local/repo</localRepository>$从注释中取出
 	4. 将标签体内容修改为已经准备好的Maven仓库目录



## POM

①含义：Project Object Model 项目对象模型

​			DOM Document Object Model 文档对象模型

②pom.xml对Maven工程是核心配置文件，与构建过程相关的一切设置都在这个文件中进行配置，重要程度相当于web.xml对于动态Web工程



## 坐标

①数学中的坐标：

1. 在平面上，使用X，Y两个向量可以唯一的定位平面中的任何一个点
2. 在空间中，使用X，Y，Z三个向量可以唯一的定位空间中的任何一个点

②Maven的坐标

​	使用下面三个向量在仓库中唯一定位一个Maven工程

   * groupid：公司或组织域名倒叙+项目名

   * artifactid：模块名

   * version：版本

     gav指的就是Maven坐标的事情

③Maven工程的坐标与仓库中路径的对应关系

![image-20210327210357230](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123557.png)



## 仓库

①仓库的分类

 1. 本地仓库：当前电脑上部署的仓库目录，为当前电脑上所有Maven工程服务

 2. 远程仓库

    （1）私服：搭建在局域网环境中，为局域网范围内的所有Maven工程服务

    （2）中央仓库：架设在Internet上，为全世界所有Maven工程服务

    （3）中央仓库镜像：为了分担中央仓库的流量，提升用户的访问速度

②仓库中保存的内容：Maven工程

	1. Maven自身所需要的插件
 	2. 第三方框架或工具的jar包
 	3. 我们自己开发的Maven工程



## 依赖【初步】

①Maven解析依赖信息时会到本地仓库中查找被依赖的jar包

​	对于我们自己开发的Maven工程，使用mvn install命令安装后就可以进入仓库

②依赖的范围

![image-20210327213801839](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123558.png)

* compile范围依赖
  * 对主程序是否有效：有效
  * 对测试程序是否有效：有效
  * 是否参与打包：参与
  * 是否参与部署：参与
  * 典型例子：spring-core
* test范围依赖
  * 对主程序是否有效：无效
  * 对测试程序是否有效：有效
  * 是否参与打包：不参与
  * 是否参与部署：不参与
  * 典型例子：junit
* provided范围依赖
  * 对主程序是否有效：有效
  * 对测试程序是否有效：有效
  * 是否参与打包：不参与
  * 是否参与部署：不参与
  * 典型例子：servlet-api.jar

![image-20210327214551355](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123559.png)

![image-20210327214717541](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123600.png)



## 生命周期

①各个构建环节执行的顺序： 不能打乱顺序，必须按照既定的正确顺序来执行

②Maven的核心程序中定义了抽象的生命周期，生命周期中各个阶段的具体任务是由插件来完成的

③Maven核心程序为了更好的实现自动化构建，按照这一特点执行生命周期中的各个阶段：不论现在要执行生命周期中的哪一个阶段，都是从这个生命周期最初的位置开始执行

![image-20210327220135429](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123601.png)

![image-20210327220230559](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123602.png)

④插件和目标

	1. 生命周期的各个阶段仅仅定义了要执行的任务是什么
 	2. 各个阶段和插件的目标是对应的
 	3. 相似的目标由特定的插件来完成
 	4. 可以将目标看作“调用插件功能的命令”

![image-20210327220555256](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123603.png)



## 在eclipse中使用Maven

①Maven插件：eclipse内置

②Maven插件的设置

	1. installations：指定Maven核心程序的位置，不建议使用插件自带的Maven程序，而应该使用我们自己解压的那个
 	2. user settings：指定conf/settings.xml的位置，进而获取本地仓库的位置

③基本操作

	1. 创建Maven版的Java工程
 	2. 创建Maven版的Web工程
 	3. 执行Maven命令



## 依赖【高级】

①依赖的传递性

![image-20210328204104586](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123604.png)

	1. 好处：可以传递的依赖不必在每个模块工程中都重复声明，在“最下面”的工程中依赖一次即可
 	2. 注意：非compile范围的依赖不能传递。所以在各个工程模块中，如果有需要就得重复声明依赖

②依赖的排除

	1. 需要设置依赖排除的场合

![image-20210328205134190](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123605.png)

	2. 依赖排除的设置方式

![image-20210328205256875](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123606.png)

③依赖的原则

	1. 作用：解决模块工程之间的jar包冲突问题
 	2. 情景设定1：验证路径最短者优先原则

![image-20210328210115000](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123607.png)

 3. 情景设定2：验证路径相同时先声明者优先

    ![image-20210328210427654](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123608.png)

    先声明指的是dependency标签的声明顺序

④统一管理依赖的版本

​	1. 情景举例

![image-20210328210928391](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123609.png)

​	这里对Spring各个jar包的依赖版本都是4.0.0

​	如果需要统一升级为4.1.1，怎么办？手动逐一修改不可靠。

 2. 建议配置方式

    * 使用properties标签内使用自定义标签统一声明版本号

    ![image-20210328211606420](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123610.png)

    * 在需要统一版本的位置，使用${自定义标签名}引用声明的版本号

    ![image-20210328211630416](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123611.png)

 3. 其实properties标签配合自定义标签声明数据的配置并不是只能用于声明依赖的版本号，凡是需要统一声明后再引用的场合都可以使用

![image-20210328212006521](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123612.png)



## 继承

①现状

​		Hello依赖的junit：4.0

​		HelloFriend依赖的junit：4.0

​		MakeFriends依赖的junit：4.9

​		由于test范围的依赖不能传递，所以必然会分散在各个模块工程中，很容易造成版本不一致

②需求：统一管理各个模块工程中对junit依赖的版本

③解决思路：将junit依赖统一提取到“父”工程中，在子工程中声明junit依赖时不指定版本，以父工程中统一设定的为准，同时也便于修改

④操作步骤：

	1. 创建一个Maven工程作为父工程。注意：打包的方式pom

![image-20210328213317951](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123613.png)

​	2. 在子工程中声明对父工程的引用

![image-20210328213414041](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123614.png)

​	3. 把子工程的坐标中与父工程坐标中重复的内容删除

![image-20210328213557277](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123615.png)

​	4. 在父工程中统一管理junit的依赖

![image-20210328224622679](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123616.png)

​	5. 在子工程中删除junit依赖的版本号部分

![image-20210328224753298](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123617.png)

⑤注意：配置继承后，执行安装命令时要先安装父工程



## 聚合

①作用：一键安装各个模块工程

②配置方式：在一个“总的聚合工程”中配置各个参与聚合的模块

![image-20210328225313829](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210714123618.png)

③使用方式：在聚合工程的pom.xml上点右键$\rightarrow$run as$\rightarrow$maven install



## 依赖查找的网站

https://mvnrepository.com/artifact/net.jodah/expiringmap
