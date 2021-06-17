

Hive is a data warehousing (not designed for online transaction processing) infrastructure based on Apache Hadoop. Hadoop provides massive scale out and fault tolerance capabilities for data storage and processing on commodity hardware.

Hive is designed to enable easy data summarization, ad-hoc querying and analysis of large volumes of data. It provides SQL which enables users to do ad-hoc querying, summarization and data analysis easily. At the same time, Hive's SQL gives users multiple places to integrate their own functionality to do custom analysis, such as User Defined Functions (UDFs).  

In normal use, Hive runs on your workstation and converts your SQL query into a series of jobs for execution on a Hadoop cluster. Hive organizes data into tables, which provide a means for attaching structure to data stored in HDFS. Metadata—such as table sche‐ mas—is stored in a database called the metastore.



Hive was originally built by Facebook, in order to analyze and query their incoming ~15TB of data every day. It allows us to query our databases with a SQL-like syntax. It transforms database queries in efficiently chained MapReduce jobs, and is able to automatically figures out the number of reducers needed per Hive query, for instance based on the data input size.


> only need to be configured on the master node

1. Downloading Hive

We need to check that the version of Hive we'll download is compatible with the version of Hadoop we are using. Hive releases will only work against particular versions of Hadoop; this information can be found on Hive's [downlaod page](https://hive.apache.org/downloads.html).



we'll use the version 3.1.2, which works with Hadoop 3.x.

Requirements

Java
| Hive | Version	| Java | Version |
|------|---------|--------|---------|
|Hive |2.x|	Java| 7|
|Hive |3.x	|Java |8|
|Hive| 4.x|	Java |8|

Hadoop
Hadoop 1.x, 2.x
Hadoop 3.x (Hive 3.x)


2.	Unpack the downloaded Hive distribution. This will result in the creation of a subdirectory named hive-2.1.0.

```bash
$ wget https://downloads.apache.org/hive/hive-3.1.2/apache-hive-3.1.2-bin.tar.gz
$ tar -xzf apache-hive-3.1.2-bin.tar.gz
$ mv apache-hive-3.1.2-bin hive && rm apache-hive-3.1.2-bin.tar.gz
```

3.	open the .bashrc file:

```bash
$ nano .bashrc
```


Set the environment variable *HIVE_HOME* to point to the installation directory and add **$HIVE_HOME/bin** to your *PATH*:

```bash
# Hive

export HIVE_HOME=/home/hadoop/hive
export PATH=$HIVE_HOME/bin:$PATH
```

4.	LEnable the new settings immediately:

```bash
$ source  ~/.bashrc
```


All Hive installations require a metastore service, which Hive uses to store table schemas and other metadata. It is typically implemented using tables in a relational database.



Hive requires only one extra component that Hadoop does not already have: the
metastore component.


The Hive metastore is simply a relational database. It stores metadata related to the tables/schemas you create to easily query big data stored in HDFS. When you run commands such as create table x..., or alter table y..., etc., the information related to the schema (column names, data types) is stored in the Hive metastore relational database.

By default, Hive uses a built-in Derby SQL server, which provides limited, single-process storage. For example, when using Derby, you can't run two simultaneous instances of the Hive CLI.

However, because multiple users and systems are likely to need concurrent access to the metastore, the default embedded database is not suitable for production.

For clusters, MySQL or a similar relational database is required.

We'll use MySQL to store Hive's metastore information.




```bash
sudo apt-get install mysql-server
```


```bash
$ mysql --version # or mysql -V
mysql  Ver 8.0.25-0ubuntu0.20.04.1 for Linux on x86_64 ((Ubuntu))
```


```bash
sudo service mysql start
```

```bash
$ service mysql status
● mysql.service - MySQL Community Server
     Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2021-06-16 15:02:15 UTC; 6min ago
    Process: 453 ExecStartPre=/usr/share/mysql/mysql-systemd-start pre (code=exited, status=0/SUCCESS)
   Main PID: 558 (mysqld)
     Status: "Server is operational"
      Tasks: 38 (limit: 4480)
     Memory: 394.5M
     CGroup: /system.slice/mysql.service
             └─558 /usr/sbin/mysqld
```


```bash
$ sudo cat /etc/mysql/debian.cnf
$ ps aux | grep mysql
```

You can find the following lines in there

```
user     = debian-sys-maint
password = password_for_the_user
```
Then:

```bash
$ mysql -u debian-sys-maint -p
Enter password:
```
type the password from debian.cnf


```MySQL
mysql> use mysql;

mysql> CREATE USER 'hive'@'%' IDENTIFIED BY 'bigdata';

mysql> GRANT ALL PRIVILEGES ON *.* TO 'hive'@'%' WITH GRANT OPTION;

mysql>  Select host, user, plugin from user;
+-----------+------------------+-----------------------+
| host      | user             | plugin                |
+-----------+------------------+-----------------------+
| %         | hive             | caching_sha2_password |
| localhost | debian-sys-maint | caching_sha2_password |
| localhost | mysql.infoschema | caching_sha2_password |
| localhost | mysql.session    | caching_sha2_password |
| localhost | mysql.sys        | caching_sha2_password |
| localhost | root             | auth_socket           |
+-----------+------------------+-----------------------+
6 rows in set (0.00 sec)

mysql> flush privileges;

mysql> SHOW GLOBAL VARIABLES LIKE 'PORT';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| port          | 3306  |
+---------------+-------+
1 row in set (0.01 sec)


mysql> quit;  # or exit; to leave the shell
Bye
```

Account names appear in SQL statements such as CREATE USER, GRANT, and SET PASSWORD and follow these rules
https://dev.mysql.com/doc/refman/8.0/en/account-names.html

- Account name syntax is 'user_name'@'host_name'.

- The @'host_name' part is optional. An account name consisting only of a user name is equivalent to 'user_name'@'%'. For example, 'me' is equivalent to 'me'@'%'.

- The user name and host name need not be quoted if they are legal as unquoted identifiers. Quotes must be used if a user_name string contains special characters (such as space or -), or a host_name string contains special characters or wildcard characters (such as . or %). For example, in the account name 'test-user'@'%.com', both the user name and host name parts require quotes.


---
Verify you able to connect server MySQL server using the client ( Client Must be installed from which server you connecting), Verify user from ambari-server 192.168.154.111(Client) and 192.168.154.113(server) is MySQL Server IP address

mysql -u hive -h 192.168.154.113 -p


Change user password with “plugin: auth_socket”

 the plugin used for root  is auth_socket

If we want to configure a password, we need to change the plugin and set the password at the same time, in the same command. First changing the plugin and then setting the password won’t work, and it will fall back to auth_socket again. So, run:

So, the correct way to do this is to run the following:


```mysql
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'bigdata';
Query OK, 0 rows affected (0.01 sec)

```
---







```bash
$ mysql -u hive --password="bigdata"
```





hive-site.xml 和 hive-default.xml.template
hive-default.xml.template 包含预先打包在 Hive 发行版中的各种配置变量的默认值。为了覆盖任何值，请改为创建hive-site.xml并在该文件中设置该值，


the comments in hive-default.xml.template states that
hive-default.xml.template is auto generated for documentation purposes only. Any changes you make to this file will be ignored by Hive.  You must make your changes in hive-site.xml instead.


hive-site.xml should be created in the same directory.



请注意，Hive 完全不使用模板文件hive-default.xml.template(从 Hive 0.9.0 开始)–配置选项的规范列表仅在[HiveConf java](https://github.com/apache/hive/blob/master/common/src/java/org/apache/hadoop/hive/conf/HiveConf.java) 类中 Management。

在 Hive 版本 0.9.0 到 0.13.1 中，模板文件不一定包含在HiveConf.java中找到的所有配置选项，并且其某些值和描述可能已过时或与实际值和描述不同步。但是，自Hive 0.14.0起，模板文件直接由HiveConf.java生成，因此它是配置变量及其默认值的可靠来源。

Management 配置变量在below中列出。用户变量在配置单元配置属性中列出。从Hive 0.14.0开始，您可以使用SHOF CONF 命令显示有关配置变量的信息。

The curious may want to peek at the **hive-default.xml.template** file, which shows the different configuration properties supported by Hive and their default values. We can safely ignore most of these properties

Changes to your configuration can be done by creating and editing an XML configuration file called **hive-site.xml** in the **~/hive/conf** directory:



For our MySQL configuration, we need to know the host and port the service is running
on. We will assume db1.mydomain.pvt and port 3306, which is the standard MySQL
port. Finally, we will assume that hive_db is the name of our catalog.


```bash
nano hive/conf/hive-site.xml
```


We set several properties for by pasting the followig XML into the file:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>

  <property>
     <!-- Whether to print the names of the columns in query output.  -->
     <name>hive.cli.print.header</name>
     <value>true</value>
  </property>
  <property>
     <!-- Whether to include the current database in the Hive prompt. -->
     <name>hive.cli.print.current.db</name>
     <value>true</value>
  </property>

  <property>
    <name>hive.metastore.warehouse.dir</name>
    <value>/user/hive/warehouse</value>
    <description>
      HDFS or local (e.g., file:////user/hive/warehouse) directory where Hive keeps table contents. This value is appended to the value of fs.defaultFS defined in the Hadoop configuration. This directory will not be used to store the table metadata, which goes in the separate metastore.
    </description>
  </property>

  <property>
    <name>javax.jdo.option.ConnectionURL</name>
    <value>jdbc:mysql://localhost:3306/hive_metastore?createDatabaseIfNotExist=true
    </value>
    <description>
      JDBC connect string for a JDBC metastore.
      To use SSL to encrypt/authenticate the connection, provide database-specific SSL flag in the connection URL.
      For example, jdbc:postgresql://myhost/db?ssl=true for postgres database.
    </description>
  </property>

  <property>
    <!-- Driver class name for a JDBC metastore -->
    <name>javax.jdo.option.ConnectionDriverName</name>
    <value>com.mysql.cj.jdbc.Driver</value>
  </property>

  <property>
    <!-- Username to use against metastore database -->
    <name>javax.jdo.option.ConnectionUserName</name>
    <value>hive</value>
  </property>

  <property>
    <name>javax.jdo.option.ConnectionPassword</name>
    <value>bigdata</value>
  </property>

</configuration>
```


This configuration is referred to as a local metastore, since the
metastore service will run in the same process as the Hive service (e.g., CLI) and connect to a database running in a separate process on the same machine (or on a remote machine).

In this configuration, the Hive table definitions that we create will be local to our machine, so we can't share them with other users. We'll see
how to configure a shared remote metastore, which is the norm in production envi‐
ronments

```xml
<property>  
  <name>hive.metastore.uris</name>  
  <value>thrift://localhost:9083</value>  
  <description>If not set (the default), use an local (in-process) metastore; otherwise, connect to one or more remote metastores</description>
</property>
```
---
??[if we choose remote, so later when using spark-sql, avoid reconfiguration](https://programmer.help/blogs/installation-and-configuration-of-hive.html)
---


You may have noticed the ConnectionURL property starts with a prefix of `jdbc:mysql`.

In this case, the `javax.jdo.option.ConnectionURL` property is set to `jdbc:mysql://host/dbname?createDatabaseIfNotExist=true`, and `javax.jdo.option.ConnectionDriverName` is set to `com.mysql.jdbc.Driver`. The username and password are set too, of course.


, there’s another metastore configuration called a remote meta‐
store, where one or more metastore servers run in separate processes to the Hive service.
This brings better manageability and security because the database tier can be com‐
pletely firewalled off, and the clients no longer need the database credentials.
A Hive service is configured to use a remote metastore by setting hive.meta
store.uris to the metastore server URI(s), separated by commas if there is more than one. Metastore server URIs are of the form thrift://host:port, where the port corresponds to the one set by METASTORE_PORT when starting the metastore server

```xml

<property>
 <name>hive.metastore.uris</name>
 <value>thrift://localhost:9083</value>
 <description>By specify this we do not use local mode of metastore</description>
</property>

```


For Hive to be able to connect to MySQL, we need to place the JDBC driver in Hive's classpath, which is simply achieved by placing it in Hive's **$HIVE_HOME/lib** directory.

Download the MySQL JDBC driver (Jconnector) from  https://dev.mysql.com/downloads/connector/j/ (deb file) or https://mvnrepository.com/artifact/mysql/mysql-connector-java (mysql-connector-java-8.*.*.jar jar file).






[How to install a deb file?](https://unix.stackexchange.com/questions/159094/how-to-install-a-deb-file-by-dpkg-i-or-by-apt)



```bash

$ sudo apt install ./mysql-connector-java_8.0.25-1ubuntu20.04_all.deb

```

Make sure that the MySQL JDBC driver is available at **$HIVE_HOME/lib**:

```bash
$ ln -s /usr/share/java/mysql-connector-java-8.0.25.jar hive/lib
```

With the driver and the configuration settings in place, Hive will store its metastore information in MySQL.



**guava-*-jre.jar** version under hadoop libraries and hive libraries can be different. They need to be of same version. Otherwise, you will get the error below:

```
Exception in thread "main" java.lang.NoSuchMethodError: com.google.common.base.Preconditions.checkArgument(ZLjava/lang/String;Ljava/lang/Object;)V
```


let's ensure guava library version is consistent between Hive and Hadoop.

```bash
$ ls hive/lib | grep guava
$ ls hadoop/share/hadoop/hdfs/lib | grep guava
```

To avoid issues like the one described on page:

https://issues.apache.org/jira/browse/HIVE-22718
https://issues.apache.org/jira/browse/HIVE-22915



Problem temporarily solved by replacing its guava-19.0.jar with Hadoop's guava-27.0-jre.jar



```bash
$ rm hive/lib/guava-19.0.jar
$ cp hadoop/share/hadoop/hdfs/lib/guava-27.0-jre.jar hive/lib
```




Starting from Hive 2.1, we need to run the [schematool command](https://cwiki.apache.org/confluence/display/Hive/Hive+Schema+Tool) as an initialization step.

This tool can be used to initialize the metastore schema for the current Hive version. It can also handle upgrading the schema from an older version to current. It tries to find the current schema from the metastore if it is available. In case of upgrades from older releases like 0.7.0 or 0.10.0, you can specify the schema version of the existing metastore as a command line option to the tool.

```bash
$ schematool -help
```
The schematool figures out the SQL scripts required to initialize or upgrade the schema and then executes those scripts against the backend database. The metastore DB connection information like JDBC URL, JDBC driver and DB credentials are extracted from the Hive configuration.  



```bash
$ schematool -initSchema -dbType mysql
```

For the flag dbType, it can be any of the following values `derby|mysql|postgres|oracle|mssql`



to get schema information:
```bash
$ schematool -dbType mysql -info
```

### Running Hive


Hive uses Hadoop, so:

- you must have Hadoop in your path OR
- `export HADOOP_HOME=<hadoop-install-dir>`

In addition, we should /tmp and /user/hive/warehouse on HDFS (aka hive.metastore.warehouse.dir that was configured with **$HIVE_HOME/conf/hive-site.xml**) and set them `chmod g+w` before you can create a table in Hive.

```bash
hadoop fs -mkdir /tmp
hadoop fs -chmod g+w /tmp
hadoop fs -mkdir -p /user/hive/warehouse
hadoop fs -chmod g+w /user/hive/warehouse
```
Or alternatively, directly run the following command:

```bash
$ $HIVE_HOME/bin/init-hive-dfs.sh
```

We’ll also assume that you have added $HIVE_HOME/bin to your environment’s PATH so
you can type hive at the shell prompt and your shell environment (e.g., bash) will find the command.


The $HIVE_HOME/bin directory contains executable scripts that launch various Hive services, including the hive command-line interface (CLI). The CLI is the most popular way to use Hive. We will use hive (in lowercase, with a fixed-width font) to refer to the CLI, except where noted. The CLI can be used interactively to type in statements one at a time or it can be used to run “scripts” of Hive statements, as we’ll see. Hive


### Running Hive CLI





we'll use the **$HIVE_HOME/bin/hive** command, which is a bash shell script, to start the Hive [command line interface (CLI)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) and use it to play around:

```bash
$ hive --service cli # or simply hive because the CLI is the default service
```


The shell is the primary way that we will interact with Hive, by issuing commands in
HiveQL. HiveQL is Hive's query language, a dialect of SQL. It is heavily influenced by
MySQL, so if you are familiar with MySQL, you should feel at home using Hive

Like SQL, HiveQL is generally case insensitive (except for string comparisons), so show
tables; works equally well here. The Tab key will autocomplete Hive keywords and
functions.

```hive
hive> SHOW TABLES;  # list all tables
hive> SHOW TABLES '.*s'; # list all tables that end with 's'
hive> DESCRIBE invites;  # show the list of columns.
```




You can also run the Hive shell in noninteractive mode. The `-f` option runs the com‐
mands in the specified file, which is script.q in this example:

```bash
$ hive -f script.q
```

For short scripts, you can use the `-e` option to specify the commands inline, in which
case the final semicolon is not required:

```bash
$ hive -e 'SELECT * FROM dummy'
```


In both interactive and noninteractive mode, Hive will print information to standard
error—such as the time taken to run a query—during the course of operation. You can
suppress these messages using the -S option at launch time, which has the effect of
showing only the output result for queries:

```bash
$ hive -S -e 'SELECT * FROM dummy'
```

Other useful Hive shell features include the ability to run commands on the host op‐
erating system by using a `!` prefix to the command and the ability to access Hadoop
filesystems using the `dfs` command (add the semicolon at the end):


```hive
hive> dfs -ls /;
hive> dfs -ls /tmp;
```

This method of accessing hadoop commands is actually more efficient than using the
`hadoop dfs ...` equivalent at the bash shell, because the latter starts up a new JVM
instance each time, whereas Hive just runs the same code in its current process.

You can see a full listing of help on the options supported by `dfs` using this command:

```hive
hive> dfs -help;
```



Hive also permits you to set properties on a per-session basis, by passing the -hiveconf
option to the hive command. For example, the following command sets the cluster (in
this case, to a pseudodistributed cluster) for the duration of the session:


```bash
$ hive -hiveconf fs.defaultFS=hdfs://localhost \
 -hiveconf mapreduce.framework.name=yarn \
 -hiveconf yarn.resourcemanager.address=localhost:8032
```


 There is a precedence hierarchy to setting properties. In the following list, lower num‐ bers take precedence over higher numbers:


1. The Hive SET command
2. The command-line -hiveconf option
3. hive-site.xml and the Hadoop site files (core-site.xml, hdfs-site.xml, mapredsite.xml, and yarn-site.xml)
4. The Hive defaults and the Hadoop default files (core-default.xml, hdfs-default.xml,
mapred-default.xml, and yarn-default.xml;Hive configuration is an overlay on top of Hadoop – it inherits the Hadoop configuration variables by default)


Execution engines
Hive was originally written to use MapReduce as its execution engine, and that is still
the default. It is now also possible to run Hive using Apache Tez as its execution engine,
and work is underway to support Spark


The execution engine is controlled by the hive.execution.engine property, which
defaults to mr (for MapReduce). It’s easy to switch the execution engine on a per-query
basis, so you can see the effect of a different engine on a particular query. Set Hive to
use Tez as follows:
hive> SET hive.execution.engine=tez;
Note that Tez needs to be installed on the Hadoop cluster first


The Hive shell is only one of several services that you can run using the hive command.
You can specify the service to run using the --service option. Type hive --service
help to get a list of available service names; some of the most useful ones are described
in the following list:
cli
The command-line interface to Hive (the shell). This is the default service.


The Hive shell is only one of several services that you can run using the hive command


Hive Web Interface (component removed as of Hive 2.2.0)
