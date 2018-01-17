#  Eclipse+Maven+Scala Project+Spark | 编译并打包wordcount程序

<font size=2>学习用Eclipse+Maven来构建并打包一个简单的单词统计的例程。
# <font size=4>**第一步 在EclipseIDE中安装Scala插件**
<font size=2>**在Eclipse中安装Scala插件**
![这里写图片描述](http://img.blog.csdn.net/20180112175944862)
![这里写图片描述](http://img.blog.csdn.net/20180112180021386)
![这里写图片描述](http://img.blog.csdn.net/20180112180033498)
# <font size=4>**第二步 创建Scala Project**
<font size=2>**创建Scala 项目**
![这里写图片描述](http://img.blog.csdn.net/20180112180154489)
![这里写图片描述](http://img.blog.csdn.net/20180112180202863)
![这里写图片描述](http://img.blog.csdn.net/20180112180214372)
# <font size=4>**第三步 给Scala项目注入maven依赖**
<font size=2>**将Scala 项目 转为 Maven 项目**
![这里写图片描述](http://img.blog.csdn.net/20180112180313033)
![这里写图片描述](http://img.blog.csdn.net/20180112180409445)
![这里写图片描述](http://img.blog.csdn.net/20180112180421704)
![这里写图片描述](http://img.blog.csdn.net/20180112180447093)
`pom.xml`

	<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
		<modelVersion>4.0.0</modelVersion>
		<groupId>com.elon33.scala</groupId>
		<artifactId>WordCount</artifactId>
		<version>0.0.1-SNAPSHOT</version>
	
		<dependencies>
			<dependency>
				<groupId>org.apache.spark</groupId>
				<artifactId>spark-core_2.11</artifactId>
				<version>2.2.1</version>
				<scope>provided</scope>
			</dependency>
		</dependencies>
	
		<build>
			<sourceDirectory>src</sourceDirectory>
			<plugins>
				<plugin>
					<artifactId>maven-compiler-plugin</artifactId>
					<version>3.5.1</version>
					<configuration>
						<source>1.8</source>
						<target>1.8</target>
					</configuration>
				</plugin>
	
				<plugin>
					<groupId>net.alchim31.maven</groupId>
					<artifactId>scala-maven-plugin</artifactId>
					<version>3.3.1</version>
				</plugin>
			</plugins>
		</build>
	</project>
# <font size=4>**第四步 设置Scala Compiler 以及修改Scala Libarary Container版本**
<font size=2>当设置完`pom.xml`，我们可以看到有一些错误出些，主要错误来源于编译器交叉编译，Scala源码包版本不对引起的。
![这里写图片描述](http://img.blog.csdn.net/20180112181848044)
<font size=2>在这个项目中，从pom.xml中可以观察到spark版本是`spark-core_2.11`，因此Maven Dependencies中已经集成了`Scala2.11`，因此可以通过指定编译器版本和源码包版本解决Errors。
![这里写图片描述](http://img.blog.csdn.net/20180112183111819)
![这里写图片描述](http://img.blog.csdn.net/20180112183443951)
# <font size=4>**第五步 Maven 编译打包**
<font size=2>通过对项目进行 Maven Install 可以得到可运行的jar包
![这里写图片描述](http://img.blog.csdn.net/20180112183606972)
![这里写图片描述](http://img.blog.csdn.net/20180117131653204)
![这里写图片描述](http://img.blog.csdn.net/20180117131706440)
![这里写图片描述](http://img.blog.csdn.net/20180112183702525)
<font size=2>编译好的jar包中包含的class文件
![这里写图片描述](http://img.blog.csdn.net/20180112184155650)
# <font size=4>**第六步 Spark 集群上运行**
<font size=2>将jar包发送到Spark集群上运行

	spark-submit --class com.elon33.wordcount WordCount-0.0.1-SNAPSHOT.jar ../opt/modules/spark-2.2.1-bin-hadoop2.7/README.md ./wordcounts

# <font size=4>**第七步 计数结果**
<font size=2>单词程序的统计结果

	[elon@hadoop scala]$ cd wordcounts/
	[elon@hadoop wordcounts]$ ls
	part-00000  _SUCCESS
	[elon@hadoop wordcounts]$ cat part-00000 
	(package,1)
	(For,3)
	(Programs,1)
	(processing.,1)
	(Because,1)
	(The,1)
	(page](http://spark.apache.org/documentation.html).,1) 
	......

<font size=2>参考资料：
[1].Using Scala IDE on Maven projects [http://scala-ide.org/docs/tutorials/m2eclipse/](http://scala-ide.org/docs/tutorials/m2eclipse/)