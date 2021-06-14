


### Downloading and installing Spark




To [download Spark](https://spark.apache.org/downloads.html), we will select the latest stable release pre-built with Hadoop. By saying that Spark is built with Hadoop, it is meant that Spark is built with the dependencies of Hadoop, i.e., with the clients for accessing Hadoop (or HDFS, to be more precise).

Thus, if we use the version of Spark built with Hadoop 3.2 or later, we will be able to access HDFS file system of a cluster with the version 3.2 or later versions of Hadoop via Spark (Hadoop 3.2 or later must still be installed on the instance).

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


Spark comes with interactive shells that enable ad hoc data analysis. Unlike most other shells, however, which let you manipulate data using the disk and memory on a single machine such as those in R, Python, and operating system shells like Bash, Spark’s shells allow you to interact with data that is distributed on disk or in memory across many machines, and Spark takes care of automatically distributing this processing.

The first step is to open up one of Spark’s shells. To open the Scala version of the shell, type:

```bash
$ spark-shell
```

`spark-shell` defaults to run locally with as many worker threads as logical cores on your machine (i.e., `master = local[*]`).


The Spark log messages indicates that ***Spark context available as sc***. This is a reference to the **SparkContext** object, which coordinates the execution of Spark jobs. Go ahead and type `sc` at the command line:

```scala
scala> sc
res0: org.apache.spark.SparkContext = org.apache.spark.SparkContext@3b68a50c
```


The shell prints the string form of the object, and for the **SparkContext** object, this is simply its name plus the hexadecimal address of the object in memory.

As an object, **SparkContext** has methods associated with it. We can see what those methods are in the spark shell by typing the name of a variable, followed by a period, followed by <kdb>Tab</kdb>:

```scala
scala> sc.[\t]
```
The SparkContext has a long list of methods.

When we use an EC2 instance with 2 logical cores, it can be verified that 2 worker threads have been spawned by running:

```Scala
scala> sc.getConf.getAll
res1: Array[(String, String)] = Array((spark.repl.class.outputDir,/tmp/spark-6c5b79a0-5460-40b8-a67f-856bd1743468/repl-9f2c4d88-f97a-495a-ba38-8d35f59278d4), (spark.driver.host,ip-172-31-30-164.ec2.internal), (spark.home,/home/hadoop/spark), (spark.executor.id,driver), (spark.app.startTime,1623654216689), (spark.repl.class.uri,spark://ip-172-31-30-164.ec2.internal:43781/classes), (spark.app.name,Spark shell), (spark.sql.catalogImplementation,hive), (spark.app.id,local-1623654218362), (spark.jars,""), (spark.master,local[*]), (spark.submit.pyFiles,""), (spark.submit.deployMode,client), (spark.ui.showConsoleProgress,true), (spark.driver.port,43781))

scala> sc.defaultParallelism
res2: Int = 2
```



We can use the `--master` option to specify the master URL for a distributed cluster, or `local[N]` to run locally with $N$ threads as shown below:

```bash
$ spark-shell --master local[2]
```

To exit the shell, press <kdb>Ctrl</kdb> + <kdb>D</kdb> or type:

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
$ pyspark --master local[1]
```

```python3
>>> sc.getConf().getAll()
[('spark.app.id', 'local-1623655224134'), ('spark.driver.host', 'ip-172-31-30-164.ec2.internal'), ('spark.executor.id', 'driver'), ('spark.app.name', 'PySparkShell'), ('spark.sql.catalogImplementation', 'hive'), ('spark.rdd.compress', 'True'), ('spark.driver.port', '46371'), ('spark.app.startTime', '1623655222961'), ('spark.serializer.objectStreamReset', '100'), ('spark.sql.warehouse.dir', 'file:/home/hadoop/spark-warehouse'), ('spark.submit.pyFiles', ''), ('spark.submit.deployMode', 'client'), ('spark.ui.showConsoleProgress', 'true'), ('spark.master', 'local[1]')]

>>> sc.defaultParallelism
1
```


To exit the PySpark shell, press <kdb>Ctrl</kdb> + <kdb>D</kdb> or type:

```python
>>> quit()
```


### Starting a Standalone Cluster


Spark applications run as independent sets of processes on a cluster, coordinated by the SparkContext object in the main program (called the driver program). Specifically, the SparkContext can connect to several types of cluster managers (either Spark's own standalone cluster manager, Mesos or YARN) to run applications on a cluster.

In this section, we will first experiment the Spark's own standalone deploy mode. We can launch a standalone cluster either manually, by starting a master and workers by hand, or use the provided [launch scripts](https://spark.apache.org/docs/latest/spark-standalone.html#cluster-launch-scripts).

To launch a Spark standalone cluster, we should create a file called **conf/workers** in the Spark directory, which must contain the hostnames of all the machines where we intend to start Spark workers.

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

If you need more workers per machine, you can set the *SPARK_WORKER_INSTANCES* environment variable to the number of workers you want on each machine.

Execute the **sshconf.sh** script to set SSH connections:

```bash
$ ./sshconf.sh
```



Once we've set up the SPARK_CONF_DIR/slaves and SSH connections, we can launch or stop the cluster with the following shell scripts, based on Hadoop's deploy scripts, and available in `$SPARK_HOME/sbin`:



- **start-master.sh** - Starts a master instance on the machine the script is executed on.
- **start-workers.sh** - Starts a worker instance on each machine specified in the conf/workers file.
- **start-worker.sh** - Starts a worker instance on the machine the script is executed on.

- **start-all.sh** - Starts both a master and a number of workers as described above.

- sbin/stop-master.sh - Stops the master that was started via the sbin/start-master.sh script.
sbin/stop-worker.sh - Stops all worker instances on the machine the script is executed on.
- sbin/stop-workers.sh - Stops all worker instances on the machines specified in the conf/workers file.

- **stop-all.sh** - Stops both the master and the workers as described above.

Execute **start-all.sh** in **$SPARK_HOME/sbin** on the machine we want to run the Spark master on. The **start-all.sh** script calls **start-master.sh** and then **start-workers.sh**. The **start-workers.sh** script will start all workers in tandem.

```bash
$ $SPARK_HOME/sbin/start-all.sh
```

Once launched, we can find a **spark://HOST:PORT** URL (e.g., **spark://ip-172-31-18-255.ec2.internal:7077** if we use EC2 instances) on the master's web UI.

We can use the spark URL to connect additional workers to it, or pass as the `master` argument to SparkContext later. To connect a new worker node to the master, we can SSH to that node and execute:

```bash
$ start-slave.sh spark://master:7077
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

Using the `--executor-memory` option, we can configure the size of executor memory spark-shell uses. Each application will have at most one executor on each worker, so this setting controls how much of that worker's memory the application will claim. By default, this setting is 1 GB. We decrease it to 512 MB as free tier EC2 instances we use have limited resources.

[We can also pass an option `--total-executor-cores <numCores>` to control the total number of cores that spark-shell uses across all executors for an application. By default, this is unlimited; that is, the application will launch executors on every available node in the cluster. To run code that accesses external libraries, we need to include the JARs for these libraries on the classpath of Spark’s processes.] proofread later


The **SparkContext** object is already created for us as variable `sc`.


If we use 3 workers with each running on an EC2 instance of 2 cores (c4.large):

```scala
sc.getConf.getAll
res0: Array[(String, String)] = Array((spark.executor.memory,512m), (spark.driver.port,38733), (spark.master,spark://master:7077), (spark.driver.host,master), (spark.home,/home/hadoop/spark), (spark.executor.id,driver), (spark.app.startTime,1623671132325), (spark.repl.class.uri,spark://master:38733/classes), (spark.app.id,app-20210614114533-0000), (spark.app.name,Spark shell), (spark.sql.catalogImplementation,hive), (spark.repl.class.outputDir,/tmp/spark-0549fe41-f885-4877-90b4-b35bf7d62c1b/repl-cb0c73b8-13ea-42ad-b71f-707b8a85d1d5), (spark.jars,""), (spark.submit.pyFiles,""), (spark.submit.deployMode,client), (spark.ui.showConsoleProgress,true))

scala> sc.defaultParallelism
res1: Int = 6
```


Text file RDDs can be created using SparkContext's `textFile` method. This method takes an URI for the file (either a local path on the machine, or a hdfs://, s3n://, etc URI) and reads it as a collection of lines. Load the input data from HDFS and declare a variable called lines:

```scala
scala> val lines = sc.textFile("hdfs://master:9000/data")
lines: org.apache.spark.rdd.RDD[String] = hdfs://master:9000/data MapPartitionsRDD[1] at textFile at <console>:24
```

[Some notes](https://spark.apache.org/docs/latest/rdd-programming-guide.html) on reading files with Spark:

-	A path on HDFS should be specified as hdfs://hostname/path_on_HDFS where hostname is the value of the **fs.defaultFS** property specified in the **$HADOOP_CONF_DIR/core-site.xml** file. We can also find the right URL on the Hadoop Namenode’s web UI.

-	If using a path on the local filesystem, the file must also be accessible at the same path on worker nodes. So we need to copy the file to all workers.

- All of Spark’s file-based input methods, including textFile, support running on directories, compressed files, and wildcards as well. For example, we can use textFile("/my/directory"), textFile("/my/directory/*.txt"), and textFile("/my/directory/*.gz").



There are a few things happening on this line that are worth going over. As we can see from the shell, the variable lines has a type of **RDD[String]**, even though we never specified that type information in our variable declaration. Type inference is a feature of the Scala programming language. Whenever possible, Scala figures out what type a variable has based on its context. In this case, Scala looks up the return type from the textFile function on the SparkContext object, sees that it returns an RDD[String], and assigns that type to the lines variable.

Whenever we create a new variable in Scala, we must preface the name of the variable with either val or var. Variables that are prefaced with val are immutable, and cannot be changed to refer to another value once they are assigned, whereas variables that are prefaced with var can be changed to refer to different objects of the same type.

Once created, lines can be acted on by dataset operations. Spark supports pulling data sets into a cluster-wide in-memory cache. This is very useful when data is accessed repeatedly. We may want to persist the lines RDD in memory using the cache method for much faster access the next time we query it.

```scala
scala> lines.cache()
res1: lines.type = hdfs://master:9000/data MapPartitionsRDD[1] at textFile at <console>:24
```


We can use the take operation of an RDD to get the first 10 records to have a look:

```scala
scala> lines.take(10)
res2: Array[String] = Array(The Project Gutenberg EBook of Encyclopaedia Britannica, 11th Edition,, Volume 6, Slice 1, by Various, "", This eBook is for the use of anyone anywhere at no cost and with, almost no restrictions whatsoever.  You may copy it, give it away or, re-use it under the terms of the Project Gutenberg License included, with this eBook or online at www.gutenberg.net, "", "", Title: Encyclopaedia Britannica, 11th Edition, Volume 6, Slice 1)
```

Unfortunately this is not very readable because `take` returns an array and Scala simply prints the array with each element separated by a comma. To make it easier to read the contents of an array, we can use the foreach method in conjunction with println to print out each value in the array on its own line:

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

Define a sequence of RDDs with the following transformations:

```scala
scala> val counts = lines.flatMap(_.split(" ")).map(x=>(x,1)).reduceByKey(_ + _, 1).map(x=>x.swap).sortByKey(false,1).map(x=>x.swap)
counts: org.apache.spark.rdd.RDD[(String, Int)] = MapPartitionsRDD[7] at map at <console>:25
```

Refer to Spark Programming Guide for details of these transformations.

Use the `saveAsTextFile` action to kick off a job to execute on a cluster:

```scala
scala> counts.saveAsTextFile("hdfs://master:9000/output")
```
We will see a console progress bar. For example, [Stage 2: shows the stage you are in now, and (14174 + 5) / 62500] is (numCompletedTasks + numActiveTasks) / totalNumOfTasksInThisStage]. The progress bar shows $numCompletedTasks / totalNumOfTasksInThisStage$.

We can also see how many lines in total are in the input data by issuing:

```scala
scala> lines.count
res5: Long = 114816
```
We may also want to add up the sizes of all the lines using the map and reduce operations as follows:

```scala
scala> lines.map(s => s.length).reduce((a, b) => a + b)
res6: Int = 6760426
```

5. Running Compiled Applications on the Standalone Cluster
