




```java
import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class WordCount {

  public static class TokenizerMapper
       extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
      StringTokenizer itr = new StringTokenizer(value.toString());
      while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
      }
    }
  }

  public static class IntSumReducer
       extends Reducer<Text, IntWritable, Text, IntWritable> {

    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                       Context context
                       ) throws IOException, InterruptedException {
      int sum = 0;
      for (IntWritable val : values) {
        sum += val.get();
      }
      result.set(sum);
      context.write(key, result);
    }
  }

  public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    Job job = Job.getInstance(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(args[0]));
    FileOutputFormat.setOutputPath(job, new Path(args[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
  }
}

```

## Build Java MapReduce Application in EC2 Instances


### Download Some Data


We will download a bunch of books from **gutenberg.org**.

First let's create a directory and write a short script to download a few dozen books into it:

```bash
$ mkdir data && nano ~/data/download_books.sh
```


```bash
#!/usr/bin/env bash

this="${BASH_SOURCE-$0}"
path=$(cd -P -- "$(dirname -- "$this")" && pwd -P)

for i in {1391..1400}
do
    wget "https://www.gutenberg.org/files/$i/$i.txt" -O "$path/$i.txt"
done
```


Save the script and exit **nano**. Then make sure it is executable and start downloading the books (some of them won't be available in .txt format, that's fine):

```bash
$ chmod +x ~/data/download_books.sh && ./data/download_books.sh
$ rm ~/data/download_books.sh
```



Once the download is finished, you can list the files in the directory with: `ls -lh data` And you can check the total size of in the directory: `du -sh data`.




Compile WordCount.java


Before the Java virtual machine (JVM) can run a Java program, the program's Java source code must be compiled into bytecode, which is a platform independent version of machine code. Use the **javac** compiler to compile **WordCount.java**:


```bash
 $ javac -cp `hadoop classpath` WordCount.java
 ```


 We can find three `.class` files produced by the Java compiler (i.e., **WordCount.class**, **WordCount$IntSumReducer.class**, and **WordCount$TokenizerMapper.class**). Use the `jar` tool to bundle the bytecode class files into a Java Archive (JAR) file executable on the JVM<sup><a href="#footnote1">1</a></sup>:


 ```bash
$ jar -cf wordcount.jar WordCount*.class
```

We can use the following command to view the content of the wordcount.jar file to make sure that the three .class files are in the root directory of the JAR file:

```bash
$ jar -tf wordcount.jar
```






When developing Hadoop applications, it is common to debug and test the application locally.




We can create a configuration file **~/conf/hadoop-local.xml** for this purpose.

```xml
<?xml version="1.0"?>

<configuration>
	<property>
		<name>fs.defaultFS</name>
		<value>file:///</value>
	</property>

	<property>
		<name>mapreduce.framework.name</name>
		<value>local</value>
	</property>

</configuration>
```

With this setup, it is easy to use any configuration with the `-conf` or `--config` command-line switch.
For example, the following command shows a directory listing on the HDFS server
running in pseudodistributed mode on localhost:

 ```bash
$ hadoop fs -conf conf/hadoop-local.xml -ls .
```


 ```bash
$ hadoop --config conf jar wordcount.jar WordCount data output
```






## Build Java MapReduce Application in EMR


Amazon Elastic MapReduce (EMR) now supports a public EMR artifact repository to help developers build applications based on the EMR distribution for Apache Hadoop and Apache Hive using **Apache Maven**.

The EMR cluster is based on Red Hat Enterprise Linux (RHEL) and thereby missing a few tools we'll need to deploy and run Mapreduce applications (e.g., **Apache Maven**).

Optional: Because EMR is based on Red Hat Linux, we can first use the **yum** package manager to update all installed packages in the local system. Switch on the `-y` option to automatically choose `yes` for future questions:

 ```bash
$ sudo yum -y update
```

Then add an external repository to `yum` and configures it:

```bash
$ sudo wget https://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo
$ sudo sed -i s/\$releasever/6/g /etc/yum.repos.d/epel-apache-maven.repo
```

And lastly, installs **Apache Maven** by running:

```bash
$ sudo yum install -y apache-maven
```


Unfortunately, installing Maven in this way will also install Java v1.7, so you will need to switch back to Java v1.8. To do that, use the `alternatives` command for java (runtime):

```bash
$ sudo alternatives --config java
There are 2 programs which provide 'java'.
 Selection Command
-----------------------------------------------
 1 /usr/lib/jvm/jre-1.8.0-openjdk.x86_64/bin/java
*+ 2 /usr/lib/jvm/jre-1.7.0-openjdk.x86_64/bin/java
Enter to keep the current selection[+], or type selection number: 1
```

And for the Java compiler:

