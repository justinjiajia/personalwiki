



# Hadoop Streaming



[Hadoop streaming](https://hadoop.apache.org/docs/current/hadoop-streaming/HadoopStreaming.html) is a utility that comes with the Hadoop distribution. It allows us to create and run Map/Reduce jobs with any executable or script as the mapper and/or the reducer. For example, both the mapper and the reducer can be just normal Linux executables:

```bash
$ mapred streaming \
> -input myInputDirs \
> -output myOutputDir \
> -mapper /bin/cat \
> -reducer /usr/bin/wc
```

- The [TextInputFormat](https://hadoop.apache.org/docs/current/api/org/apache/hadoop/mapreduce/lib/input/TextInputFormat.html) is used as the default input format class. Since the TextInputFormat returns keys of LongWritable class, which are actually not part of the input data, the keys will be discarded; only the values will be piped to the streaming mapper.

- The streaming mapper takes input stream from [stdin](http://en.wikipedia.org/wiki/Standard_streams) and emit key-value pairs to [stdout](http://en.wikipedia.org/wiki/Standard_streams). 

- When the Map/Reduce framework reads a line from the stdout of the streaming mapper,  it splits the line into a key/value pair. By default, the prefix of a line up to the first tab character  (`'\t'`) is the key and the rest of the line (excluding the tab character) will be the value. If there is no tab character in the line, then entire line is considered as key and the value is null. 

- The streaming reducer takes input key-value pairs from stdin; output key-value pairs to stdout.

  



Starting from Ubuntu 20.04, Python 3 is included in the base system installation.


```bash
$ which python3
```

You can run the following code to check the exact Python version installed:

```shell
$ python3 --version
```

For the word count application, the Python code for the mapper can be specified as follows[^1]:

[^1]: `#!/usr/bin/env python3` must be included as the first line in the Python scripts, as it instructs the program loader of a Unix-like operating system which program should be loaded to run them.

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

Then quit **nano**. Remember to enter **y** to save the change.

Now, we've created the **mapper.py** and **reducer.py** scripts. Next, grant them execution permission by running the following command:

```shell
$ chmod +x mapper.py reducer.py
```

Pasting the code directly into the nano editor may mess up the indentation. Therefore, it is recommended to test the two Python scripts locally before submitting them to the cluster.  

Our local testing takes the form that conforms to typical UNIX-style piping:

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
$ mapred streaming \
> -D mapreduce.job.reduces=2 \
> -input input_dir_on_HDFS \
> -output output_dir_on_HDFS \
> -mapper mapper.py \
> -reducer reducer.py \
> -file mapper.py \
> -file reducer.py
```



The generic option`mapred.reduce.tasks` is used to specify the number of reducers.

The executables do not pre-exist on the machines in the cluster. So we use the `-file` option to tell the framework to pack the executable files (**mapper.py** and **reducer.py**) as a part of job submission.  

We can also use the `-files` option to specify a comma-separated list of files (there should be no space between any adjacent files). These files are then made available on all nodes that run the tasks . And because `-files` is a generic option (see `mapred streaming -help`), it comes before those command options.

```shell
$ mapred streaming \
> -D mapreduce.job.reduces=2 \
> -files mapper.py,reducer.py \
> -input input_dir_on_HDFS \
> -output output_dir_on_HDFS \
> -mapper mapper.py \
> -reducer reducer.py \
```







## Streaming  & Generic Command Options 



Streaming supports both [streaming command]() options and [generic command options](https://hadoop.apache.org/docs/current/hadoop-streaming/HadoopStreaming.html#Generic_Command_Options). 

The general command line syntax is shown below.

```text
mapred streaming [genericOptions] [streamingOptions]
```

Be sure to place the generic options before the streaming options, otherwise the command will fail. 





Following up the previous word count example, how can we get the most frequent words using MapReduce? 

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
$ mapred streaming \
> -D mapreduce.job.reduces=0 \
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
> -D stream.num.map.output.key.fields=2 
> -D mapreduce.job.output.key.comparator.class=org.apache.hadoop.mapreduce.lib.partition.KeyFieldBasedComparator 
> -D mapreduce.partition.keycomparator.options=-k2,2nr 
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





## Other useful command options (both streaming and generic)

- `-D <property>=<value>`: Specify additional configuration variables. See [details](https://hadoop.apache.org/docs/current/hadoop-streaming/HadoopStreaming.html#Specifying_Directories).

- `-D stream.map.output.field.separator=<sep>`: Specify a field separator other than the tab character, `\t` (the default); used for separating the key from the value.

- `-D stream.num.map.output.key.fields=<num>`: Specify the key to include the prefix up to the `<num>`-th field separator in a line (the rest of the line will be the value). If the separator has less occurrences than specified, then the whole line will be the key and the value will be an empty Text object (like the one created by `new Text("")`).

-  `-D map.output.key.field.separator=<sep>` or `-D mapreduce.map.output.key.field.separator=<sep>`: Specify the field separator to further divide the key into fields for partitioning.

- `-partitioner <Java class>`: Use the library class, [`org.apache.hadoop.mapred.lib.KeyFieldBasedPartitioner`](https://hadoop.apache.org/docs/current/api/org/apache/hadoop/mapred/lib/KeyFieldBasedPartitioner.html), as the Partitioner, allowing the MapReduce framework to partition the map outputs based on certain key fields rathan than the whole keys. 

-  `-D mapred.text.key.partitioner.options=-k<pos1.c>[,<pos2.c>]` or `-D mapreduce.partition.keypartitioner.options=-k<pos1.c>[,<pos2.c>]`: When used with `-partitioner org.apache.hadoop.mapred.lib.KeyFieldBasedPartitioner`, specify the key fields between `<pos1>` and `<pos2>` used for partitioning (both inclusive).  `<.c>` is the number of the 1st character from the beginning of the corresponding field.

- `-D mapreduce.job.output.key.comparator.class=<Java class>` or `mapred.output.key.comparator.class=<Java class>`: Use the library class, KeyFieldBasedComparator, as the comparator, allowing the Map/Reduce framework to compare the map outputs based on certain key fields, not the whole keys. Can be set to either [`org.apache.hadoop.mapred.lib.KeyFieldBasedComparator`](https://hadoop.apache.org/docs/current/api/org/apache/hadoop/mapred/lib/KeyFieldBasedComparator.html) or [`org.apache.hadoop.mapreduce.lib.partition.KeyFieldBasedComparator`](https://hadoop.apache.org/docs/current/api/org/apache/hadoop/mapreduce/lib/partition/KeyFieldBasedComparator.html).

- `-D mapred.text.key.comparator.options=-k<pos1.c>[,<pos2.c>]` or `-D mapreduce.partition.keycomparator.options=-k<pos1.c>,<pos2.c>`: Specify the comparator rule. The `-n` option can be used to specify that the sorting is numerical sorting and the `-r` option can be used to specify that the result should be reversed.

  

- `-D mapred.map.tasks`: A hint to the number of mappers. If not work, you may want to `change mapred.min.split.size` in **mapred-site.xml**.



---

- Similarly, you can use `-D stream.reduce.output.field.separator=SEP` and `-D stream.num.reduce.output.fields=NUM` to specify the NUM-th field separator in a line of the reduce outputs as the separator between the key and the value.
- Similarly, you can specify `stream.map.input.field.separator` and `stream.reduce.input.field.separator` as the input separator for Map/Reduce inputs (the default one is also `\t`).?? for streaming or 



---







## References:



https://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/#reduce-step-reducerpy

http://www.inf.ed.ac.uk/teaching/courses/exc/labs/hadoop_streaming.html

https://xuhappy.github.io/courses/BigData/tutorial/tutorial3/
