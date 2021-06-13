

# Installing Pig and Running Pig


Pig does not need to be installed on a Hadoop cluster. It runs on the machine from which we launch Hadoop jobs as a **client-side application**. In practice, a cluster owner sets up one or more machines that have access to the Hadoop cluster but are not part of the cluster (i.e., they are not DataNodes or NodeManagers). But Pig needs to know the configuration of the cluster (e.g., the location of the cluster's NameNode and ResourceManager). The configurations can be found in core-site.xml, hdfs-site.xml, yarn-site.xml, and mapred-site.xml. Make sure that these files are present on the machine from where we interact with the Hadoop cluster.


We can clone the AMI with the Hadoop setup to launch an EC2 instance for Pig installation. In this way, we can enrich our big data analytics stack incrementally over the course of learning.


Download the latest stable release (0.17.0) from one of the Apache Download Mirrors (see [Pig Releases](https://pig.apache.org/releases.html#Download)). Unpack the downloaded tarball:



```shell
$ wget https://downloads.apache.org/pig/pig-0.17.0/pig-0.17.0.tar.gz
$ tar -xzf pig-0.17.0.tar.gz
$ mv pig-0.17.0 pig && rm pig-0.17.0.tar.gz
```

Open the **.bashrc** file:

```shell
$ nano .bashrc

```

We need to add **~/pig/bin** to the environment variable *PATH* for running the Pig script file<sup><a href="#footnote1">1</a></sup>. Also, in order to run the Pig scripts in MapReduce mode, we need to set the *PIG_CLASSPATH* environment variable to the location of the cluster configuration directory (the directory that contains the core-site.xml, hdfs-site.xml, yarn-site.xml, and mapred-site.xml files).

Append the following code to the bottom of **.bashrc**:


```shell
# Pig

export PATH=$/home/hadoop/pig/bin:$PATH
export PIG_CLASSPATH=$HADOOP_CONF_DIR

```








Press CTRL+X to save the change and exit nano. Load the new directory into the working environment:

```shell
$ source ~/.bashrc
```
Test the Pig installation with this simple command:


```shell
$ pig -help
```


## 2. Running Pig Locally in Interactive Mode

We can run Pig in local mode interactively using the **Grunt shell**. Local mode helps prototype and debug the Pig Latin scripts on small representative datasets before we apply the same processing to large data<sup><a href="#footnote2">2</a></sup>.   

Invoke the Grunt shell using the `pig` command, setting the execution type to local by using the `-x` or `-exectype` flag as shown below:

```bash
$ pig -x local
```
Grunt has line-editing facilities like those used in the bash shell and many other command-line applications. For instance, the <kbd>Ctrl</kbd>+<kbd>E</kbd> key combination will move the cursor to the end of the line, while Ctrl-A will move the cursor to the start of the line. Grunt remembers command history, and we can recall lines in the history buffer using the up or down cursor keys.

Another handy feature is Grunt's completion mechanism, which will try to complete Pig Latin keywords and functions when we press the Tab key. For example, consider the following incomplete line:

```pig
grunt> a = foreach b ge
```


If we press the <kdb>Tab</kdb> key at this point, `ge` will expand to generate, a Pig Latin keyword:


```pig

grunt> a = foreach b generate

```


A number of commands that come directly from Unix shells can operate in ways that are familiar: these include cat, cp, ls, mkdir, mv, rm, and so on.

We can terminate the Grunt session with the `quit` command, the shortcut `\q`, or Ctrl-D. If we are in the middle of executing a Pig script, we can use Ctrl-C to terminate the execution.

Let's exit Grunt for now:


```pig
grunt> quit
```

Prepare the datasets to be processed by executing the following commands:

$ mkdir most_visited
$ echo "amy	,cnn.com,30
amy,bbc.com,20
amy,flickr.com,15
fred,cnn.com,15" > most_visited/visits
$ echo "cnn.com,news,0.9
bbc.com,news,0.8
flickr.com,photos,0.7
espn.com,sports,0.9" > most_visited/urls



<sup>[1](#footnote1)</sup> **~/pig/bin/** contains the Pig script file, [**pig**](https://github.com/apache/pig/blob/trunk/bin/pig), which describes and sets Pig's environment variables, such as *PIG_CONF_DIR** and *PIG_CONF_DIR*. **~/pig/conf** contains the Pig properties file, **pig.properties**.

<sup>[2](#footnote2)</sup> When running locally, Pig uses the Hadoop class LocalJobRunner, which reads from the local filesystem and executes MapReduce jobs locally. This has the nice property that Pig jobs run locally in the same way as they will on our cluster.
