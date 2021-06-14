


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




2. Spark’s Interactive Shells


Spark comes with interactive shells that enable ad hoc data analysis. Unlike most other shells, however, which let you manipulate data using the disk and memory on a single machine such as those in R, Python, and operating system shells like Bash, Spark’s shells allow you to interact with data that is distributed on disk or in memory across many machines, and Spark takes care of automatically distributing this processing.

The first step is to open up one of Spark’s shells. To open the Scala version of the shell, type:

```bash
$ spark-shell
```

`spark-shell` defaults to run locally with as many worker threads as logical cores on your machine (i.e., `master = local[*]`).

when we use an EC2 instance with 2 logical cores, it can be verified that 2 worker threads have been spawned by running:

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
