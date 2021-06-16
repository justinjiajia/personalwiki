

# Installing Pig and Running Pig


Pig does not need to be installed on a Hadoop cluster. It runs on the machine from which we launch Hadoop jobs as a **client-side application**. In practice, a cluster owner sets up one or more machines that have access to the Hadoop cluster but are not part of the cluster (i.e., they are not DataNodes or NodeManagers). But Pig needs to know the configuration of the cluster (e.g., the location of the cluster's NameNode and ResourceManager). The configurations can be found in **core-site.xml**, **hdfs-site.xml**, **yarn-site.xml**, and **mapred-site.xml**. Make sure that these files are present on the machine from where we interact with the Hadoop cluster.





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

We need to add **~/pig/bin** to the environment variable *PATH* for running the Pig script file<sup><a href="#footnote1">1</a></sup><sup><a href="#footnote2">2</a></sup><sup><a href="#footnote3">3</a></sup>. Also, in order to run the Pig scripts in MapReduce mode, we need to set the *PIG_CLASSPATH* environment variable to the location of the cluster configuration directory (the directory that contains the core-site.xml, hdfs-site.xml, yarn-site.xml, and mapred-site.xml files).

Append the following code to the bottom of **.bashrc**:


```shell
# Pig

export PATH=$/home/hadoop/pig/bin:$PATH
export PIG_CLASSPATH=$HADOOP_CONF_DIR

```


Press <kdb>CTRL</kdb>+<kdb>X</kdb> to save the change and exit nano and apply changes:

```shell
$ source ~/.bashrc
```
Test the Pig installation with a simple command `pig -help` or `pig -h`:


It also lists a number of command-line options that you can use with Pig. Most of these options will be discussed later, in the sections that cover the features these options control.


## 2. Running Pig Locally in Interactive Mode

We can run Pig in local mode interactively using the **Grunt shell**. Local mode helps prototype and debug the Pig Latin scripts on small representative datasets before we apply the same processing to large data<sup><a href="#footnote4">4</a></sup>.   



Invoke the Grunt shell using the `pig` command, setting the execution type to local by using the `-x` or `-exectype` flag as shown below:

```bash
$ pig -x local
```

Now we can start entering our Pig Latin statements and Pig commands interactively at the Grunt shell.

Grunt has line-editing facilities like those used in the bash shell and many other command-line applications. For instance, the <kbd>Ctrl</kbd>+<kbd>E</kbd> key combination will move the cursor to the end of the line, while <kbd>Ctrl</kbd>+<kbd>A</kbd> will move the cursor to the start of the line. Grunt remembers command history, and we can recall lines in the history buffer using the up or down cursor keys.

Another handy feature is Grunt's completion mechanism, which will try to complete Pig Latin keywords and functions when we press the <kdb>Tab</kdb> key. For example, consider the following incomplete line:




```pig
grunt> a = foreach b ge
```


If we press the <kdb>Tab</kdb> key at this point, `ge` will expand to generate, a Pig Latin keyword:


```pig

grunt> a = foreach b generate

```


A number of commands that come directly from Unix shells can operate in ways that are familiar: these include `cat`, `cp`, `ls`, `mkdir`, `mv`, `rm`, and so on:

```pig
grunt> pwd
grunt> cd ~/pig
grunt> ls
```

We can get a list of commands using the `help` command.  When we've finished the Grunt session, we can terminate the Grunt session with the `quit` command, the shortcut `\q`, or <kdb>Ctrl</kdb>+<kdb>D</kdb>. If we are in the middle of executing a Pig script, we can use <kdb>Ctrl</kdb>+<kdb>C</kdb> to terminate the execution.

Let's exit Grunt for now:


```pig
grunt> quit
```





Prepare the datasets to be processed by executing the following commands:

```bash
$ mkdir most_visited
$ echo "amy	,cnn.com,30
amy,bbc.com,20
amy,flickr.com,15
fred,cnn.com,15" > most_visited/visits
$ echo "cnn.com,news,0.9
bbc.com,news,0.8
flickr.com,photos,0.7
espn.com,sports,0.9" > most_visited/urls

```

Restart the Grunt shell and run the following pig Latin statements sequentially to find the most-visited websites for each category as exemplified in our lecture.

```pig
grunt> visits  = LOAD 'most_visited/visits' using PigStorage(',') AS (user:chararray, url:chararray, time:double);
grunt> gVisits  = GROUP visits BY url;
grunt> visitCounts  = FOREACH gVisits GENERATE group as url, COUNT(visits) as count;
grunt> urls  = LOAD 'most_visited/urls' using PigStorage(',') AS (url:chararray, category:chararray, pagerank:double);
grunt> jVisitCounts  = JOIN visitCounts BY url, urls BY url;
grunt> gCategories  = GROUP jVisitCounts BY category;
grunt> topUrls  = FOREACH gCategories GENERATE  group As category, TOP(1, 1, jVisitCounts);
grunt> STORE topUrls into 'most_visited/topUrls';
```

During the process, we can use diagnostic operations (`dump`, `describe`, `illustrate`, etc.) freely to figure out the content and schemas of the intermediate relations.

The result should be a lot of output on the screen. Much of this is MapReduce's LocalJobRunner generating logs. But some of it is Pig telling us how it will execute the script, giving us the status as it executes, etc.

Near the bottom of the output we should see the simple message **Success!**. This means all went well. The script stores its output to a local directory named **topUrls** that contains a file named **part-r-00000**. Enter the following to see this:


```pig
grunt> ls most_visited/topUrls
grunt> cat most_visited/topUrls/part-r-00000
```





## 3. Running Pig with UDFs Locally in Batch Mode (Optional)

Terminate the Grunt shell and come back to the bash shell. Prepare the datasets to be processed and the Java source code of the user defined function by executing the following commands:

```bash
$ mkdir revenue_attribution
$ echo "lakers,top,50
Lakers,side,20
Kings,top,30
Kings,side,10" > revenue_attribution/revenues
$ echo "lakers,nba.com,1
Lakers,espn.com,2
Kings,nhl.com,1
Kings,nba.com,2" > revenue_attribution/results
$ mkdir myudfs
$ cd myudfs && nano DistRev.java
```

Copy the code below into this file.


? java code 有问题需要改

```java
package myudfs;
import java.io.IOException;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.Map;

import org.apache.pig.EvalFunc;
import org.apache.pig.data.DataBag;
import org.apache.pig.data.DataType;
import org.apache.pig.data.DefaultBagFactory;
import org.apache.pig.data.Tuple;
import org.apache.pig.data.TupleFactory;
import org.apache.pig.impl.logicalLayer.FrontendException;
import org.apache.pig.impl.logicalLayer.schema.Schema;


public class DistRev extends EvalFunc<DataBag> {

        public DataBag exec(Tuple input) throws IOException {
                if (input == null || input.size()!=2) return null;
                try{
                        DataBag output = DefaultBagFactory.getInstance().newDefaultBag();
                        DataBag in1 = (DataBag)input.get(0);
                        DataBag in2 = (DataBag)input.get(1);
                        Map<String, Double> adSlots = new HashMap<String, Double>();
                        Map<String, Long> ranks = new HashMap<String, Long>();

                        Iterator<Tuple> it1 = in1.iterator();
                        Iterator<Tuple> it2 = in2.iterator();

                        while (it2.hasNext()) {
                                Tuple t = it2.next();
                                String adSlot = (String)t.get(1);
                                Double amount = (Double)t.get(2);
                                adSlots.put(adSlot, amount);
                        }

                        while (it1.hasNext()) {
                                Tuple t = it1.next();
                                String url = (String)t.get(1);
                                Long rank = (Long)t.get(2);
                                ranks.put(url, rank);
                        }

                        Iterator<String> it = ranks.keySet().iterator();
                        int rankSize = ranks.size();
                        while (it.hasNext()) {
                                String url = it.next();
                                Long rank = ranks.get(url);
                                Tuple t = TupleFactory.getInstance().newTuple(2);
                                t.set(0, url);
                                if (rank == 1) t.set(1, adSlots.get("top")+ adSlots.get("side")/ rankSize);
                                else t.set(1, adSlots.get("side")/ rankSize);
                                output.add(t);
                        }
                        return output;
                }catch (Exception e){
                        System.err.println("DistRev: failed to process input; error - " + e.getMessage());
                        return null;
                }
        }

        public Schema outputSchema(Schema input) {
                Schema bagSchema = new Schema();
                bagSchema.add(new Schema.FieldSchema("url", DataType.CHARARRAY));
                bagSchema.add(new Schema.FieldSchema("amount", DataType.DOUBLE));
                try{
                        return new Schema(new Schema.FieldSchema(getSchemaName(this.getClass().getName().toLowerCase(),input), bagSchema, DataType.BAG));               
                }catch (FrontendException e){
                        return null;
                }
        }
}
 ```

Save it and exit the nano editor. The **Hadoop jar** and the **pig jar** that contain required libraries are needed to compile the Java source code. Executing

```bash
$ hadoop classpath
```

prints the class path needed to get the **Hadoop jar**. Since we downloaded the binary distribution of Pig, **pig.jar** is part of it. Copy **pig-*.jar** from **~/pig** to **myudfs** directory by issuing:

```bash
$ cp ~/pig/pig-*.jar pig.jar
```

To compile the Java source code, we use the javac command<sup><a href="#footnote5">5</a></sup> with `-cp` option specifying where to find required class files:

```bash
$ javac -cp pig.jar:`hadoop classpath`  DistRev.java
```


Use the jar tool to bundle the bytecode class files into a jar file called **myudfs.jar**. Move the jar file into **revenue_attribution**:

```bash
$ cd ..
$ jar -cf myudfs.jar myudfs
$ mv myudfs.jar revenue_attribution
$ cd revenue_attribution
```



Use the nano editor to create a pig script called **revenue_attribution.pig**:

```bash
$ nano revenue_attribution.pig
```



Paste the following code into the opened editor. Save the change and exit it.

```pig
register myudfs.jar;
results = load 'results' as (query:chararray, url:chararray, rank:long);
revenues = load 'revenues' as (query:chararray, adSlot:chararray, amount:double);
grouped_data = cogroup results by query, revenues by query;
url_revenues = foreach grouped_data generate group, myudfs.DistRev(results, revenues);
dump url_revenues;
```

Enter the following code to run the pig script in batch mode:

```bash
pig -x local revenue_attribution.pig
```





## 	MapReduce mode

In MapReduce mode, Pig translates queries into MapReduce jobs and runs them on a Hadoop cluster. MapReduce mode with a fully distributed cluster is what you use when you want to run Pig on large datasets.

To use MapReduce mode, you need to check that the version of Pig you downloaded is compatible with the version of Hadoop you are using. Pig releases will only work against particular versions of Hadoop; this is documented in the [release notes](https://pig.apache.org/releases.html).


Pig honors the `HADOOP_HOME` environment variable for finding which Hadoop client to
run. However, if it is not set, Pig will use a bundled copy of the Hadoop libraries. Note that these may not match the version of Hadoop running on your cluster, so it is best to explicitly set `HADOOP_HOME` (we have done this in the AMI we use).

Next, you need to point Pig at the cluster's namenode and resource manager. If the
installation of Hadoop at `HADOOP_HOME` is already configured for this, then there is nothing more to do. Otherwise, you can set `HADOOP_CONF_DIR` to a directory containing the Hadoop site file (or files) that define fs.defaultFS, yarn.resourcemanager.address,
and mapreduce.framework.name (agian, we have done this in the AMI we use).


Alternatively, you can set these properties in the [**~pig/conf/pig.properties** file](https://pig.apache.org/docs/r0.17.0/start.html#properties).

In MapReduce mode, we can optionally enable auto-local mode (by setting pig.auto.local.enabled to true), which is an optimization that runs small jobs locally if the input is less than 100 MB (set by pig.auto.local.input.maxbytes, default 100,000,000) and no more than one reducer is being used.



Once we have configured Pig to connect to a Hadoop cluster, we proceed to launch the set of daemons necessary to run a Pig Latin program in the MapReduce mode.

Apart from regular daemons, Pig needs one more daemon to be running on Hadoop called a job history server. The job history server maintains information about MapReduce jobs after their Application Master terminates. All the job history data per job is persisted to HDFS by the MapReduce ApplicationMaster.


 ```bash
$ hdfs namenode -format
$ start-dfs.sh
$ start-yarn.sh
$ mapred --daemon start historyserver
```

`jps` prints:

```
JobHistoryServer
Jps
ResourceManager
NameNode
```

We need only one MapReduce job history server per cluster. It can run on any node we like, including a dedicated node of its own, but traditionally runs on the same node as the resourcemanager. The job history server is declared in mapred-site.xml. Its configurational options can be found in the online manual for [Hadoop Cluster Setup](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/ClusterSetup.html).



---

As of Hadoop 2.5.1, the YARN timeline server does not yet store MapReduce job history, so a MapReduce job history server daemon is still needed

???but 3.2.1 seems still need to use mapreduce job history server???
10200 is the port for mapreduce.jobhistory.address	0.0.0.0:10020

yarn.timeline-service.address	${yarn.timeline-service.hostname}:10200


2021-06-14 09:28:19,241 [main] INFO  org.apache.hadoop.yarn.client.AHSProxy - Connecting to Application History server at /0.0.0.0:10200
...
org.apache.hadoop.mapred.ClientServiceDelegate - Application state is completed. FinalApplicationStatus=SUCCEEDED. Redirecting to job history server
2021-06-14 09:28:52,476 [main] INFO  org.apache.hadoop.ipc.Client - Retrying connect to server: 0.0.0.0/0.0.0.0:10020. Already tried 0 time(s); retry policy is RetryUpToMaximumCountWithFixedSleep(maxRetries=10, sleepTime=1000 MILLISECONDS)


some
```bash
$ yarn --daemon start timelineserver
```


```
JobHistoryServer
Jps
ResourceManager
NameNode
ApplicationHistoryServer
```

---

The Hadoop UIs are reachable through the following:

-	http://PUBLIC_IP_OF_RESOURCEMANAGER:8088 – <a href="https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-common/yarn-default.xml#yarn.resourcemanager.webapp.address" target="_blank">Resource Manager</a>

-	http://PUBLIC_IP_OF_NAMENODE:9870 – <a href="https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/hdfs-default.xml#dfs.namenode.http-address" target="_blank">NameNode</a>

-	http://PUBLIC_IP_OF_JOBHISTORY_SERVER:19888 – <a href="https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml#mapreduce.jobhistory.webapp.address" target="_blank">MapReduce JobHistory Server</a>




### WordCount with Pig

#### Data Preparation

We prepare the same set of text files as before by sequentially execute the following commands:


```shell
$ cd ~
$ mkdir data
$ cd data
$ wget https://archive.org/download/encyclopaediabri31156gut/pg31156.txt
$ wget https://archive.org/download/encyclopaediabri34751gut/pg34751.txt
$ wget https://archive.org/download/encyclopaediabri35236gut/pg35236.txt
$ cd ..
$ (hadoop or pig -e) fs -put data /
```

There are a number of command-line options that we can use with Pig. We can see the full list by entering `pig -h`.  

`-e` or `-execute` execute a single command in Pig. For example, `pig -e fs -ls /` lists your home directory on HDFS.


With HDFS, YARN, and the job history server daemons running on the cluster and input data prepared on HDFS, we can launch Pig, setting the `-x` option to `mapreduce` or omitting it entirely, as MapReduce mode is the default. And we can also use the `-brief` option to stop timestamps from being logged:

```bash
$ pig -x mapreduce -brief
```

As we can see from the output, Pig reports the file system (but not the YARN resource manager) that it has connected to (Connecting to hadoop file system at: **hdfs://master:9000**). The Grunt shell is started.



Besides entering Pig Latin interactively, Grunt's other major use is to act as a shell for HDFS. All `hadoop fs` shell commands are available in Pig. They are accessed using the keyword `fs`. The dash (-) used in `hadoop fs` is also required:


```pig
grunt> fs -ls
```



Then, we start to interact with the Grunt shell. First, we load the input data from HDFS and define a Pig bag called `lines`:


```pig
grunt> lines = LOAD '/data' AS (line:chararray);

```



Then we extract words from each line, put them into a bag, and flatten the bag to get one word on each row:



```pig
grunt> words = FOREACH lines GENERATE FLATTEN(TOKENIZE(line)) AS word;
```




We further filter out any words that are just white spaces:

```pig
grunt> filtered_words = FILTER words BY word MATCHES '\\w+';
```







Then create a group for each word:

```pig
grunt> word_groups = GROUP filtered_words BY word;
```


Count the entries in each group:

```pig
grunt> word_count = FOREACH word_groups GENERATE COUNT(filtered_words) AS count, group AS word;
```




Order the records by count:


```pig
grunt> ordered_word_count = ORDER word_count BY count DESC;
```


Use the `STORE` operator possibly with the load/store functions to write final results to the HDFS (`PigStorage` is the default store function):


```pig
grunt> STORE ordered_word_count INTO '/output';

```

Note that as we issue Pig Latin commands, the Pig interpreter parses them and verifies that the input files and bags being referred to by the command are valid. During this process, Pig constructs a logical plan for every bag that we define.



However, no data processing is actually carried out until we invoke the `STORE` command. At that point, the logical plan is compiled into a physical plan, and is executed.  



The above Pig Latin statements will be compiled into 3 consecutive MapReduce jobs. Pig stores the intermediate data generated between MapReduce jobs in a temporary location on HDFS. This location can be configured using the **pig.temp.dir** property. The property's default value is **/tmp**.







Note: During the testing/debugging phase of your implementation, you can use `DUMP` to display results to your screen. However, in a production environment, you always want to use the `STORE` operator to save your results (see [Store vs. Dump](https://pig.apache.org/docs/r0.17.0/perf.html#store-dump)).



Pig supports a number of [properties](https://pig.apache.org/docs/r0.17.0/start.html#properties) that you can use to customize Pig behavior. You can retrieve a list of the properties using the [`pig -help properties` command](http://pig.apache.org/docs/r0.17.0/cmds.html#help). All of these properties are optional; none are required.



5. Running Pig in Batch Mode

In addition to working with the Grunt shell, we can also run a Pig Latin program in batch mode using Pig scripts and the `pig` command (in local or mapreduce mode).

We combine the Pig Latin statements used in the previous section into a Pig script (use `nano wordcount.pig`) with annotated comments. For multi-line comments we use `/*...*/`, and for single-line comments we use `--`.


```pig
/* wordcount.pig
some explanatary text
*/

lines = LOAD '/data' AS (line:chararray);

-- Extract words from each line and put them into a pig bag
-- datatype, then flatten the bag to get one word on each row

words = FOREACH lines GENERATE FLATTEN(TOKENIZE(line)) AS word;

-- filter out any words that are just white spaces

filtered_words = FILTER words BY word MATCHES '\\w+';

--create a group for each word

word_groups = GROUP filtered_words BY word;

-- count the entries in each group

word_count = FOREACH word_groups GENERATE COUNT(filtered_words) AS count, group AS word;

-- order the records by count

ordered_word_count = ORDER word_count BY count DESC;

STORE ordered_word_count INTO '/output';
```

To run the script in mapreduce mode, we simply type:

```bash
$ pig -x mapreduce wordcount.pig
```
Note: you can also change the input/output directories to those on the local file system, and issue the following command to run the pig script in local mode,

$ pig -x mapreduce wordcount.pig


<sup>[1](#footnote1)</sup> **~/pig/bin/** contains the Pig script file, [**pig**](https://github.com/apache/pig/blob/trunk/bin/pig), which describes and sets Pig's environment variables, such as *PIG_CONF_DIR** and *PIG_CONF_DIR*. **~/pig/conf** contains the Pig properties file, [**pig.properties**](https://github.com/apache/pig/blob/trunk/conf/pig.properties).

<sup>[2](#footnote2)</sup>	You need to make sure that the environment variable *JAVA_HOME* is set to the directory that contains your Java distribution. Pig will fail immediately if this value is not in the environment. You can set this in your shell (we have done this in the AMI we use), or specify it on the command line when you invoke Pig.

<sup>[3](#footnote3)</sup> If you have already added **~hadoop/bin/hadoop** into the *PATH* variable, **~/pig/bin/pig** will automatically configure Hadoop-related environment variable, such as [*HADOOP_BIN*](https://github.com/apache/pig/blob/trunk/bin/pig#L288) and [*HADOOP_HOME*](https://github.com/apache/pig/blob/trunk/bin/pig#L318), so that Pig can use the right Hadoop library from your local Hadoop distribution. But under certain circumstances, it is best to explicitly set *HADOOP_HOME*.

<sup>[4](#footnote4)</sup> When running locally, Pig uses the Hadoop class **LocalJobRunner**, which reads from the local filesystem and executes MapReduce jobs locally. This has the nice property that Pig jobs run locally in the same way as they will on our cluster.


<sup>[5](#footnote5)</sup> Invoke Java programming language compiler