```bash
$ sudo alternatives --config javac
There are 2 programs which provide 'javac'.
 Selection Command
-----------------------------------------------
 1 /usr/lib/jvm/java-1.8.0-openjdk.x86_64/bin/javac
*+ 2 /usr/lib/jvm/java-1.7.0-openjdk.x86_64/bin/javac
Enter to keep the current selection[+], or type selection number: 1
```

Check that everything went well:

```bash
$ mvn -version
```



```bash
$ mvn archetype:generate -DgroupId=example -DartifactId=wordcount -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.1 -DinteractiveMode=false
```

```bash
$ tree -d wordcount
wordcount
└── src
    ├── main
    │   └── java
    │       └── example
    └── test
        └── java
            └── example
 ```

 ```bash
$ nano ~/wordcount/src/main/java/example/WordCount.java
 ```

Then copy and paste the Java source code for the word count application.

```bash
$ nano ~/wordcount/pom.xml
```

The EMR artifact repository hosts the same optimized versions of libraries and dependencies that are available with specific Amazon EMR release versions, ensuring that artifacts used in building applications against the EMR stack are compatible with the runtime libraries on the EMR cluster.

To use the right repository, we need to add its URL to the Maven settings file or to a specific project's pom.xml configuration file. We can then specify the dependencies in your project configuration. Visit [Checking dependencies using the Amazon EMR artifact repository](https://docs.amazonaws.cn/en_us/emr/latest/ReleaseGuide/emr-artifact-repository.html) for detailed information.




Currently, EMR 6.3.0 Releases Repository is not available. Use that for 6.2.0 instead:


We can remove the Maven generated **pom.xml** file and then create a new one with the XML below:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>example</groupId>
	<artifactId>wordcount</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>wordcount</name>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>

	<repositories>
		<repository>
			<id>emr-6.2.0-artifacts</id>
			<name>EMR 6.2.0 Releases Repository</name>
			<releases>
				<enabled>true</enabled>
			</releases>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
			<url>https://s3.us-east-1.amazonaws.com/us-east-1-emr-artifacts/emr-6.2.0/repos/maven/</url>
		</repository>
	</repositories>

	<dependencies>
		<dependency>
			<groupId>org.apache.hadoop</groupId>
			<artifactId>hadoop-client</artifactId>
			<version>3.2.1</version>
		</dependency>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.11</version>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.8.1</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
				</configuration>
			</plugin>

		</plugins>
	</build>

</project>
```

You can make sure
that this dependency has been successfully added by running:


```bash
$ cd ~/wordcount && mvn dependency:tree


```

the command `mvn package` will execute the package phase and all prior phases of the default lifecycle. But you can skip certain prior phases by using the `maven.*.skip` property, e.g., `mvn package -Dmaven.test.skip=true`.

 The `clean` lifecycle handles the deletion of temporary files and generated artifacts from the target directory.


```bash
mvn clean package

...
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ wordcount ---
[INFO] Building jar: /home/hadoop/wordcount/target/wordcount-0.0.1-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
...
```

The package suffix after the mvn command is a Maven phase that
compiles Java code and packages it into the JAR file.

The SNAPSHOT qualifier in the project’s version carries a special meaning. It
indicates that the project is in a development stage. When a project uses a
SNAPSHOT dependency, every time the project is built, Maven will fetch and
use the latest SNAPSHOT artifact.


```bash
$ jar -tf ~/wordcount/target/wordcount-0.0.1-SNAPSHOT.jar
META-INF/
META-INF/MANIFEST.MF
example/
WordCount$TokenizerMapper.class
WordCount$IntSumReducer.class
WordCount.class
example/App.class
META-INF/maven/
META-INF/maven/example/
META-INF/maven/example/wordcount/
META-INF/maven/example/wordcount/pom.xml
META-INF/maven/example/wordcount/pom.properties
```

local mode:

```bash
$ hadoop --config conf jar ~/wordcount/target/wordcount-0.0.1-SNAPSHOT.jar WordCount data output
```

distributed mode:

```bash
$ hadoop jar ~/wordcount/target/wordcount-0.0.1-SNAPSHOT.jar WordCount /data /output
```



 On your journey to deployment, one of your options is to package your application in an uber JAR. An uber JAR (also known as super JAR or fat JAR) is a JAR above (literally the German translation of über) the other JARs. The uber JAR contains all the classes needed by your application, regardless of the number of JARs you had on your class path. In other words, it contains most, if not all, the dependencies of your application. Logistics are then uber simplified because you will handle only one JAR.

To build the uber JAR, you will use the Maven Shade build plugin. You can read its full documentation at http://maven.apache.org/plugins/maven-shade-plugin/index.html


## Footnote


<sup>[1](#footnote1)</sup> Java Archive (JAR) is a package file format typically used to aggregate many Java class files and associated metadata and resources (text, images, etc.) into one file to distribute applications on the Java platform. For details, see [Using JAR Files: The Basics](https://docs.oracle.com/javase/tutorial/deployment/jar/basicsindex.html).


<sup>[2](#footnote2)</sup>
