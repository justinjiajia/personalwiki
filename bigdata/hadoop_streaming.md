



# Hadoop Streaming



[Hadoop streaming](https://hadoop.apache.org/docs/current/hadoop-streaming/HadoopStreaming.html) is a utility that comes with the Hadoop distribution. It allows us to create and run MapReduce jobs with any executable or script as the mapper and/or the reducer. For example, both the mapper and the reducer can be just normal Linux executables:

```bash
$ mapred streaming \
> -input myInputDirs \
> -output myOutputDir \
> -mapper /bin/cat \
> -reducer /usr/bin/wc
```

- When the MapReduce framework 's mapper/reducer is initialized, each map/reduce task launches the specified executable as a separate process (referred to as the streaming mapper/reducer).

- The framework's mapper/reducer converts input key/value pairs to lines by concatenating the key and the value with the tab character `\t` (the actual separator to use can be set via the `stream.<map/reduce>.input.field.separator` option) and present them to the executable via stdin.

- The streaming mapper and streaming reducer both read input, line by line, from the standard input ([stdin](http://en.wikipedia.org/wiki/Standard_streams)), and write output to the standard output (stdout).

- After the streaming mapper/reducer processes each line of input, the framework's mapper/reducer collects the output from stdout and converts each line to an output key/value pair.

- When the MapReduce framework performs a conversion of lines to key/value pairs,  it splits the line into a key/value pair. By default, the prefix of a line up to the 1st tab character  (`'\t'`) (the separator can be customized via the `stream.<map/reduce>.output.field.separator` option) is the key and the rest of the line (excluding the tab character) will be the value. If there is no tab character in the line, then entire line is considered as key and the value is null. 

- The [TextInputFormat](https://hadoop.apache.org/docs/current/api/org/apache/hadoop/mapreduce/lib/input/TextInputFormat.html) is used as the default input format class. Since the TextInputFormat returns keys of LongWritable class, which are actually not part of the input data, the keys will be discarded; only the values will be piped to the streaming mapper.

  

  



Starting from Ubuntu 20.04, Python 3 is included in the base system installation.


```bash
$ which python3
```

You can run the following code to check the exact Python version installed:

```shell
$ python3 --version
```

For the word count application, the Python code for the mapper can be specified as follows[^1]:

[^1]: `#!/usr/bin/env python3` must be included as the first line in a Python script, as it instructs the program loader of a Unix-like operating system which program (i.e., the Python 3 interpreter) should be loaded to execute it.

```python
#!/usr/bin/env python3

import sys
import re

for line in sys.stdin:
    line = line.strip().lower()
    line = re.sub('[^A-Za-z\s]', '', line)
    for word in line.split():
        print("%s\t%s" % (word, 1))
```

The easiest way to create a **.py** file and populate it with the code above is to use the **nano** editor. Run the following code in the home directory:

```shell
$ nano mapper.py
```

Copy and paste the Python code into the opened file, and format it to make sure code lines are properly indented.

After you finish editing, hit <kbd>Ctrl</kbd>+<kbd>X</kbd> to quit the **nano** editor.  Enter **y** to save the change.

The Python code for the reducer can be specified as follows:

```python
#!/usr/bin/env python3

import sys

current_word, current_count = None, 0

for line in sys.stdin:
    word, count = line.strip().split('\t', 1)
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

Then quit **nano**. Remember to enter **y** to save the change.

Now, we've created the **mapper.py** and **reducer.py** scripts. Next, grant them execution permission by running the following command:

```shell
$ chmod +x mapper.py reducer.py
```



Be careful, as pasting the code directly into the nano editor may mess up the indentation. 



Hadoop streaming may give non-informative exceptions when mappers’ or reducers’ scripts work incorrectly. For example[^2]

[^2]: Kkeep in mind that if the error "PipeMapRed.waitOutputThreads()" appears after "map 100% reduce 0%", the actual error is inside a reducer; if the error appears after "map 0% reduce 0%", the actual error is inside a mapper.



```text
Error: java.lang.RuntimeException: PipeMapRed.waitOutputThreads(): subprocess failed with code 1
```



You can spend a lot of time by iterating on:

1. write code.
2. submit your application to a cluster and wait for the result
3. investigate logs, if unsuccessful go to step 1.

You can safe a lot of time, by debugging your applications locally. 

Therefore, it is recommended to test the two Python scripts locally before submitting them to the cluster.  



Our local testing simply uses bash and takes the form that conforms to typical UNIX-style piping:

```shell
$ echo "foo FOO2 quux. lab foo Ba1r Quux" | ~/mapper.py | sort -k1,1 | ~/reducer.py
```

This emulates the same pipeline (in a non-distributed manner) that Hadoop will perform when streaming in a distributed way.

If exceptions are raised,  open the two **.py** files to examine whether all lines of the Python code are correctly indented.

If you observe the following output, it means the code works properly:

```
bar     1
foo     3
lab     1
quux    2
```



Now everything is ready. You can run the Python MapReduce job on your EMR cluster by typing:



```shell
$ mapred streaming -D mapreduce.job.reduces=2 \
> -input input_dir_on_HDFS \
> -output output_dir_on_HDFS \
> -mapper mapper.py \
> -reducer reducer.py \
> -file mapper.py -file reducer.py
```



The generic option`mapred.reduce.tasks` is used to specify the number of reducers.

The executables do not pre-exist on the machines in the cluster. So we use the `-file` option to tell the framework to pack the executable files (**mapper.py** and **reducer.py**) as a part of job submission.  

We can also use the `-files` option to specify a comma-separated list of files (there should be no space between any adjacent files). These files then get copied to the working directory on all nodes that run the tasks . And because `-files` is a generic option (see `mapred streaming -help`), it comes before those command options.

```shell
$ mapred streaming -D mapreduce.job.reduces=2 \
> -files mapper.py,reducer.py \
> -input input_dir_on_HDFS \
> -output output_dir_on_HDFS \
> -mapper mapper.py \
> -reducer reducer.py \
```



the output is like

```text
a       9386
ab      1
abandon 10
abandoning      1
abandonment     4
abasements      1
...
```







## Streaming  & Generic Command Options 



Streaming supports both [streaming command options](https://hadoop.apache.org/docs/current/hadoop-streaming/HadoopStreaming.html#Streaming_Command_Options) and [generic command options](https://hadoop.apache.org/docs/current/hadoop-streaming/HadoopStreaming.html#Generic_Command_Options). 

The general command line syntax is shown below.

```text
mapred streaming [genericOptions] [streamingOptions]
```

Be sure to place the generic options before the streaming options, otherwise the command will fail. 



To set a stage for the use of these options, let's consider a problem that follows up the previous word count example: how can we get the most frequent words using MapReduce? 

We know Hadoop will sort the intermediate results by the key. Let's leverage this property and design our mapper as (`swap.py`):

```python
#!/usr/bin/env python3

import sys

for line in sys.stdin:
    word, count = line.split()
    print('%07d\t%s' % (int(count), word))
```



By default, shuffling stage performs a string sorting, not integer sorting. We played a trick, `%07d`, to pad leading zeros, so that string sorting and integer sorting are equivalent in this case.

We don't need a reducer in this case. To create a map-only Jobs, simply set `mapreduce.job.reduces` to zero

Again, you can use a Linix equivalent to test the Python script locally: 

```bash
$ cat combined | ~/swap.py | sort -k1,1 | tail -n20
```

Run with Hadoop Streaming:

```shell
$ mapred streaming -D mapreduce.job.reduces=0 \
> -files swap.py \
> -input input_dir_on_HDFS \
> -output output_dir_on_HDFS \
> -mapper swap.py \
```





There are many options we can specify in the command line argument. Try the following command:



```shell
$ cat combined | sort -t -k2,2nr | head -n20
```



```shell
$ mapred streaming \
> -D stream.num.map.output.key.fields=2 \           # each line has a single \t; so the value will be an empty Text object
> -D mapreduce.job.output.key.comparator.class=org.apache.hadoop.mapreduce.lib.partition.KeyFieldBasedComparator \
> -D mapreduce.partition.keycomparator.options=-k2,2nr \
> -mapper /bin/cat -input input_dir_on_HDFS -output output_dir_on_HDFS
```

or

```bash
$ mapred streaming \
> -D stream.num.map.output.key.fields=2 \
> -D mapred.output.key.comparator.class=org.apache.hadoop.mapred.lib.KeyFieldBasedComparator \
> -D mapred.text.key.comparator.options=-k2,2nr \
> -mapper /bin/cat -reducer cat -input input_dir_on_HDFS -output output_dir_on_HDFS
```





## Useful Command Options (both streaming and generic)

- `-D <property>=<value>`: Specify additional configuration variables. See [details](https://hadoop.apache.org/docs/current/hadoop-streaming/HadoopStreaming.html#Specifying_Directories).

- `-D stream.map.output.field.separator=<sep>`: Specify a field separator (other than the default one,  `\t`) to be used for separating the key from the value when the MapReduce framework reads a line from the stdout of the streaming mapper.

- `-D stream.num.map.output.key.fields=<num>`: Specify the key to include the prefix up to the `<num>`-th field separator in a line (the rest of the line will be the value). If the separator has less occurrences than specified, then the whole line will be the key and the value will be an empty Text object (like the one created by `new Text("")`).

- `-D map.output.key.field.separator=<sep>` or `-D mapreduce.map.output.key.field.separator=<sep>`: Specify the field separator to further divide the key into fields logically (no actual breakup) so as to condition partitioning and sorting on different fields. It has nothing to do with key-value separation and removal of separators. All resulting fields are still concatenated with the separator.

- `-partitioner <Java class>`: Partitionality is only possible to do with Java. You can write your own Java class to do partitioning. But there is already a collection of library classes that you can use to tune your stream in MapReduce application. 

  For example, use the library class, [`org.apache.hadoop.mapred.lib.KeyFieldBasedPartitioner`](https://hadoop.apache.org/docs/current/api/org/apache/hadoop/mapred/lib/KeyFieldBasedPartitioner.html), as the Partitioner, allowing the MapReduce framework to partition the map outputs based on certain key fields rathan than the whole keys. From my personal experience, knowing how to work with this one will be enough for the large amount of streaming applications that you will write.

- `-D mapred.text.key.partitioner.options=-k<pos1.c>[,<pos2.c>]` or `-D mapreduce.partition.keypartitioner.options=-k<pos1.c>[,<pos2.c>]`: When used with `-partitioner org.apache.hadoop.mapred.lib.KeyFieldBasedPartitioner`, specify the key fields between `<pos1>` and `<pos2>` used for partitioning (both inclusive; if `pos2` is omitted, it defaults to the end of line).  `<.c>` is the number of the 1st character from the beginning of the corresponding field.

- `-D mapreduce.job.output.key.comparator.class=<Java class>` or `mapred.output.key.comparator.class=<Java class>`: Use the library class, **KeyFieldBasedComparator**, as the comparator, allowing the MapReduce framework to compare the map outputs based on certain key fields, not the whole keys. Can be set to either [`org.apache.hadoop.mapred.lib.KeyFieldBasedComparator`](https://hadoop.apache.org/docs/current/api/org/apache/hadoop/mapred/lib/KeyFieldBasedComparator.html) or [`org.apache.hadoop.mapreduce.lib.partition.KeyFieldBasedComparator`](https://hadoop.apache.org/docs/current/api/org/apache/hadoop/mapreduce/lib/partition/KeyFieldBasedComparator.html).

- `-D mapred.text.key.comparator.options=-k<pos1.c>[,<pos2.c>]` or `-D mapreduce.partition.keycomparator.options=-k<pos1.c>,<pos2.c>`: Specify the comparator rule. The `-n` option can be used to specify that the sorting is numerical sorting and the `-r` option can be used to specify that the result should be reversed.

- `-D mapred.map.tasks`: A hint to the number of mappers. If not work, you may want to `change mapred.min.split.size` in **mapred-site.xml**.

- Similarly, you can set `-D stream.reduce.output.field.separator=<sep>` , ``-D stream.map.input.field.separator=<sep>`, and `stream.reduce.input.field.separator=<sep> `. For example, if you want to generate an output like the following for the word count example:

  ```text
  a,9386
  ab,1
  abandon,10
  abandoning,1
  abandonment,4
  ...
  ```

  the scripts can be changed to (actually only the print step of the reducer script matters):

  ```python
  #!/usr/bin/env python3
  # mapper.py
  
  import sys
  import re
  
  for line in sys.stdin:
      line = line.strip().lower()
      line = re.sub('[^A-Za-z\s]', '', line)
      for word in line.split():
          print("%s,%s" % (word, 1))      # set -D stream.map.output.field.separator=, for correct partitioning
  ```

  ```python
  #!/usr/bin/env python3
  # reducer.py
  
  import sys
  
  current_word, current_count = None, 0
  
  for line in sys.stdin:
      line = line.strip()
      word, count = line.split(',', 1)    # set -D stream.reduce.input.field.separator=,
      count = int(count)
      if current_word == word:
          current_count += count
      else:
          if current_word:
              print("%s,%s" % (current_word, current_count))
          current_count = count
          current_word = word
  
  if word == current_word: print("%s,%s" % (current_word, current_count))
  ```

  And we need to add `-D stream.map.output.field.separator=, -D stream.reduce.input.field.separator=,` to the streaming command.

  But don't add `-D stream.reduce.output.field.separator=,`, as the reducer will identify the prefix up to the 1st  `,` as the key and the rest the value and concatenate them with `\t` before emitting the output key/value pair. So you will end up with tab-delimited lines, again:

  ```text
  a       9386
  ab      1
  abandon 10
  abandoning      1
  abandonment     4
  abasements      1
  ...
  ```

  Actually, we can keep `word, count = line.split('\t', 1)` even if the print step of the mapper script is `print("%s,%s" % (word, 1))`, and run the streaming command only with `-D stream.map.output.field.separator=,`.




- `-D stream.num.reduce.output.fields=<num>`: Specify the `<num>`-th field separator in a line of the reduce outputs as the separator between the key and the value.
- `-combiner=<executable> or <Java class>`: Specify Combiner executable for map output







use of custom partitioner



https://stackoverflow.com/questions/13191468/how-to-specify-the-partitioner-for-hadoop-streaming

---



During the shuffle stage, the result of the map stage is written to disk, sorted and then transmitted over the network to the machines where the reduce stage runs. As a result, if the size of the output of the map stage is large, the shuffle stage can be computationally difficult which can cause a significant delay. Such behaviour is not reasonable for tasks where the map and reduce functions are simple and do not incur long latency. For example, in the word counting task, map and reduce functions are computationally simple while mappers emit one key-value pair per word in a dataset which can be very large. One way to simplify the shuffle stage is to reduce the size of the output of the map stage by *combining*. Combining means “pre-reducing” the results of multiple mappers which run on the same machine. If a machine generates less output during the map stage, then less data goes to the shuffle stage. As a result, less data is written to disk, sorted and transferred over the network which can accelerate a MapReduce job. Enabling combining does not require a change in reducers. Indeed, with combining, the reducers will just receive partially reduced results which does not affect correctness.

To achieve this with Hadoop Streaming, one can define a **combiner** – a program that “pre-reduces” a part of mapper’s output generated by one machine. The combiner is very similar to the reducer – the combiner accepts the output of a few mappers and outputs a partially reduced result. For the word counting task, the combiner can be identical to the reducer. An important difference of a combiner from a reducer is that not all values corresponding to one key are sent to the same combiner. Note that Hadoop Streaming does not guarantee that the combiner will be executed at all for the output of the mapper. Therefore, the combining function is not always applicable (for example, in the case of calculating a median value by a key). Nevertheless, in those problems where the combining function is applicable, its use allows achieving a significant increase in the speed of execution of the MapReduce task.

For the word-counting task, we could simply reuse the reducer logic from above for making partial aggregation. The memory-efficient version of reducer provided below also can be used as a combiner.





### Distributed Cache and Map-side Joins



#### Telecommunication data:

https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/EGZHFV

This dataset provides information about the telecommunication activity over the city of Milano. The dataset is the result of a computation over the Call Detail Records (CDRs) generated by the Telecom Italia cellular network over the city of Milano. CDRs log the user activity for billing purposes and network management. There are many types of CDRs, for the generation of this dataset we considered those related to the following activities:

- Square id: The id of the square that is part of the city GRID.

- Time interval: The beginning of the time interval expressed as the number of millisecond elapsed from the Unix Epoch on January 1st, 1970 at UTC. The end of the time interval can be obtained by adding 600000 milliseconds (10 minutes) to this value.

- Country code: The phone country code of a nation. Depending on the measured activity this value assumes different meanings that are explained later.

- SMS-in activity: The activity in terms of received SMS inside the Square id, during the Time interval and sent from the nation identified by the Country code.

- SMS-out activity: The activity in terms of sent SMS inside the Square id, during the Time interval and received by the nation identified by the Country code.

- Call-in activity: The activity in terms of received calls inside the Square id, during the Time interval and issued from the nation identified by the Country code.

- Call-out activity: The activity in terms of issued calls inside the Square id, during the Time interval and received by the nation identified by the Country code.

- Internet traffic activity: The activity in terms of performed internet traffic inside the Square id, during the Time interval and by the nation of the users performing the connection identified by the Country code.



#### Spatial data:



https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/QJWLFU



![](https://raw.githubusercontent.com/justinjiajia/img/master/bigdata/milan_grid_map.jpg)



suppose it is stored as **/data/milano-grid.geojson** in HDFS

Square ID is an identificator of a polygon under the map of Milan. 



We have a big dataset of telecommunication data and a small dataset of spacial data.



Suppose that we're going to validate the following hypothesis:

>  Is it true that people who live in at the north of the city  make twice more calls than people who live in the south?



To validate the hypothesis, we need to join the telecommunication data with the spacial data:





```python
#!/usr/bin/env python3

# the script for the streaming mapper

import sys

def download_grid(hdfs_path):
    child_process = subprocess.Popen(["hdfs", "dfs", "-cat", hdfs_path], stdout=subprocess.PIPE)
    out, err = child_process.communicate()
    geojson = json.loads(out)
    return geojson


geojson = download_grid("/data/milano-grid.geojson")      # read spatial data stored in HDFS directly from the streaming script
# load_grid contains an assignment: grid[square_id] = "North" if (min_y + max_y) / 2 > middle_point else "South"
grid = load_grid(geojson)    

for line in sys.stdin:
    square_id, aggregate = line.split("\t", 1)
    square_id = int(square_id)
    time_interval, country, sms_in, sms_out, call_in, call_out, internet = aggregate.sp1it("\t")
    if sms_in:
        sms_in = float(sms_in)
        print(grid[square_id], sms_in, sep="\t")
```



```bash
$ yarn jar $HADOOP_STREAMING_JAR -D mapreduce.job.reduces=0 \
> -files read_from_hdfs_mapper.py \
> -mapper 'python read_From_hdfs_mapper.py' \
> -numReduceTasks 0 \
> -input input_dir_on_HDFS \ \
> -output output_dir_on_HDFS 
```



the spatial data will be transferred over the network to each mapper. 

But if we use a distributed cache, this data will be replicated to each node once, and will be available locally for each mapper. 

Therefore, we'll dramatically reduce the and increase the level of data locality. 



```python
#!/usr/bin/env python3

# the script for the streaming mapper

import sys
import json


geojson = json.load(open("milano-grid.geojson"))           # read spatial data from the local file system 
# load_grid contains an assignment: grid[square_id] = "North" if (min_y + max_y) / 2 > middle_point else "South"
grid = load_grid(geojson)    

for line in sys.stdin:
    square_id, aggregate = line.split("\t", 1)
    square_id = int(square_id)
    time_interval, country, sms_in, sms_out, call_in, call_out, internet = aggregate.sp1it("\t")
    if sms_in:
        sms_in = float(sms_in)
        print(grid[square_id], sms_in, sep="\t")
```

 

 

Please bear in mind the API of parsing an HDFS file to a distributed cache. Prefix the path with `hdfs`. 

```bash
$ yarn jar $HADOOP_STREAMING_JAR -D mapreduce.job.reduces=0 \
> -files read_from_hdfs_mapper.py,hdfs:///data/milano-grid.geojson \   
> -mapper 'python read_From_hdfs_mapper.py' \
> -input /data/telecommunication \ 
> -output output_dir_on_HDFS 
```



### Reduce-side Joins



Imagine that your telecommunication company has grown, and you have to aggregate that over thousands of cities.  So your spacial data is more granular and not small anymore. 



You need to find a way to join several big datasets. Then we should use a Reduce-side join.

![](https://raw.githubusercontent.com/justinjiajia/img/master/bigdata/reduce-side_joins.PNG)



```python
#!/usr/bin/env python3

# the script for the streaming mapper

import sys
import json

if "geojson" in os.environ["mapreduce_map_input_file"]:
    geojson = json.load(sys.stdin)
    grid = load_grid(geojson)
    for grid_id, ce11_type in grid.items():
        print(grid_id, "grid", ce11_type, sep="\t")

else:
    for line in sys.stdin:
        square_id, aggregate = line.split("\t", 1)
        square_id = int(square_id)
        time_interval, country, sms_in, sms_out, call_in, call_out, internet = aggregate.sp1it("\t")
        
        if sms_in:
            sms_in = float(sms_in)
            print(square_id, "logs", sms_in, sep="\t")
                                                                                        
```

```python
#!/usr/bin/env python3

# the script for the streaming reducer
# shuffle and sort ensures that, for each grid id, the record with tag grid ome before those with tag logs 

import sys


current_grid = None
grid_load = 0
grid_location = None

for line in sys.stdin:
    grid_id, label, value = line.strip("\n").split("\t", 2)
    if label == "grid":
        if current_grid: print(current_grid, grid_location, grid_load, sep="\t")
        current_grid = grid_id
        grid_load = 0
        grid_location = value
    
    else:
        counts = float(value)
        grid_load += counts

if current_grid != "grid":
    print(current_grid, grid_location, grid_load, sep="\t")
```



```bash
yarn jar $HADOOP_STREAMING_JAR -D mapreduce.job.reduces=5 \
> -D stream.num.map.output.key.fields=2 \                              # use secondary sort
> -D mapreduce.partition.keypartitioner.options="-k1,1" \
> -files reduce_side_mapper.py,reduce_side_reducer.py \
> -mapper 'python reduce_side_mapper.py' > -reducer 'python reduce_side_reducer.py' \
> -input /data/telecommunication,/data/geojson \
> -output output_dir_on_HDFS \
> -partitioner org.apache.hadoop.mapred.lib.KeyFieldBasedPartitioner
 
```



### Hadoop Aggregate Package



Hadoop has a library package called [Aggregate](https://hadoop.apache.org/docs/current/api/org/apache/hadoop/mapred/lib/aggregate/package-summary.html). Aggregate provides a special reducer class and a special combiner class, and a list of simple aggregators that perform aggregations such as “sum”, “max”, “min” and so on over a sequence of values. Aggregate allows you to define a mapper plugin class that is expected to generate “aggregatable items” for each input key/value pair of the mappers. The combiner/reducer will aggregate those aggregatable items by invoking the appropriate aggregators.



To use Aggregate, simply specify `-reducer aggregate`:

```bash

$ mapred streaming 
> -input input_dir_on_HDFS -output output_dir_on_HDFS \
> -mapper aggregate_mapper.py \
> -reducer aggregate \
> -file aggregate_mapper.py
```

 

In the mapper output, you need to prefix each key by the type of values such as long,  double, or string, and also prefix keys by action. 

For example, sum, mean, max, or unique. 

Correct and complete examples are double the value sum and stream value mean. 

```python
#!/usr/bin/env python3

# the script for the streaming mapper

import sys

for line in sys.stdin:
    key, value = line.strip('\n').split('\t', 1)
    print("DoubleValueSum:{}".format(key), value, sep='\t')
```









## References:



https://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/#reduce-step-reducerpy

http://www.inf.ed.ac.uk/teaching/courses/exc/labs/hadoop_streaming.html

https://xuhappy.github.io/courses/BigData/tutorial/tutorial3/

https://www.programmersought.com/article/1872542763/
