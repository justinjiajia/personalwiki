
# Launching Hadoop Cluster Using EC2 Instances From Scratch



In this tutorial, we will cover how to set up a fully-distributed Hadoop cluster on Amazon EC2 from scratch.

## 1. Creating a fresh EC2 instance

Log into the AWS Management Console, go to EC2, and click on **Launch instances**.

The technical setup of the fresh instance are as follows:

- **Ubuntu Server 20.04 LTS (HVM), SSD Volume Type**

- **t2.micro** – free tier

- The default security rule allows only SSH connections to the instance.


## 2. Install and Configure Hadoop on one EC2 instance



Refresh updates and upgrade all packages

```shell
$ sudo apt-get update && sudo apt-get dist-upgrade
```



### 2.1  Create a dedicated user


When deploying a production environment, it is recommended to create a dedicated user for the express purpose of owning and running Hadoop tasks later. Simply type the following in the terminal to create a *hadoop* group and add a user with the name *hadoop* in the group:

```Shell
$ sudo addgroup hadoop
$ sudo adduser --ingroup hadoop hadoop
```


Most of the commands here need to be prefaced with the *sudo* command. This allows for executing commands with privileges elevated to the root-user administrative level on an as-needed basis. It is necessary when working with directories or files not owned by your user account. When using *sudo* you will be prompted for the password of the current user account. Initially, only users with *sudo* (administrative) privileges (in this case **ubuntu**) will be able to use this command.

It will prompt you to create the password for the newly-added user. To facilitate our successive configuration, use **bigdata** as the password (IMPORTANT!!!)<sup><a href="#footnote1">1</a></sup>.  


Then, add the user into group *sudo* to allow root access with the following command

```Shell
$ sudo adduser hadoop sudo
```

Now, switch to the new account:

```Shell
$ sudo su - hadoop
```

Then we can find the prefix of the command line change to *hadoop*, indicating that the active user is *hadoop*. And the working directory becomes **/home/hadoop**. From now on, we'll refer to this directory as the home directory (or just **home**). We have full access rights (read/write/execute, or **rwx**) to the home directory, so we won't have to fiddle with *sudo* every time we need to make a change.




### 2.2 Install Java

Because this is a fresh virtual machine with only Ubuntu OS installed on it, we need to configure our computing environment by installing necessary software packages. The following commands install the OpenJDK 8 on this machine (See the Hadoop Wiki for <a href="https://cwiki.apache.org/confluence/display/HADOOP/Hadoop+Java+Versions">known good versions</a>):

```Shell
$ sudo apt-get update
$ sudo apt install openjdk-8-jdk
```

We can observe the installation starts. We will also be able to monitor the progress of the installation and read a success message in the end.

Now typing `java -version` can tell us that Java is successfully installed.

### 2.3 Download the latest stable release of Hadoop (version 3.2.1)

We'll use *wget* to download files from the [Hadoop website](https://hadoop.apache.org/releases.html). Download the binary distribution for installation by typing:

```bash
$ cd ~
$ wget http://apache.mirrors.tds.net/hadoop/common/hadoop-3.2.1/hadoop-3.2.1.tar.gz
$ ls
```

- `wget` is used to download files from the Web. It supports HTTP, HTTPS, and FTP protocols, as well as retrieval through HTTP proxies.

- <kbd>Ctrl</kbd>+<kbd>C</kbd> can cancel the currently-running download process.

- For huge files, we can put the download in background using wget option `-b`. We can always check the status of the download using `tail -f`.


Unpack the downloaded tar file and delete the compressed file to save the space:

```bash
$ tar vxzf hadoop-3.2.1.tar.gz
$ rm hadoop-3.2.1.tar.gz && ls
```

Several flags are used with the tar command.  Among them, the `x` flag tells `tar` we are extracting files, and the `f` flag lets us specify the name of the file we're going to be working with. The `z` option tells tar to read or write archives through gzip, and it should be used when operating on files with the extension **.tar.gz**. The `v` option tells tar to operate verbosely.

Rename the extracted directory for easier reference. Because we are going to set Hadoop configurations by editing files in this directory, we need to give the active user the permission to make such changes in the **hadoop** directory. Give the ownership of the directory to the active user:

```bash

$ mv hadoop-3.2.1 hadoop
$ sudo chown -R hadoop hadoop

```

