

When spinning up a new cluster , you can use EMR Configurations API to change appropriate values. http://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-configure-apps.html
https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-configure-apps.html

```
classification=core-site, properties=[hadoop.http.staticuser.user=hadoop]
classification=hdfs-site, properties=[dfs.block.size=16M, dfs.replication=3]
classification=hive-site, properties=[hive.execution.engine=mr]
```

s3://isom5170/emr_hive.json
```JSON
[
  {
    "Classification": "core-site",
    "Properties": {
      "hadoop.http.staticuser.user": "hadoop"
    }
  },
  {
    "Classification": "hdfs-site",
    "Properties": {
      "dfs.block.size": "16M",
      "dfs.replication": "3"
    }
  },
  {
      "Classification": "hive-site",
      "Properties": {
        "hive.execution.engine": "mr"
      }
  }
]
```


EMR configuration files are located in `/etc`



#### `hdfs-site.xml`


`hdfs-site.xml` is in `/etc/hadoop/conf`

```BASH
nano /etc/hadoop/conf/hdfs-site.xml
```

#### `mapred-site.xml`




`mapreduce.cluster.local.dir`

https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml

```BASH
nano /etc/hadoop/conf.empty/mapred-site.xml
```

```xml
<property>
   <name>mapreduce.cluster.local.dir</name>
   <value>/mnt/mapred,/mnt1/mapred</value>
 </property>

```


#### `yarn-site.xml`

Well, the yarn-site.xml and capacity-scheduler.xml are indeed under correct locations (/etc/hadoop/conf.empty/)
and on running cluster , editing them on master node and restarting YARN RM Daemon will change the scheduler.

```BASH
nano /etc/hadoop/conf.empty/yarn-site.xml
```

#### `hive-site.xml`



`hive-site.xml` is in `/etc/hive/conf`

On EMR, By default, Hive records metastore information in a MySQL database on the master node's file system.


```BASH
nano /etc/hive/conf/hive-site.xml
```

```XML

<property>
  <name>hive.execution.engine</name>
  <value>mr</value>
</property>


<property>
    <name>fs.defaultFS</name>
    <value>hdfs://ip-172-31-80-210.ec2.internal:8020</value>
</property>

<property>
   <name>hive.metastore.uris</name>
   <value>thrift://ip-172-31-80-210.ec2.internal:9083</value>
   <description>JDBC connect string for a JDBC metastore</description>
 </property>

<property>
  <name>javax.jdo.option.ConnectionURL</name>
  <value>jdbc:mysql://ip-172-31-80-210.ec2.internal:3306/hive?createDatabaseIfNotExist=true</value>
  <description>username to use against metastore database</description>
</property>

<property>
  <name>javax.jdo.option.ConnectionDriverName</name>
  <value>org.mariadb.jdbc.Driver</value>
  <description>username to use against metastore database</description>
</property>

<property>
  <name>javax.jdo.option.ConnectionUserName</name>
  <value>hive</value>
  <description>username to use against metastore database</description>
</property>

<property>
  <name>javax.jdo.option.ConnectionPassword</name>
  <value>CwS88SncyEzJXP72</value>


<property>
   <name>hive.exec.mode.local.auto</name>
   <value>true</value>
 </property>


```

 file:///C:/References/Unknown/2015/White%20-%20Hadoop%20The%20Definitive%20Guide.pdf
P478-482

By default, the metastore is run in the same process as the Hive service.
But it is possible to run the metastore as a standalone (remote) process (as a server). Set the
METASTORE_PORT environment variable (or use the -p command-line option) to
specify the port the server will listen on (defaults to 9083).


A Hive service is configured to use a standalone (remote) metastore by setting hive.meta
store.uris to the metastore server URI(s), separated by commas if there is more than
one. Metastore server URIs are of the form thrift://host:port, where the port
corresponds to the one set by METASTORE_PORT when starting the metastore server.

Hive Metadata Server

The Hive Metadata server can only be accessed by the trusted engines, specifically the Hive and emr_record_server to protect from unauthorized access.
The Hive metadata server is also accessed by all nodes on the cluster and required port 9083 is accessible by all nodes to the master node.
