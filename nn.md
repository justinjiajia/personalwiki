
# Launching Hadoop Cluster Using EC2 Instances From Scratch



In this tutorial, we will cover how to set up a fully-distributed Hadoop cluster on Amazon EC2 from scratch.

## 1. Creating a fresh EC2 instance

Log into the AWS Management Console, go to EC2, and click on **Launch instances**.

The technical setup of the fresh instance are as follows:

- **Ubuntu Server 20.04 LTS (HVM), SSD Volume Type**

- **t2.micro** – free tier

- The default security rule allows only SSH connections to the instance.


## 2. Install and Configure Hadoop on one EC2 instance


### 2.1  Create a dedicated user


When deploying a production environment, it is recommended to create a dedicated user for the express purpose of owning and running Hadoop tasks later. Simply type the following in the terminal to create a *hadoop* group and add a user with the name *hadoop* in the group:

```Shell
$ sudo addgroup hadoop
$ sudo adduser --ingroup hadoop hadoop
```


Most of the commands here need to be prefaced with the *sudo* command. This allows for executing commands with privileges elevated to the root-user administrative level on an as-needed basis. It is necessary when working with directories or files not owned by your user account. When using *sudo* you will be prompted for the password of the current user account. Initially, only users with *sudo* (administrative) privileges (in this case ubuntu) will be able to use this command.

It will prompt you to create the password for the newly-added user. To facilitate our successive configuration, use *bigdata* as the password (IMPORTANT!!!)<sup><a href="#footnote1">1</a></sup>.  


Then, add the user into group *sudo* to allow root access with the following command

```Shell
$ sudo adduser hadoop sudo
```

Now, switch to the new account:

```Shell
$ sudo su - hadoop
```

Then we can find the prefix of the command line change to *hadoop*, indicating that the active user is *hadoop*. And the working directory becomes */home/hadoop*. From now on, we'll refer to this directory as the home directory (or just home). We have full access rights (read/write/execute, or **rwx**) to the home directory, so we won't have to fiddle with *sudo* every time we need to make a change.


### 2.2 Install Java

Because this is a fresh virtual machine with only Ubuntu OS installed on it, we need to configure our computing environment by installing necessary software packages. The following commands install the OpenJDK 8 on this machine (See the Hadoop Wiki for <a href="https://cwiki.apache.org/confluence/display/HADOOP/Hadoop+Java+Versions">known good versions</a>):

```Shell
$ sudo apt-get update
$ sudo apt install openjdk-8-jdk

```

We can observe the installation starts. We will also be able to monitor the progress of the installation and read a success message in the end.

Now typing `java -version` can tell us that Java is successfully installed.

### 2.3 Download the latest stable release of Hadoop (version 3.2.1)

We'll use *wget* to download files from the Hadoop website. Download the binary distribution for installation by typing:

```bash
$ cd ~
$ wget http://apache.mirrors.tds.net/hadoop/common/hadoop-3.2.1/hadoop-3.2.1.tar.gz
$ ls
```

Unpack the downloaded tar file using this command:

```bash
$ tar vxzf hadoop-3.2.1.tar.gz
$ ls
```

Several flags are used with the tar command.  Among them, the *x* flag tells *tar* we are extracting files, and the *f* flag lets us specify the name of the file we're going to be working with.

Rename the extracted directory for easier reference. Because we are going to set Hadoop configurations by editing files in this directory, we need to give the active user the permission to make such changes in the “hadoop” directory. Give the ownership of the directory to the active user. Lastly, delete the compressed file to save the space:

```bash

$ mv hadoop-3.2.1 hadoop
$ sudo chown -R hadoop hadoop
$ rm hadoop-3.2.1.tar.gz

```

### 2.4 Configuring Environment of Hadoop Daemons

We have successfully installed Hadoop. Now, we want to configure the Hadoop cluster.



