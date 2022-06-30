

## Start EMR

Edit software settings:

Enter configurations

```
classification=core-site, properties=[hadoop.http.staticuser.user=hadoop]
classification=hdfs-site, properties=[dfs.block.size=16M, dfs.replication=3, dfs.webhdfs.enabled=true]
classification=hive-site, properties=[hive.execution.engine=mr]
```

# SSH into the instance

```bash
$ hive --version
Hive 3.1.2-amzn-4
Git file:///codebuild/output/src037254793/src/build/hive/rpm/BUILD/apache-hive-3.1.2-amzn-4-src -r Unknown
Compiled by release on Wed Mar 31 01:06:19 UTC 2021
From source with checksum ce52ab0efc78ab65267e0be8ef6ddbab

$ sudo lsof -n -i :9083 | grep LISTEN              # metastore server ??
java    2019 hive  560u  IPv6 148149      0t0  TCP *:emc-pp-mgmtsvc (LISTEN)

$ sudo lsof -ni :9083
COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
java    2019 hive  560u  IPv6 148149      0t0  TCP *:emc-pp-mgmtsvc (LISTEN)
java    2019 hive  586u  IPv6 162876      0t0  TCP 172.31.90.188:emc-pp-mgmtsvc->172.31.90.188:33536 (ESTABLISHED)
java    2019 hive  587u  IPv6 199614      0t0  TCP 172.31.90.188:emc-pp-mgmtsvc->172.31.90.188:33596 (ESTABLISHED)
java    7227 hive  553u  IPv6 163986      0t0  TCP 172.31.90.188:33536->172.31.90.188:emc-pp-mgmtsvc (ESTABLISHED)
java    7227 hive  570u  IPv6 199613      0t0  TCP 172.31.90.188:33596->172.31.90.188:emc-pp-mgmtsvc (ESTABLISHED)


$ sudo netstat -nlp | grep :9083
tcp6       0      0 :::9083                 :::*                    LISTEN      2019/java

$  mysql --version
mysql  Ver 15.1 Distrib 5.5.68-MariaDB, for Linux (x86_64) using readline 5.1


$ systemctl --type=service | grep -i mariadb    # MariaDB is a distribution of mysql?
mariadb.service                        loaded active running MariaDB database server

$ sudo mysql
```

```sql


MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| hive               |
| hue                |
| mysql              |
| performance_schema |
+--------------------+
5 rows in set (0.00 sec)

MariaDB [(none)]> use hive;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [hive]> show tables;
MariaDB [hive]> exit;
Bye
```
