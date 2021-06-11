

# Installing Pig and Running Pig


Pig does not need to be installed on a Hadoop cluster. It runs on the machine from which we launch Hadoop jobs as a **client-side application**. In practice, a cluster owner sets up one or more machines that have access to the Hadoop cluster but are not part of the cluster (i.e., they are not DataNodes or NodeManagers). But Pig needs to know the configuration of the cluster (e.g., the location of the cluster's NameNode and ResourceManager). The configurations can be found in core-site.xml, hdfs-site.xml, yarn-site.xml, and mapred-site.xml. Make sure that these files are present on the machine from where we interact with the Hadoop cluster.


We can clone the AMI with the Hadoop setup to launch an EC2 instance for Pig installation. In this way, we can enrich our big data analytics stack incrementally over the course of learning.


Download the latest stable release (0.17.0) from one of the Apache Download Mirrors (see [Pig Releases](https://pig.apache.org/releases.html#Download)). Unpack the downloaded tarball<sup><a href="#footnote1">1</a></sup>:



```shell
$ wget https://downloads.apache.org/pig/pig-0.17.0/pig-0.17.0.tar.gz
$ tar -xzf pig-0.17.0.tar.gz
$ mv pig-0.17.0 pig && rm pig-0.17.0.tar.gz
```

Open the **.bashrc** file to set some environment variables for running Pig:

```shell
$ nano .bashrc

```

We specify the *PIG_HOME* environment variable and add **~/pig/bin** to the environment variable *PATH* to use Pig command.  In order to run the Pig scripts in MapReduce mode, we need to set the *PIG_CLASSPATH* environment variable to the location of the cluster configuration directory (the directory that contains the core-site.xml, hdfs-site.xml, yarn-site.xml, and mapred-site.xml files). Append the following code to the bottom of **.bashrc**:


```shell
# Pig

export PIG_HOME=/home/hadoop/pig
export PATH=$PIG_HOME/bin:$PATH
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


<sup>[1](#footnote1)</sup> **~/pig/bin/** contains the Pig script file, **pig**, which describes Pig's environment variables, e.g., *PIG_CONF_DIR**, *PIG_CONF_DIR*, etc., and **~/pig/conf** contains the Pig properties file, **pig.properties**.