### 2.4 Configuring Environment of Hadoop Daemons

We have successfully installed Hadoop. Now, we want to configure the Hadoop cluster.



To do so, we will need to configure the ***environment*** in which the Hadoop daemons execute as well as the ***configuration parameters*** for the Hadoop daemons (e.g., HDFS daemons are NameNode, SecondaryNameNode, and DataNode. YARN daemons are ResourceManager, NodeManager, and WebAppProxy. If MapReduce is to be used, then the MapReduce Job History Server will also be running).


Most of Hadoop management environment variables can be set in **.bashrc** (for shell environment configuration)<sup><a href="#footnote2">2</a></sup><sup><a href="#footnote3">3</a></sup>. Make sure **/home/hadoop** is the current working directory. Use the Ubuntu *nano* editor to open **.bashrc** by using:

```bash
$ cd ~
$ nano .bashrc
```


**.bashrc** contains the script that will be read and executed whenever the interactive shell is started. We want to add several environment variables (e.g., *JAVA_HOME*, *HADOOP_HOME*, *PATH*, etc.) to the **.bashrc** file<sup><a href="#footnote4">4</a></sup><sup><a href="#footnote5">5</a></sup>.  

Copy and paste the following lines to the bottom of this file:

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

Press <kbd>Ctrl</kbd>+<kbd>X</kbd> to exit the editing mode. Remember to enter **y** to save the change you just made to **.bashrc**.

Use *source* to reload the **.bashrc** profile into the current command prompt. To check whether the environment variables have been updated correctly, we can use *echo* to print their values to the prompt:

```bash
$ source .bashrc
$ echo $HADOOP_HOME && echo $JAVA_HOME
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

Use <kbd>CTRL</kbd>+<kbd>W</kbd> and input **JAVA_HOME** to locate and uncomment the following line:


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

-	Read-only default configuration: **core-default.xml**, **hdfs-default.xml** , **yarn-default.xml**, and **mapred-default.xml**<sup><a href="#footnote6">6</a></sup><sup>.

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

In this file, we define one essential property for the entire system; that is, the name of the default file system we wish to use. The value of `fs.defaultFS` is the URI (protocol specifier, hostname, and port) that describes the NameNode for the cluster. Each machine in the cluster on which Hadoop is expected to operate needs to know the NameNode’s address. For example, the DataNode processes needs this information to register with NameNode, and make their data available through it. Individual client programs will connect to this address to retrieve the locations of actual file blocks.

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

Next, edit **hdfs-site.xml** to configure NameNode, SecondaryNameNode, and DataNode. Copy the following and paste it between the configuration tags in the nano editor, save and exit the editor.


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


Next is the **yarn-site.xml** used for configuring ResourceManager and NodeManager. Proceed to the $HADOOP_CONF_DIR directory again and open the yarn-site.xml file:

```shell
$ cd $HADOOP_CONF_DIR && nano yarn-site.xml
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


Edit **mapred-site.xml** to configure MapReduce Applications and MapReduce JobHistory Server:

```shell
$ nano mapred-site.xml
```

Copy and paste the following between the configuration tags<sup><a href="#footnote7">7</a></sup>:


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



These files should later be replicated consistently across all machines in the cluster, as we launch our fully-distributed cluster. For more configuration options, please reference the online manual for [Hadoop Cluster Setup](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/ClusterSetup.html).




### 2.6 Setting up the SSH key

Later, password-less SSH is going to be used for remote accesses among machines in our Hadoop cluster (e.g., the master node remotely start the DataNodes and YARN services on worker nodes). Generate SSH key pairs (other than that we use to interact with the Unix shell via PUTTY) by issuing the following commands:

```shell
$ cd ~ && ssh-keygen -t rsa -P ''
```


`ssh-keygen` requires us to specify the key type with the `–t` option, as there is no default. `ssh-keygen` can produce either a DSA or RSA key (both DSA and RSA are encryption algorithms). Here, `rsa` stands for the RSA algorithm. `-P ''` (two primes) indicates that the newly-created pair does not require a passphrase for authentication<sup><a href="#footnote8">8</a></sup>.


`ssh-keygen` then creates a local SSH configuration directory **~/.ssh** if it doesn't already exist, and stores the private and public components of the generated key in two files there. By default, their names are **id_dsa** and **id_dsa.pub**

