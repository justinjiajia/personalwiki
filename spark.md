


### Downloading and installing Spark




To [download Spark](https://spark.apache.org/downloads.html), we will select the latest stable release pre-built with Hadoop. By saying that Spark is built with Hadoop, it is meant that Spark is built with the dependencies of Hadoop, i.e., with the clients for accessing Hadoop (or HDFS, to be more precise).



Thus, if we use the version of Spark built with Hadoop 3.2 or later versions, we will be able to access HDFS file system of a cluster with the verison 3.2 or later versions of Hadoop via Spark (Hadoop 3.2 or later must still be installed on the instance).

```bash
$ wget https://downloads.apache.org/spark/spark-3.1.2/spark-3.1.2-bin-hadoop3.2.tgz
```

3.	Unpack the compressed file, and rename the extracted directory for easier reference. We give the active user the permission to make such changes in the “spark” directory. Give the ownership of the directory to the active user. Lastly, delete the compressed file to save the space

```bash
$ tar -xzf spark-3.1.2-bin-hadoop3.2.tgz
$ mv spark-3.1.2-bin-hadoop3.2 spark && rm spark-3.1.2-bin-hadoop3.2.tgz
$ cd spark && ls
```

```bash
LICENSE  R          RELEASE  conf  examples  kubernetes  python  yarn
NOTICE   README.md  bin      data  jars      licenses    sbin
```

`ls` lists the contents of the Spark directory.Let's briefly consider the names and purposes of some of the more important files and directories we see here that come with Spark:

```bash
LICENSE  R          RELEASE  conf  examples  kubernetes  python  yarn
NOTICE   README.md  bin      data  jars      licenses    sbin
```


**README.md**: contains short instructions for getting started with Spark.
**bin**: contains executable files that can be used to interact with Spark in various ways (e.g., the Spark shell).

**core**, **streaming**, **python**, ...: contains the source code of major components of the Spark project. (???seems in jars now)


**examples**: contains some helpful Spark standalone jobs that you can look at and run to learn about the Spark API.

Open the **.bashrc** file:

```bash
$ nano  ~/.bashrc
```


Set several environment variables as follows:

```bash
# Spark

export SPARK_HOME=/home/hadoop/spark
export PATH=$PATH:$SPARK_HOME/bin
export PATH=$PATH:$SPARK_HOME/sbin
```

Run `source  ~/.bashrc` to load the newly defined environment variables into the working environment.




2. Spark's Interactive Shells




Spark comes with interactive shells that enable ad hoc data analysis. Spark's shells allow us to interact with data that is distributed on disk or in memory across many machines, as most other shells, such as those in R, Python, and operating system shells like bash, let us manipulate data using the disk and memory on a single machine. Spark takes care of automatically distributing this processing.



The first step is to open up one of Spark's shells. To open the Scala version of the shell, type:

```bash
$ spark-shell
```

`spark-shell` defaults to run locally with as many worker threads as logical cores on your machine (i.e., `master = local[*]`).

In local mode, Spark uses the client process as the single executor in the cluster, and the number of threads specified determines how many tasks can be executed in parallel.

The Spark log messages indicates that `Spark context available as sc`, meaning that
**SparkContext** is already created for us as variable `sc` in this shell. **SparkContext** is an object that coordinates the execution of Spark jobs.  Go ahead and type `sc` at the command line:

```scala
scala> sc
res0: org.apache.spark.SparkContext = org.apache.spark.SparkContext@796affb8
```

The shell prints the string form of the **SparkContext** object. It is simply its name plus the hexadecimal address of the object in memory.


As an object, **SparkContext** has methods associated with it. We can see what those methods are by typing the name of a variable, followed by a period, followed by <kdb>Tab</kdb>:




```scala
scala> sc.[\t]
```


The SparkContext has a long list of methods. But the ones that we're going to use most often allow us to create Resilient Distributed Datasets, or RDDs. RDDs are Spark's fundamental abstraction for distributed data and computation. In Spark, we express our computation through operations on RDDs.


When we use an EC2 instance with 2 logical cores (e.g., c4.large), it can be verified that 2 threads have been spawned by running:

```Scala
scala> sc.getConf.getAll
res1: Array[(String, String)] = Array((spark.repl.class.outputDir,/tmp/spark-6c5b79a0-5460-40b8-a67f-856bd1743468/repl-9f2c4d88-f97a-495a-ba38-8d35f59278d4), (spark.driver.host,ip-172-31-30-164.ec2.internal), (spark.home,/home/hadoop/spark), (spark.executor.id,driver), (spark.app.startTime,1623654216689), (spark.repl.class.uri,spark://ip-172-31-30-164.ec2.internal:43781/classes), (spark.app.name,Spark shell), (spark.sql.catalogImplementation,hive), (spark.app.id,local-1623654218362), (spark.jars,""), (spark.master,local[*]), (spark.submit.pyFiles,""), (spark.submit.deployMode,client), (spark.ui.showConsoleProgress,true), (spark.driver.port,43781))

scala> sc.defaultParallelism
res2: Int = 2
```



We can use the `--master` option to specify the master URL for a distributed cluster, or `local[N]` to run locally with $N$ threads as shown below<sup><a href="#footnote1">1</a></sup>:

```bash
$ spark-shell --master local[4]
```

To exit the shell, press <kbd>Ctrl</kbd>+<kbd>D</kbd> or type:

```Scala
scala> :q
```
or

```Scala
scala> :quit
```

If this is your first time using the Spark shell, run the `:help` command to list available commands in the shell. `:history` and `:h?` can be helpful for finding the names that you gave to variables or functions that you wrote during a session but can't seem to find at the moment. `:paste` can help you correctly insert code from the clipboard.



To open the Python version of the Spark shell, which is referred to as the PySpark Shell, go into your Spark directory and type:

```bash
$ pyspark --master local[4]
```

```python3
>>> sc.getConf().getAll()
[('spark.app.id', 'local-1623655224134'), ('spark.driver.host', 'ip-172-31-30-164.ec2.internal'), ('spark.executor.id', 'driver'), ('spark.app.name', 'PySparkShell'), ('spark.sql.catalogImplementation', 'hive'), ('spark.rdd.compress', 'True'), ('spark.driver.port', '46371'), ('spark.app.startTime', '1623655222961'), ('spark.serializer.objectStreamReset', '100'), ('spark.sql.warehouse.dir', 'file:/home/hadoop/spark-warehouse'), ('spark.submit.pyFiles', ''), ('spark.submit.deployMode', 'client'), ('spark.ui.showConsoleProgress', 'true'), ('spark.master', 'local[4]')]

>>> sc.defaultParallelism
4
```


To exit the PySpark shell, press <kbd>Ctrl</kbd>+<kbd>D</kbd> or type:

```python
>>> quit()
```


### Starting a Standalone Cluster


Spark applications run as independent sets of processes on a cluster, coordinated by the SparkContext object in the **driver program**. Specifically, the SparkContext can connect to several types of cluster managers (either Spark's own standalone cluster manager, Mesos or YARN) to run applications on a cluster.




In this section, we will first experiment the Spark's standalone deploy mode. We can launch a standalone cluster either manually, by starting a master and workers by hand, or use the provided [launch scripts](https://spark.apache.org/docs/latest/spark-standalone.html#cluster-launch-scripts).




To launch a Spark standalone cluster, we should create a file called **workers** in the **$SPARK_HOME** directory, which contains the hostnames of all the machines where we intend to start Spark workers.


```bash
$ cd $SPARK_HOME/conf
$ cp workers.template workers
$ nano workers
```

The **workers** file (similar to a Hadoop **workers** file) should contain a list of worker hostnames, each on a separate line (here, to avoid insufficient memory issue, we don't use the master node as a worker).



```
worker1
worker2
worker3
...
```

????If you need more workers per machine, you can set the *SPARK_WORKER_INSTANCES* environment variable to the number of workers you want on each machine.



Execute the **sshconf.sh** script to set SSH connections:

```bash
$ ./sshconf.sh
```


Then, we can launch or stop the cluster with the following shell scripts, based on Hadoop's deploy scripts, and available in `$SPARK_HOME/sbin`:



- **start-master.sh** - Starts a master instance on the machine the script is executed on.
- **start-workers.sh** - Starts a worker instance on each machine specified in the conf/workers file.
- **start-worker.sh** - Starts a worker instance on the machine the script is executed on.

- **start-all.sh** - Starts both a master and a number of workers as described above.

- **stop-master.sh** - Stops the master that was started via the sbin/start-master.sh script.

- **stop-worker.sh** - Stops all worker instances on the machine the script is executed on.

- **stop-workers.sh** - Stops all worker instances on the machines specified in the conf/workers file.

- **stop-all.sh** - Stops both the master and the workers as described above.

Execute **start-all.sh** in **$SPARK_HOME/sbin** on the machine we want to run the Spark master on. The **start-all.sh** script calls **start-master.sh** and then **start-workers.sh**. The **start-workers.sh** script will start all workers in tandem.

```bash
$ cd ~ && spark/sbin/start-all.sh
```

Once launched, we can find a **spark://HOST:PORT** URL (e.g., **spark://ip-172-31-18-255.ec2.internal:7077** if we use EC2 instances) on the master's web UI.



We can use the spark URL to connect additional workers to it, or pass as the `master` argument to SparkContext later.

To connect a new worker node to the master, we can ssh to that node and execute:

```bash
$ start-worker.sh spark://master:7077
```

**7077** is the default port for the Spark master to bind to. Once the worker is successfully added to the cluster, look at the master's web UI. We should see the new node listed there, along with its number of CPUs and memory.






### Running an Interactive Spark Shell against the Standalone Cluster

We can run Spark alongside our existing Hadoop cluster by just launching it as a separate service on the same machines.


Configure **$HADOOP_CONF_DIR/workers** and run the following commands to launch the HDFS service from the master node:

```bash
$ hdfs namenode -format
$ start-dfs.sh
```

`jps` prints:

```
Jps
Master
NameNode
```

```
Jps
Worker
DataNode
SecondaryNameNode
```

We prepare the same set of text files as before by sequentially executing the following commands:

```shell
$ cd ~
$ mkdir data
$ cd data
$ wget https://archive.org/download/encyclopaediabri31156gut/pg31156.txt
$ wget https://archive.org/download/encyclopaediabri34751gut/pg34751.txt
$ wget https://archive.org/download/encyclopaediabri35236gut/pg35236.txt
$ hadoop fs -put data  /
```


To run an interactive Spark shell on the cluster, issue the following command:

```bash
$ spark-shell --master spark://master:7077 --executor-memory 512m
```




Using the `--executor-memory` option, we can configure the size of memory each executor uses. Each application will have at most one executor on each worker, so this setting controls how much of that worker's memory the application will claim. By default, this setting is 1 GB. We decrease it to 512 MB as EC2 instances we are using may have limited resources.







[We can also pass an option `--total-executor-cores <numCores>` to control the total number of cores that spark-shell uses across all executors for an application. By default, this is unlimited; that is, the application will launch executors on every available node in the cluster. To run code that accesses external libraries, we need to include the JARs for these libraries on the classpath of Spark’s processes.] proofread later




The **SparkContext** object is already created for us as variable `sc`. If we use 3 workers with each running on an EC2 instance of 2 cores (c4.large):

```scala
sc.getConf.getAll
res0: Array[(String, String)] = Array((spark.executor.memory,512m), (spark.driver.port,38733), (spark.master,spark://master:7077), (spark.driver.host,master), (spark.home,/home/hadoop/spark), (spark.executor.id,driver), (spark.app.startTime,1623671132325), (spark.repl.class.uri,spark://master:38733/classes), (spark.app.id,app-20210614114533-0000), (spark.app.name,Spark shell), (spark.sql.catalogImplementation,hive), (spark.repl.class.outputDir,/tmp/spark-0549fe41-f885-4877-90b4-b35bf7d62c1b/repl-cb0c73b8-13ea-42ad-b71f-707b8a85d1d5), (spark.jars,""), (spark.submit.pyFiles,""), (spark.submit.deployMode,client), (spark.ui.showConsoleProgress,true))

scala> sc.defaultParallelism
res1: Int = 6
```


Text file RDDs can be created using SparkContext's `textFile` method. This method takes a URI for the file (either a local path on the machine, or a hdfs://, s3n://, etc。) and reads it as a collection of lines. Load the input data from HDFS and declare a variable called `lines`:


```scala
scala> val lines = sc.textFile("hdfs://master:9000/data")
lines: org.apache.spark.rdd.RDD[String] = hdfs://master:9000/data MapPartitionsRDD[1] at textFile at <console>:24
```

[Some notes](https://spark.apache.org/docs/latest/rdd-programming-guide.html) on reading files with Spark:

-	A path on HDFS should be specified as hdfs://hostname/path_on_HDFS where hostname is the value of the **fs.defaultFS** property specified in the **$HADOOP_CONF_DIR/core-site.xml** file. We can also find the right URL on the Hadoop Namenode’s web UI.



-	If using a path on the local filesystem, the file must also be accessible at the same path on worker nodes. So we need to copy the file to all workers.

- All of Spark's file-based input methods, including `textFile`, support running on directories, compressed files, and wildcards as well. For example, we can use textFile("/my/directory"), textFile("/my/directory/*.txt"), and textFile("/my/directory/*.gz").




There are a few things happening on this line that are worth going over. As we can see from the shell, the variable `lines` has a type of **RDD[String]**, even though we never specified that type information in our variable declaration. Type inference is a feature of the Scala programming language. Whenever possible, Scala figures out what type a variable has based on its context. In this case, Scala looks up the return type from the textFile function on the SparkContext object, sees that it returns an `RDD[String]`, and assigns that type to the `lines` variable.




Whenever we create a new variable in Scala, we must preface the name of the variable with either `val` or `var`. Variables that are prefaced with `val` are immutable, and cannot be changed to refer to another value once they are assigned, whereas variables that are prefaced with `var` can be changed to refer to different objects of the same type.



Once created, `lines` can be acted on by dataset operations. Spark supports pulling data sets into a cluster-wide in-memory cache. This is very useful when data is accessed repeatedly. We may want to persist the lines RDD in memory using the cache method for much faster access the next time we query it.



```scala
scala> lines.cache()
res1: lines.type = hdfs://master:9000/data MapPartitionsRDD[1] at textFile at <console>:24
```


We can use the `take` operation of an RDD to get the first 10 records to have a look:

```scala
scala> lines.take(10)
res2: Array[String] = Array(The Project Gutenberg EBook of Encyclopaedia Britannica, 11th Edition,, Volume 6, Slice 1, by Various, "", This eBook is for the use of anyone anywhere at no cost and with, almost no restrictions whatsoever.  You may copy it, give it away or, re-use it under the terms of the Project Gutenberg License included, with this eBook or online at www.gutenberg.net, "", "", Title: Encyclopaedia Britannica, 11th Edition, Volume 6, Slice 1)
```

Unfortunately this is not very readable because `take` returns an array and Scala simply prints the array with each element separated by a comma. To make it easier to read the contents of an array, we can use the foreach method in conjunction with `println` to print out each value in the array on its own line:




```scala
scala> lines.take(10).foreach(println)
The Project Gutenberg EBook of Encyclopaedia Britannica, 11th Edition,
Volume 6, Slice 1, by Various

This eBook is for the use of anyone anywhere at no cost and with
almost no restrictions whatsoever.  You may copy it, give it away or
re-use it under the terms of the Project Gutenberg License included
with this eBook or online at www.gutenberg.net


Title: Encyclopaedia Britannica, 11th Edition, Volume 6, Slice 1
```

The `foreach(println)` pattern is an example of a common functional programming pattern, where we pass one function (i.e., `println`) as an argument to another function (i.e., `foreach`) in order to perform some action.




Define a sequence of RDDs with the following transformations and build a dataset of (String, Int) pairs called `counts`:




```scala
scala> val counts = lines.flatMap(_.split(" ")).map(x=>(x,1)).reduceByKey(_ + _, 1).map(x=>x.swap).sortByKey(false,1).map(x=>x.swap)
counts: org.apache.spark.rdd.RDD[(String, Int)] = MapPartitionsRDD[7] at map at <console>:25
```

Refer to [Spark Programming Guide](http://spark.apache.org/docs/latest/rdd-programming-guide.html) for details of these transformations.


Then use the `saveAsTextFile` action to kick off a job to execute on a cluster:

```scala
scala> counts.saveAsTextFile("hdfs://master:9000/output")
```

We will see a console progress bar. For example, [Stage 2: shows the stage you are in now, and (14174 + 5) / 62500] is (numCompletedTasks + numActiveTasks) / totalNumOfTasksInThisStage]. The progress bar shows $numCompletedTasks / totalNumOfTasksInThisStage$.








We can also see how many lines in total are in the input data by issuing:

```scala
scala> lines.count
res5: Long = 114816
```
We may also want to add up the sizes of all the lines using the `map` and `reduce` operations as follows:

```scala
scala> lines.map(s => s.length).reduce((a, b) => a + b)
res6: Int = 6760426
```



### Running Compiled Applications on the Standalone Cluster


Apart from running interactively, Spark also supports the running of compiled applications.

Make a working directory **~/wordcount** in the local filesystem, and create a scala file called **WordCount.scala** in the **~/wordcount/src/main/scala** directory:

```bash
$ mkdir -p ~/wordcount/src/main/scala
$ nano ~/wordcount/src/main/scala/WordCount.scala
```


 Copy and paste the following code into the file and save the change:

new version


```Scala
/* WordCount.scala */

import org.apache.spark.sql.SparkSession

object WordCount {
  def main(args: Array[String]) {
      val spark = SparkSession.builder.appName("Word Count").getOrCreate()
      val input = spark.sparkContext.textFile("hdfs://master:9000/data").cache()
      val words = input.flatMap(line => line.split(" "))
      val counts = words.map(word => (word, 1)).reduceByKey{case (x, y) => x + y}
      counts.saveAsTextFile("hdfs://master:9000/output")
      spark.stop()
     }
}

```

Note that applications should define a `main()` method. We declare functions using the keyword `def`, and specify the types of the arguments to our function; here, we indicate that the `args` argument is a String array.

Unlike the earlier example with the Spark shell, which initializes its own SparkSession and SparkContext, we initialize a SparkSession as part of the program.



We call `SparkSession.builder` to construct a SparkSession, then set the application name (this will identify our application on the cluster manager's UI if we connect to a cluster), and finally call `getOrCreate` to get the SparkSession instance


Additional parameters exist for configuring how our application executes or adding code to be shipped to the cluster.


Then we can use all the methods as before to create RDDs (e.g., from a text file) and manipulate them. Finally, to shut down Spark, we can either call the `stop()` method on your SparkContext, or simply exit the application (e.g., with `System.exit(0)` or `sys.exit()`).

Our application depends on the Spark API `org.apache.spark.sql.SparkSession`.

---

old version

```scala

import org.apache.spark.SparkConf
import org.apache.spark.SparkContext
import org.apache.spark.SparkContext._

object WordCount {
    def main(args: Array[String]) {
        val conf = new SparkConf().setAppName("Word Count")
       val sc = new SparkContext(conf)
       val input = sc.textFile("hdfs://master:9000/wordcount/inputfiles")
        val words = input.flatMap(line => line.split(" "))
        val counts = words.map(word => (word, 1)).reduceByKey{case (x, y) => x + y}
       counts.saveAsTextFile("hdfs://master:9000/wordcount/outputfiles")
        sc.stop()
     }
}


```



We pass the SparkContext constructor a `SparkConf` which contains information about our application. This example shows the minimal way to initialize a SparkContext, where we only pass an application name, namely Word Count. This will identify our application on the cluster manager’s UI if we connect to a cluster. Additional parameters exist for configuring how our application executes or adding code to be shipped to the cluster.
 

After we have initialized a SparkContext, we can use all the methods as before to create RDDs (e.g., from a text file) and manipulate them. Finally, to shut down Spark, we can either call the `stop` method on your SparkContext, or simply exit the application (e.g., with `System.exit(0)` or `sys.exit()`).




---

It is very common that Java or Scala programs import and thus depend on several libraries. When we submit an application to Spark, it must ship with all libraries in its transitive dependency graph to the cluster. This includes not only the libraries the application directly depends on, but also their dependencies, their dependencies' dependencies, and so on.









Manually tracking the transitive dependency graph and submitting the associated JAR files would be extremely cumbersome. Instead, it's common practice to rely on a build tool to compile the code and produce a single large JAR containing the entire transitive dependency graph of an application. This is often called an uber JAR or an assembly JAR, and most Java or Scala build tools can produce this type of artifact.




The most popular build tools for Java and Scala are **Maven** and **sbt** (**Scala build tool**). Either tool can be used with either language, but **Maven** is more often used for Java projects and **sbt** for Scala projects.



To install sbt, please refer to http://www.scala-sbt.org/release/docs/Installing-sbt-on-Linux.html




```bash
# before start, make sure that a JDK is installed. sbt recommends the Oracle JDK 8 or OpenJDK 8.

sudo apt-get update  

# point to the debian distribution of sbt and add sbt to the sources list.
echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list  

# add the key of scala to the key list used by apt to authenticate packages
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2EE0EA64E40A89B84B2DF73499E82A75642AC823  

#  download the package lists from the repositories to ensure that the list of information on the newest version of packages and their dependencies are updated locally
sudo apt-get update  

# install sbt
sudo apt-get install sbt  
```

To compile code, we include an sbt configuration file, **build.sbt** under the **~/wordcount** directory, which explains the dependency. This file also adds a repository that Spark depends on:


```
name := "Word Count Project"

version := "1.0"

scalaVersion := "2.12.10"

libraryDependencies += "org.apache.spark" %% "spark-sql" % "3.1.2"
```


For `sbt` to work correctly, we'll need to layout **WordCount.scala** and **build.sbt** according to the typical directory structure (i.e., `sbt` expects user code to be in a src/main/scala directory relative to the project root):

```bash
$ find wordcount/
wordcount/build.sbt
wordcount/src
wordcount/src/main
wordcount/src/main/scala
wordcount/src/main/scala/WordCount.scala
```

Once that is in place, we can create a JAR package containing the application's code by changing the directory to ~/wordcount and issuing the following commands:



```bash
$ cd ~/wordcount && sbt package
```


As a result, we can see a **target** subdirectory and a **project** subdirectory are created in **~/wordcount**. The jar package that needs to be submitted to the cluster is in the **~/wordcount/target/scala-2.12** directory.




The spark-submit script in Spark’s bin directory is used to launch applications on a cluster. This script can support different cluster managers and deploy modes that Spark supports. To run our program with the standalone deploy mode, simply execute:


The [**spark-submit** script](https://github.com/apache/spark/blob/master/bin/spark-submit) in Spark's **bin** directory is used to launch applications on a cluster. This script supports different [cluster managers](http://spark.apache.org/docs/latest/cluster-overview.html#cluster-manager-types) and deploy modes through a uniform interface. To run our program with the standalone deploy mode, simply execute:

```bash
$ cd ~
$ spark-submit --class "WordCount" \
  --master spark://master:7077 \
  wordcount/target/scala-2.12/word-count-project_2.12-1.0.jar
```

Some of the commonly used options for the **spark-submit** script are:

-	`--class`: The entry point for your application

-	`--master`: The master URL for the cluster

-	`--deploy-mode`: For standalone clusters, Spark currently supports two deploy modes. In ***client*** mode (the [default one](https://github.com/apache/spark/blob/master/core/src/main/scala/org/apache/spark/deploy/SparkSubmit.scala#L242)), the **driver** is launched in the same process as the **client** that submits the application. In ***cluster*** mode, however, the **driver** is launched from one of the **Worker** processes inside the cluster, and the **client** process exits as soon as it fulfills its responsibility of submitting the application without waiting for the application to finish.







-	`--conf`: Arbitrary Spark configuration property in `key=value` format. For values that contain spaces wrap `"key=value"` in quotes.






-	application-jar: path to a bundled jar including the application and all dependencies. The URL must be globally visible inside of your cluster, for instance, an hdfs:// path or a file:// path that is present on all nodes.

-	application-arguments: Arguments passed to the main method of your main class, if any.


- `--jars`: any additional jars that your application depends on. Use comma as a delimiter (e.g. `--jars jar1,jar2`)


Python version can also be found https://spark.apache.org/docs/latest/quick-start.html

```bash
cd ~ && spark/sbin/stop-all.sh
```

### Running Spark on YARN

```bash
$ cd ~
$ spark-submit --class "WordCount" \
  --master yarn \
  --deploy-mode cluster \
  wordcount/target/scala-2.12/word-count-project_2.12-1.0.jar
```

Again, there are two deploy modes that can be used to launch Spark applications on YARN. In ***cluster*** mode, the Spark driver runs inside an application master process which is managed by YARN on the cluster, and the client initiates the application, and then periodically polls the application master for status updates and displays them in the console.

In ***client*** mode, the driver runs in the client process, and the application master is only used for requesting resources from YARN.

Unlike other cluster managers supported by Spark in which the master's address is specified in the `--master` parameter, in YARN mode the ResourceManager's address is picked up from the Hadoop configuration. Thus, the `--master` parameter is `yarn`.



### Processing Data with the Spark Shell in YARN client mode

The purpose of this section is to provide you a further venue to be acquainted with data analysis tasks Spark can do and Spark's APIs to do them.

The dataset we'll be analyzing was curated from a record linkage study that was performed at a German hospital in 2010. It contains several million pairs of patient records that were matched according to several criteria, such as the patient’s name, address, and birthday. Each matching field was assigned a numerical score from 0.0 to 1.0 based on how similar the strings were. The dataset has the following fields:








id_1, id_2, cmp_fname_c1, cmp_fname_c2, cmp_lname_c1, cmp_lname_c2, cmp_sex, cmp_bd, cmp_bm, cmp_by, cmp_plz, is_match




The first two fields are integer IDs that represent the patients that were matched in the record. The next nine values are (possibly missing; missing values are denoted by `?`) double values that represent match scores on different fields of the patient records, such as their names, birthdays, and location. The last field is a boolean value (TRUE or FALSE), indicating whether or not the pair of patient records represented by the line was a match. A sample of the input data looks as follows:




```
37291,53113,0.833333333333333,?,1,?,1,1,1,1,0,TRUE
39086,47614,1,?,1,?,1,1,1,1,1,TRUE
70031,70237,1,?,1,?,1,1,1,1,1,TRUE
49451,90407,1,?,1,?,1,1,1,1,0,TRUE
39932,40902,1,?,1,?,1,1,1,1,1,TRUE
```


Let's use the following commands to pull the data from an online repository to the local file system:

```bash
$ mkdir linkage
$ cd linkage
$ wget -O - http://bit.ly/1Aoywaq > donation.zip
$ sudo apt install unzip
$ unzip donation.zip
$ unzip 'block_*.zip'
```

The dataset is spread across 10 CSV files. To explore data examples to get some idea about the structure of this dataset, type:

```bash
$ cat block* | head -n20
```

It can be observed that these CSV files each have a header row.


Create a directory for the data in HDFS and copy the CSV files from the data set there:

```bash
$ hadoop fs -mkdir /linkage
$ hadoop fs -put block_*.csv /linkage
```

Because spark-shell is a client process used for interactive queries,
the **driver** is running in it, submitting jobs and communicating with executors. So it supports only client-deploy mode.

Launch spark-shell with YARN in client mode:

```
$ spark-shell --master yarn --deploy-mode client
```

In the spark shell, load the data by issuing:

```scala
scala> val rawblocks = sc.textFile("hdfs://master:9000/linkage")
```

There are a couple of issues with the data that we need to address before we begin our analysis. First, the CSV files each contain a header row that we want to filter out from our subsequent analysis. We can use the presence of the `"id_1"` string in the row as the filter condition, and write a Scala function that tests for the presence of that string. Type `:paste` to enter the paste mode, and paste the following code (the following code without being prefaced with `scala>` should all be inserted in this way):





```scala
def isHeader(line: String): Boolean = { line.contains("id_1") }
```



The return type for the function is declared by the keyword Boolean following a colon right after the argument list. Click <kbd>Ctrl</kbd>+<kbd>D</kbd> to finish. Then we apply the `isHeader` function to the millions of linkage records represented by the `rawblocks` RDD:







```scala
scala> val noheader = rawblocks.filter(x => !isHeader(x))
```


We can use the take operation of an RDD to get the first 10 records to have a look. To make it easier to read the contents of an array, we can use the `foreach` method in conjunction with `println` to print out each value in the array on its own line:



```scala
scala> noheader.take(10).foreach(println)
```





To make it a bit easier to analyze these records, we want to parse them into a structured format that converts the different fields into the correct data type. We use the split function to break up the components of line, returning an `Array[String]`. Then we convert the individual elements of each array to the appropriate type using Scala's type conversion functions, such as `toInt`, `toDouble` and so on.






However, there are `?` entries (representing missing values) in the fields for match scores that should be double-valued, and the original toDouble method doesn't know how to convert `?` to a Double. It requires us to write a function that will return a NaN value whenever a `?` is encountered:







```scala
def toDouble(s: String) = { if ("?".equals(s)) Double.NaN else s.toDouble }
```

Scala provides a convenient syntax for creating a simple record type, called case class, that allows us to address its fields by name. Let's declare a case class `MatchData` for our record linkage data:







```scala
case class MatchData(id1: Int, id2: Int, scores: Array[Double], matched: Boolean)
```


Now we can define our parse method to return an instance of our `MatchData` case class:

```scala
def parse(line: String) = {
    val pieces = line.split(',')
    val id1 = pieces(0).toInt
    val id2 = pieces(1).toInt
    val scores = pieces.slice(2, 11).map(toDouble)
    val matched = pieces(11).toBoolean
    MatchData(id1, id2, scores, matched)
}
```

Let's apply this parsing function to the data by calling the `map` function on the `noheader` RDD, and cache the generated RDD in memory by calling the `cache` method:



```scala
scala> val parsed = noheader.map(line => parse(line))
scala> parsed.cache()
```

Let's start out by creating a simple histogram to count how many of the MatchData records in `parsed` have a value of true or false for the matched field. The `countByValue` action performs on a RDD of type-T records and returns the results as a `Map[T,Long]`. As Scala's `Map` class does not have methods for sorting its contents, we convert the resulted Map into a Scala `Seq` type, which provide support for sorting:







```scala
scala> val matchCountsSeq = parsed.map(md => md.matched).countByValue().toSeq
```



The `matchCountsSeq` sequence is made up of elements of type `(String, Long)`, and we can use the sortBy method to control which of the indices we use for sorting (here, we use the positional functions, starting from `_1`; `_2` thereby contains the number of records with the matched field being either true or false). By default, the `sortBy` function sorts numeric values in ascending order, but it's often more useful to look at the values in descending order. We can reverse the sort order of any type by calling the `reverse` method on the sequence before we print it out:



```scala
scala> matchCountsSeq.sortBy(_._2).reverse.foreach(println)
```


The `stats` action provides us with exactly the summary statistics about the values in the RDD. The missing NaN values that we are using as placeholders in the scores arrays, however, will trip up Spark's summary statistics. Spark does not currently have a nice way of excluding and/or counting up the missing values for us, so we have to filter them out manually using the isNaN function from Java's `Double` class. We use Scala's Range construct to create a loop that would iterate through each index value and compute the statistics for the column in the scores array:



```scala
import java.lang.Double.isNaN
val stats = (0 until 9).map(i => { parsed.map(md => md.scores(i)).filter(!isNaN(_)).stats()})
```



<sup>[1](#footnote1)</sup> You can specify more threads than available CPU cores. That way, CPU cores can be better utilized. Although it depends on the complexity of your jobs, multiplying the number of CPU cores by two or three gives you a good starting point for this parameter (for example, for a machine with quad-core CPUs, set the number of threads to a value between 8 and 12)