To do so, we will need to configure the ***environment*** in which the Hadoop daemons execute as well as the configuration parameters for the Hadoop daemons (e.g., HDFS daemons are NameNode, SecondaryNameNode, and DataNode. YARN daemons are ResourceManager, NodeManager, and WebAppProxy. If MapReduce is to be used, then the MapReduce Job History Server will also be running).


Most of Hadoop management environment variables can be set in **.bashrc** (for shell environment configuration)<sup><a href="#footnote2">2</a></sup><sup><a href="#footnote3">3</a></sup>. Make sure **/home/hadoop** is the current working directory. Use the Ubuntu *nano* editor to open the **.bashrc** file by using:

```bash
$ cd ~
$ nano .bashrc
```


The .bashrc file contains the script that will be read and executed whenever the interactive shell is started. We want to add several environment variables (e.g., JAVA_HOME, HADOOP_HOME, PATH, etc) to the **.bashrc** file.  Copy and paste the following lines to the bottom of this file and press CTRL+X to exit the editing mode. Remember to enter **y** to save the change you just made to the **.bashrc** file.

```bash
# Java
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH =$JAVA_HOME/bin:$PATH

# Hadoop
export HADOOP_HOME=/home/hadoop/hadoop
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
export HADOOP_CLASSPATH=$JAVA_HOME/lib/tools.jar
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
```


Use *source* to reload the **.bashrc** profile into the current command prompt. To check whether the environment variables have been updated correctly, we can use *echo* to print their values to the prompt:

```bash
$ source .bashrc
$ echo $HADOOP_HOME
$ echo $JAVA_HOME
```

Now, we can also check Hadoop installation by running:

```bash
$ hadoop version
```

Administrators should use the **hadoop-env.sh** and optionally the  **mapred-env.sh** and the **yarn-env.sh** scripts in *$HADOOP_CONF_DIR* to do site-specific customization of the Hadoop daemons' process environment (e.g., control the Hadoop scripts found in the **$HADOOP_HOME/bin**).

At the very least, we must specify the *JAVA_HOME* so that it is correctly defined on each remote node.

To do so, we use the *nano* editor to open **hadoop-env.sh**.

```bash
$ nano $HADOOP_CONF_DIR/hadoop-env.sh
```

Use CTRL+W and input *JAVA_HOME* to locate and uncomment the following line:


```bash
# export JAVA_HOME=
```

Append the path to the JDK installation to the end:

```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

Then press **CTRL+X** to exit the editing mode. Remember to enter **y** to save the change you just made.


### 2.5 Configuring the Hadoop Daemons

After done with the setup of the environment variables, we move on to configuring the Hadoop Daemons. Hadoop's configuration is driven by two types of important configuration files:

-	Read-only default configuration: **core-default.xml**, **hdfs-default.xml** , **yarn-default.xml**, and **mapred-default.xml**<sup><a href="#footnote5">5</a></sup><sup>.

-	Site-specific configuration: **core-site.xml**, **hdfs-site.xml**, **yarn-site.xml** and **mapred-site.xml** (located in **$HADOOP_CONF_DIR**).



The Hadoop configuration settings are a set of name-value pairs in these XML files.

Since all site-specific configuration files are located in the **$HADOOP_CONF_DIR** directory, we can proceed to the directory to edit them sequentially:



```bash


$ cd $HADOOP_CONF_DIR
```


First, edit **core-site.xml**. Use the nano editor to open it:

```bash
$ nano core-site.xml
```

In this file, we define one essential property for the entire system; that is, the name of the default file system we wish to use. The value of *fs.defaultFS* is the URI (protocol specifier, hostname, and port) that describes the NameNode for the cluster. Each machine in the cluster on which Hadoop is expected to operate needs to know the NameNode’s address. For example, the DataNode processes needs this information to register with NameNode, and make their data available through it. Individual client programs will connect to this address to retrieve the locations of actual file blocks.

Paste the following in the `<configuration>` tag. Save and exit the editor.

```xml
<property>
    <name>fs.defaultFS</name>
    <value>hdfs://master:9000</value>
    <description>NameNode URI. master is the hostname of the machine running namenode. 9000 denotes port on which datanode will send heartbeat to namenode.</description>