Run the following command, you'll find that the directory **~/.ssh** has been created and  both **id_rsa** and **id_rsa.pub** placed.

```shell
$ cd .ssh && ls
```
To be able to log into this account (hadoop) from other nodes of the cluster using the private key, we must associate the public key with this account.

This is done by editing a file **authorized_keys** in **~/.ssh**.

A typical **authorized_keys** file contains a list of public-key data, one key per line, for this machine to act as a server. The login attempt from a client will be accepted if the client proves that it holds the private key and the public key is in the server's authorization list (i.e., **authorized_keys**).


we should add the public key to **authorized_keys**. We do so by appending **id_rsa.pub** to **authorized_keys** with the following command:

```shell
$ cat id_rsa.pub >> authorized_keys
```
You can then take a look at what **authorized_keys** contains:

```shell
$ cat authorized_keys
```

The output may look like the following:

```shell
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDFL3aGOyW1BvpwaGFEMNLwh8ppwgs7i/P9EfPvRnp9UFqrz+AsB3mF8tMD4x9LyACDQQghhIK3+WtyB+FrkPaeJrXlJrdwjWteCNq50g73K9q19upHCQa+wVx5Pnh1JoNhWQMTXJ2TuLEee8+g0E8J8BIMFr9Ja5tpuCzZEVytDAMfrRuexKx1zDGU2fU++Txtn4BrxaAs4bf91wFMiFjKPu8KHO9G/ztBuGiM5EFOcnCplOeIetncrun50qLmROFngIBBZjbD8J1j646Vf5g7Qud9yFuoaTXqKwwBsfOlJ5oki1u/Yl0inA6xFo2nqtysKz9ef1aNpRIpmwW0/v+jYRaQNvlspHpJ1gErncUoJo+3I72BUJp5mQ3TvdzfzjX+thzpF1uUeo5/M1a4noesnTfB3wbtlx5l12Nr10YLv0EXOIU1q50pdNuC5dTgOXCHBiIjfHcZDoZzz7FqFvoPeiXfElkXKY1MPi7rkMvTWBBzQOzafHp46XQuNtDUwx0= hadoop@ip-172-31-84-231
```

**authorized_keys** now contains only 1 public key, occupying its own line. You may observe line breaks inside the long number when using a command line interface. They are printing artifacts, because it is too long to fit on the screen.



## 2. Creating an AMI for the Configured instance

We have successfully set up Hadoop on one EC2 instance. Hadoop's configuration information should be consistent across all machines in a fully-distributed cluster. To make things easier, we will now just clone the EC2 instance to create an AMI. To do that:

1.	Select the configured instance on the EC2 dashboard. Then, click on the **Actions** tab. Click **Image and templates** > **Create image**. Give you image a name (e.g., hadoop-your-ITSC-account). Your instance is rebooting, and your SSH connection is getting lost.

2.	Look on the left pane of your EC2 dashboard. Click **AMIs** to view your created image. It should be in pending status. Wait for it to be available.

3.	Once available, we can clone this AMI to create more EC2 instances that share the same setting as the configured one to launch a fully-distributed cluster.


## 3. Launching a Fully-Distributed Cluster


To launch a fully-distributed cluster with more than one EC2 instance, we can click on Launch to clone the AMI. We will go through the same steps as when we created the one for Hadoop installation and configuration. During this process, remember to select the existing security group and key pair.

- c5.xlarge

- Configure the security group to allow inbound **SSH** from **Anywhere** and **All TCP** from all instances in the same security group (the source of this rule will be the id of the same security group).


We can launch as many instances as we want.




### 3.1 Configuring SSH Connections among EC2 Instances


We want to create a shell script to automate the creation of the IP-address-hostname mapping on and the setup of SSH connections among all the EC2 instances we are going to use. Use **nano** to open a file called **sshconf.sh** in the home directory, and copy and paste the following commands into it<sup><a href="#footnote9">9</a></sup>.  Save the change.

