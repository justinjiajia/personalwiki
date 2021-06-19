




```bash

 $ javac -cp `hadoop classpath` WordCount.java
 ```


 We can find three .class files produced by the Java compiler. They are WordCount.class, WordCount$IntSumReducer.class, and WordCount$TokenizerMapper.class. Use the jar tool to bundle the bytecode class files into a jar file executable on the JVM:

 ```bash

$ jar -cf wordcount.jar WordCount*.class
```

When developing Hadoop applications, it is common to debug and test the
application locally

Then we write a short script to download a few dozen books with vim download_books.sh or nano download_books.sh:

```bash
#!/usr/bin/env bash

for i in {1380..1400}
do
    wget "http://www.gutenberg.org/files/$i/$i.txt"
done
```


Save the script, then make sure it is executable: chmod +x download_books.sh. We can now start downloading the books (some of them won't be available in .txt format, that's fine):






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






### build Java Mapreduce application on EMR


Out of the box, the Amazon Elastic MapReduce (EMR) cluster is missing a few tools.
It is based on Red Hat Enterprise Linux (RHEL). You will need to deploy and run
your application. In this section, you will first connect to the server, update it, and install the missing tools


Because EMR is based on Red Hat Linux, you can use the yum package manager to
update the system:

Optionally, first update all installed packages, with `-y` switch to automatically choose "yes" for future questions:

 ```bash
$ sudo yum -y update
```

Then add an external repository to yum and configures it:

 ```bash
$ sudo wget https://repos.fedorapeople.org/repos/dchen/apache-maven/ epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo
$ sudo sed -i s/\$releasever/6/g /etc/yum.repos.d/epel-apache-maven.repo
```

and lastly, installs **Apache Maven**:

 ```bash
$ sudo yum install -y apache-maven
```


Unfortunately, installing Maven in this way will also install Java v1.7, so you will need to switch back to Java v1.8. To do that, use the alternatives command for java (runtime):

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
$ nano ~/wordcount/src/main/java//example/WordCount.java
 ```

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

```bash
$ nano ~/wordcount/pom.xml
```



https://aws.amazon.com/about-aws/whats-new/2018/11/amazon-emr-now-supports-a-public-EMR-artifact-repository-for-maven-builds/

The EMR artifacts repository hosts the same optimized versions of libraries and dependencies that are available with specific Amazon EMR release versions, ensuring that artifacts used in building applications against the EMR stack are compatible with the runtime libraries on the EMR cluster.

[Checking dependencies using the Amazon EMR artifact repository](https://docs.amazonaws.cn/en_us/emr/latest/ReleaseGuide/emr-artifact-repository.html)


EMR 6.3.0 Releases Repository is not available. Use that for 6.2.0 instead:


. Putting repository information in the pom.xml   file

```xml

...
<name>wordcount</name>
<url>http://maven.apache.org</url>

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
 <version>3.8.1</version>
 <scope>test</scope>
</dependency>
</dependencies>
</project>
```

You can make sure
that this dependency has been successfully added by running:


```bash
$ cd ~/wordcount && mvn dependency:tree


```

the command `mvn
package` will execute the package phase and all prior phases of the default lifecycle. But you can skip certain prior phases by using the `maven.*.skip` property, e.g., `mvn package –Dmaven.test.skip=true`

 The `clean` lifecycle handles the deletion of
temporary files and generated artifacts from the
target directory


```bash
mvn clean package

...
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ wordcount ---
[INFO] Building jar: /home/hadoop/wordcount/target/wordcount-1.0-SNAPSHOT.jar
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
$ jar -tf wordcount/target/wordcount-1.0-SNAPSHOT.jar
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

```bash
$ hadoop --config conf jar ~/wordcount/target/wordcount-1.0-SNAPSHOT.jar WordCount data output
```



 On your journey to deployment, one of your options is to package your application in
an uber JAR. As you may recall from chapter 5, an uber JAR is an archive containing
all the classes needed by your application, regardless of the number of JARs you had
on your class path. The uber JAR contains most, if not all, the dependencies of your
application. Logistics are then uber simplified, as you will handle only one JAR. Let’s
build an uber JAR with Maven.