</property>

<property>
    <name>io.file.buffer.size</name>
    <value>131072</value>
</property>

<property>
    <name>hadoop.tmp.dir</name>
    <value>/home/hadoop/hadoop_tmp</value>
    <description>A base for other temporary directories.</description>
</property>


<property>
    <name>hadoop.http.staticuser.user</name>
    <value>hadoop</value>
    <description>The username used when using the HDFS web UI.</description>
</property>
```
Next, edit hdfs-site.xml to configure NameNode, SecondaryNameNode, and DataNode. Copy the following and paste it between the configuration tags in the nano editor, save and exit the editor.


```XML

<property>
    <name>dfs.namenode.name.dir</name>
    <value>file://${hadoop.tmp.dir}/dfs/name</value>
    <description> Path on the local filesystem where the NameNode stores the name table (fsimage) and transactions logs persistently.</description>
</property>


<property>
    <name>dfs.datanode.data.dir</name>
    <value>file://${hadoop.tmp.dir}/dfs/data</value>
    <description> Comma separated list of paths on the local filesystem of a DataNode where it should store its blocks.</description>
</property>

<property>
    <name>dfs.blocksize</name>
    <value>16m</value>
</property>


```

Now return to the local system. Create directories on the local file system for the NameNode to store the namespace and transactions logs and for individual DataNodes to store their blocks.

```shell
$ cd ~
$ mkdir -p hadoop_tmp/dfs/name hadoop_tmp/dfs/data
```

In practice, if there is one instance exclusively used as the NameNode, we don't need to create the **~/hadoop_tmp/dfs/data** directory on it. Similarly, there is no need to create the namenode directory on the machine exclusively used as the DataNode.


Next is the yarn-site.xml used for configuring ResourceManager and NodeManager. Proceed to the $HADOOP_CONF_DIR directory again and open the yarn-site.xml file:

```shell
$ cd $HADOOP_CONF_DIR
$ nano yarn-site.xml
```

Copy the following and paste it between the configuration tags just like before.

```XML
<property>    
    <name>yarn.resourcemanager.hostname</name>
    <value>master</value>
    <description>The hostname of the ResourceManager.</description>
</property>

<property>    
  <name>yarn.nodemanager.resource.detect-hardware-capabilities</name>
  <value>true</value>
  <description>Enable auto-detection of node capabilities such as memory and CPU. </description>
</property>

<property>
   <name>yarn.nodemanager.aux-services</name>
   <value>mapreduce_shuffle</value>
   <description>Shuffle service that needs to be set for MapReduce applications.</description>
</property>

```

Save and exit the editor.


Edit mapred-site.xml to configure MapReduce Applications and MapReduce JobHistory Server:

```shell
$ nano mapred-site.xml
```

Copy and paste the following between the configuration tags<sup><a href="#footnote6">6</a></sup>:


```xml
<property>
  <name>mapreduce.framework.name</name>
  <value>yarn</value>
  <description>The runtime framework for executing MapReduce jobs.</description>
</property>

<property>
  <name>yarn.app.mapreduce.am.env</name>
  <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
  <description>User added environment variables for the MR App Master processes.</description>
</property>

<property>
  <name>mapreduce.map.env</name>
  <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
  <description> User added environment variables for the map task processes.</description>
</property>

<property>
  <name>mapreduce.reduce.env</name>
  <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
  <description> User added environment variables for the reduce task processes.</description>