```bash
#!/bin/bash
ETC_HOSTS=/etc/hosts
array=()
array2=()

echo "How many worker(s) are you going to use (with the master excluded from counting)?"
read workersNumber

re='^[1-9]$'
if ! [[ $workersNumber =~ $re ]] ; then
   echo "Error number of workers: Must be between 1 and 9" >&2; exit 1
fi

array=("master")
for ((i=1;i<$workersNumber+1;++i)); do
   array+=("worker$i")
done

array2=()
for ((i=0;i<${#array[@]};++i)); do
   echo "Enter the private IP address for ${array[i]}?"
   read ipAddr
   array2+=("$ipAddr")
done

for ((i=0;i<${#array[@]};++i)); do
    if [ -n "$(grep ${array[i]} /etc/hosts)" ]
    then
        echo "${array[i]} Found in your $ETC_HOSTS, Removing now...";
        sudo sed -i".bak" "/${array[i]}/d" $ETC_HOSTS
    else
        echo "${array[i]} was not found in your $ETC_HOSTS";
    fi
done

for ((i=0;i<${#array[@]};++i)); do

    HOSTS_LINE="${array2[i]}\t${array[i]}"
    echo "Adding new ${array[i]} to your $ETC_HOSTS";
    sudo -- sh -c -e "echo '$HOSTS_LINE' >> /etc/hosts";

done
rm -Rf $PWD/.ssh/known_hosts
for HOSTNAME in ${array[@]};
do
    ssh -o StrictHostKeyChecking=no $HOSTNAME "exit";
done


for HOSTNAME in ${array[@]};
do
    ssh $HOSTNAME -t 'echo bigdata | sudo -S chmod 666 /etc/hosts'
    rcp /etc/hosts $HOSTNAME:/etc/hosts
    rcp $PWD/.ssh/known_hosts $HOSTNAME:$PWD/.ssh/known_hosts
    ssh $HOSTNAME -t 'echo bigdata | sudo -S chmod 644 /etc/hosts'
done

```

The term `bigdata` in `'echo bigdata | sudo -S chmod 644 /etc/hosts'` and `'echo bigdata | sudo -S chmod 644 /etc/hosts'` is the password for the dedicated user, *hadoop*.

Give the execute permission to the script file and execute it:

```shell
$ chmod +x sshconf.sh
$ ./sshconf.sh
```

Provide information as instructed. Consequently, we can find the following mapping created in **/ect/hosts** of each instance:

```
private_IP_address master
private_IP_address worker1
private_IP_address worker2
...
```

This mapping associates the private IP address of a machine with a hostname, which serves as an easy reference to the machine.


### 3.2 Starting the Hadoop Daemons  

One last thing to do before launching the cluster is to declare all the worker nodes to use inside the file $HADOOP_CONF_DIR/workers (i.e., the nodes where we want to start DataNode and NodeManager). Use **nano** to open it:


```bash
$ nano $HADOOP_CONF_DIR/workers
```


Remove the line showing `localhost`. Add one hostname per line into the file. Save and exit the editor.

```
worker1
worker2
...
```
To start a Hadoop cluster we need to start both the HDFS and YARN cluster.

The first time we bring up HDFS, it must be formatted. Format a new distributed filesystem by issuing:


```bash
$ hdfs namenode -format
```


We are now ready to launch the Hadoop cluster. Since the **$HADOOP_CONF_DIR/workers** file and SSH trusted access are configured, the HDFS daemons can be started with a utility script. In order to highlight the difference introduced by executing the start-dfs.sh script, we can first check all java processes running currently on this machine. Issue the following commands sequentially:

```bash
$ jps
$ start-dfs.sh
```


This command will start the NameNode daemon on the node where the **start-dfs.sh** script was invoked.


We can check the status of the Hadoop daemons:

```bash
$ jps
```
It should display the following processes:

```
Namenode
SecondaryNamenode
Jps
```



It will also start the DataNode daemon process on each of the worker nodes specified in the **workers** file.

In the background scene, this script will ssh into each worker machine to start a DataNode daemon.





Similarly, all of the YARN processes can be started with a utility script as well:

```bash
$ start-yarn.sh
```




You can test the functionality by submitting a MapReduce job for the canonical word count example:

```bash
$ yarn jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar wordcount /input/data /output
```
To run MapReduce applications on YARN, we should also start the MapReduce JobHistory Server with the following command:

```bash
$ mapred --daemon start historyserver
```

Several Hadoop Web UIs are reachable through the following:

-	http://PUBLIC_IP_OF_RESOURCEMANAGER:8088 – <a href="https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-common/yarn-default.xml#yarn.resourcemanager.webapp.address" target="_blank">Resource Manager</a>

-	http://PUBLIC_IP_OF_NAMENODE:9870 – <a href="https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/hdfs-default.xml#dfs.namenode.http-address" target="_blank">NameNode</a>


-	http://PUBLIC_IP_OF_JOBHISTORY_SERVER:19888 – <a href="https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml#mapreduce.jobhistory.webapp.address" target="_blank">MapReduce JobHistory Server</a>


After use, enter the following commands to stop the HDFS daemons, the YARN daemons, and MapReduce Jobhistory Server:

```shell
$ mapred --daemon stop historyserver
$ stop-yarn.sh
$ stop-dfs.sh
```



## Use of HDFS Web UI


Because we've set `hadoop.http.staticuser.user` (whose default value is `dr.who`) in **core-site.xml** to `hadoop`, we can experiment with the first icon right to the long text field to create a new directory, e.g., **/usr**.

This returns a new row that has several clickable fields, including Permission, Owner, Group, Name.

Different from what we did to interact with HDFS in our lab, we are going to prepare the data on HDFS via the use
of this interface.

To make the file upload icon function (the second icon right to the long text field) so that we can upload files from the local computer, we need to do some configuration on our local computer (rather than any EC2 instance).


For Mac users, start a new Terminal session, and edit **/etc/hosts** on your own computer.

```shell
$ sudo nano /etc/hosts
```


Provide the mapping of the public IPs (not the private ones this time, because your computer is not part of the cluster) of the EC2 instances and their hostnames by appending lines of the form:

```
Public_IP_Address master
Public_IP_Address worker1
Public_IP_Address worker2
...
```

to the file. The order of the mapping should be the same as the one you input when running **sshconf.sh**.

 Save the change and exit **nano**.


For Windows users, navigate to the folder **C:\Windows\System32\drivers\etc**, open the file a file named **hosts** (right-click the file icon
and choose run as administrator) and append lines of the form:


```
Public_IP_Address master
Public_IP_Address worker1
Public_IP_Address worker2
...
```


to the file in the same way. Again, the order of the mapping should be the same as the one you input when running **sshconf.sh**.

Save the change and close notepad.

The default setting can be rolled back by removing these lines after you are done with the assignment.


Once this local configuration is done, you can use the file upload icon to upload a file from your local computer to HDFS. You can download files from HDFS as well.


## Hadoop Streaming


Python 3 is installed on Ubuntu EC2 instances. You can run the following code to check the exact Python version installed on an instance:

```shell
$ python3 --version
```

For the word counting application, the Python code for the mapper can be specified as follows:

```python
#!/usr/bin/env python3

import sys
import re

for line in sys.stdin:
    line = line.strip().lower()
    line = re.sub('[^A-Za-z\s]', '', line)
    words = line.split()
    for word in words:
        print("%s\t%s" % (word, 1))

```


The easiest way to create a **.py** file and populate it with the code above is to use *nano*. Run the following code in the home directory:

```shell
$ nano mapper.py
```

Copy and paste the Python code into the opened file, and format it to make sure code lines are properly indented.



After you finish editing, hit Ctrl+X (on Windows) or Control+X (on macOS) to quit nano.  Enter **y** to save the change.

The Python code for the reducer can be specified as follows:

```python
#!/usr/bin/env python3

import sys

current_word, current_count = None, 0
for line in sys.stdin:
    line = line.strip()
    word, count = line.split('\t', 1)
    count = int(count)
    if current_word == word:
        current_count += count
    else:
        if current_word:
            print("%s\t%s" % (current_word, current_count))
        current_count = count
        current_word = word

if word == current_word: print("%s\t%s" % (current_word, current_count))

```


Run the following code in the home directory to create a .py file named reducer:

```shell
$ nano reducer.py
```

Again, copy and paste the Python code into the opened file and format it properly.





Then quit nano. Remember to enter **y** to save the change.

Now, we've created the **mapper.py** and **reducer.py** scripts. Next, grant them execution permission by running the following command:

```shell
$ chmod +x mapper.py reducer.py
```

Pasting the code directly into the nano editor may mess up the indentation. Therefore, it is recommended to use the following line of code to test the two Python scripts LOCALLY before submitting them to the cluster:

```shell
$ echo "foo FOO2 quux. lab foo Ba1r Quux" | ~/mapper.py | sort -k 1,1 | ~/reducer.py
```

If exceptions are raised, use `nano mapper.py` and `nano reducer.py` to open the two .py files to examine whether all lines of the Python code are correctly indented.

If you observe the following output, it means the code works properly:

```
bar     1
foo     3
lab     1
quux    2
```


Now everything is ready. You can run the Python MapReduce job on your EMR cluster by typing:


or:

```shell
$ mapred streaming \
-D mapreduce.job.reduces=2 \
-input input_dir_on_HDFS \
-output output_dir_on_HDFS \
-mapper mapper.py \
-reducer reducer.py \
-file mapper.py \
-file reducer.py
```
-file is a command option

```shell
$ mapred streaming \
-D mapreduce.job.reduces=2 \
-files mapper.py,reducer.py \
-input input_dir_on_HDFS \
-output output_dir_on_HDFS \
-mapper mapper.py \
-reducer reducer.py \
```

We can also use the -files option to specify a comma-separated list of files (in an adjacent pair of files, the name of the latter file must succeed that of the former without any space in between) to be copied to the cluster. These files will get copied onto the current working directory of mapper or reducer on all nodes.
And because `-files` is a generic option (see `mapred streaming -help`), it comes before those command options.




## Footnote


<sup>[1](#footnote1)</sup> If you use a password other than *bigdata*, you must change the affected part of **sshconf.sh** accordingly for configuring SSH connections.


<sup>[2](#footnote2)</sup> When getting started, the specific shell will read and execute the configuration program native to that shell. For example, ksh executes .profile, csh executes .cshrc, and bash executes .bash_profile and/or .bashrc.



<sup>[3](#footnote3)</sup>Numerous files end with the letters **rc** (e.g., **.bashrc**) on Unix installations. The letters **rc** are historical in nature and are taken from the words run commands to indicate the intended purpose of the file

<sup>[4](#footnote4)</sup> For example, the *PATH* variable contains a colon-separated list of directories that the shell searches for commands (Colon (:) is used to concatenate multiple directories). With $JAVA_HOME/bin added to the list, all tools in **$JAVA_HOME/bin**, e.g., `jps`, the tool used to list all java processes of a user, can be directly called in without specifying the path.

<sup>[5](#footnote5)</sup> *export* is a bash shell built-in command. It marks an environment variable to be exported to child-processes. If a value is supplied, assign the value before exporting. The syntax is `export [-f] [-n] [name[=value] ...]` or `export -p`. There should be no space around the `=` sign in `name[=value]`.


<sup>[6](#footnote6)</sup> The actual files are located in different JAR files under **$HADOOP_HOME/share/hadoop**. For example, `jar tf $HADOOP_HOME/share/hadoop/common/hadoop-common-3.2.1.jar | grep default`, `jar tf $HADOOP_HOME/share/hadoop/hdfs/hadoop-hdfs-3.2.1.jar | grep default`, `jar tf $HADOOP_HOME/share/hadoop/yarn/hadoop-yarn-common-3.2.1.jar | grep default`, and `jar tf $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-client-core-3.2.1.jar | grep default`. The information can also be found in the Apache Hadoop documentation under **$HADOOP_HOME/share/doc/hadoop**. For example, you can find them by running `ls -R $HADOOP_HOME/share/doc/hadoop | grep default.xml`. You can also find the information online.


<sup>[7](#footnote7)</sup> In Hadoop 3, YARN containers do not inherit the NodeManagers' environment variables. Therefore, if you want to inherit NodeManager's environment variables (e.g. `HADOOP_MAPRED_HOME`), you need to set additional parameters (e.g., `mapreduce.admin.user.env` and `yarn.app.mapreduce.am.env`). https://issues.apache.org/jira/browse/MAPREDUCE-6702

<sup>[8](#footnote8)</sup> We say *passphrase* instead of *password* both to differentiate it from a login password, and to stress that spaces and punctuation are allowed and encouraged.


<sup>[9](#footnote9)</sup> What the code does is to mimic the iterative process of sshing into a machine, adding the IP-address-hostname mapping of all machines to the /ect/hosts file on that machine, and sshing to all the other machine from there to do the former two steps in a nested manner.