</property>
```

Save and exit the editor.



These files should later be replicated consistently across all machines in the cluster, as we launch our fully-distributed cluster (For more configuration options, please reference the online manual for [Hadoop Cluster Setup](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/ClusterSetup.html).)




### 2.6 Setting up the SSH key

Later, password-less SSH is going to be used for remote accesses among machines in our Hadoop cluster (e.g., the master node remotely start the DataNodes and YARN services on worker nodes). Generate SSH key pairs (other than that we use to interact with the Unix shell via PUTTY) by issuing the following commands:

```shell
$ cd ~
$ ssh-keygen -t rsa -P ''
```

`ssh-keygen` requires us to specify the key type with the `–t` option, as there is no default.

`rsa` stands for the RSA algorithm for public-key cryptography. `-P ''` (two primes) indicates that the newly-created pair does not require a passphrase for authentication<sup><a href="#footnote7">7</a></sup>.


`ssh-keygen` then creates a local SSH directory **~/.ssh** if it doesn’t already exist, and stores the private and public components of the generated key in two files there. By default, their names are **id_dsa** and **id_dsa.pub**

Run the following command, you'll find that the directory **~/.ssh** has been created and  both **id_rsa** and **id_rsa.pub** placed.

```shell
$ cd .ssh && ls
```

To be able to log into this node from other nodes of the cluster using the private key, we should first add the public key to **authorized_keys**. We copy the public key **id_rsa.pub** to the authorized_keys file with the following command:

```shell
$ cat id_rsa.pub >> authorized_keys
```

The authorized_keys file holds a list of authorized public keys for this machine to act as a server. The login attempt from a client will be accepted if the client proves that it knows the private key and the public key is in the server's authorization list (~/.ssh/authorized_keys on the instance).



## Footnote


<sup>[1](#footnote1)</sup> If you use a password other than *bigdata*, you must change the affected part of **sshconf.sh** accordingly for configuring SSH connections.


<sup>[2](#footnote2)</sup> When getting started, the specific shell will read and execute the configuration program native to that shell. For example, ksh executes .profile, csh executes .cshrc, and bash executes .bash_profile and/or .bashrc.



<sup>[3](#footnote3)</sup>Numerous files end with the letters **rc** (e.g., **.bashrc**) on Unix installations. The letters **rc** are historical in nature and are taken from the words run commands to indicate the intended purpose of the file


<sup>[4](#footnote4)</sup> *export* is a bash shell built-in command. It marks an environment variable to be exported to child-processes.If a value is supplied, assign the value before exporting. The syntax is `export [-f] [-n] [name[=value] ...]` or `export -p`. There should be no space around the `=` sign in `name[=value]`.


<sup>[5](#footnote5)</sup> The actual files are present in different JAR files under **$HADOOP_HOME/share/hadoop**. For example, `jar tf $HADOOP_HOME/share/hadoop/common/hadoop-common-3.2.1.jar | grep default`, `jar tf $HADOOP_HOME/share/hadoop/hdfs/hadoop-hdfs-3.2.1.jar | grep default`, `jar tf $HADOOP_HOME/share/hadoop/yarn/hadoop-yarn-common-3.2.1.jar | grep default`, and `jar tf $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-client-core-3.2.1.jar | grep default`. The information can also be found in the Apache Hadoop documentation under **$HADOOP_HOME/share/doc/hadoop**. For example, you can find them by running `ls -R $HADOOP_HOME/share/doc/hadoop | grep default.xml`. You can also find the information online.


<sup>[6](#footnote6)</sup> In Hadoop 3, YARN containers do not inherit the NodeManagers' environment variables. Therefore, if you want to inherit NodeManager's environment variables (e.g. `HADOOP_MAPRED_HOME`), you need to set additional parameters (e.g., `mapreduce.admin.user.env` and `yarn.app.mapreduce.am.env`). https://issues.apache.org/jira/browse/MAPREDUCE-6702

<sup>[7](#footnote7)</sup> We say *passphrase* instead of *password* both to differentiate it from a login password, and to stress that spaces and punctuation are allowed and encouraged.
