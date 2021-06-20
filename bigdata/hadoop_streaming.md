



# Hadoop Streaming





[Hadoop streaming](https://hadoop.apache.org/docs/current/hadoop-streaming/HadoopStreaming.html) is a utility that comes with the Hadoop distribution. It allows us to create and run Map/Reduce jobs with any executable or script as the mapper and/or the reducer. For example, the mapper and reducer can be just normal Linux executables:

```bash
$ mapred streaming \
> -input myInputDirs \
> -output myOutputDir \
> -mapper /bin/cat \
> -reducer /usr/bin/wc
```

- Mapper: takes input stream from [standard input](http://en.wikipedia.org/wiki/Standard_streams); emit key-value pairs to [standard output](http://en.wikipedia.org/wiki/Standard_streams). Each key-value pair takes one line and is formatted as `'%s\t%s' % (key, value)`.
- Reducer: takes input key-value pairs from STDIN; output key-value pairs to STDOUT.
- By default, the prefix of a line up to the first tab character  (`'\t'`) is the key and the rest of the line (excluding the tab character) will be the value. If there is no tab character in the line, then entire line is considered as key and the value is null. 



Starting from Ubuntu 20.04, Python 3 is included in the base system installation.


```bash
$ which python3
```

You can run the following code to check the exact Python version installed:

```shell
$ python3 --version
```

For the word count application, the Python code for the mapper can be specified as follows:

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


The easiest way to create a **.py** file and populate it with the code above is to use *nano*. Run the following code in the home directory:

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
> -D mapreduce.job.reduces=2 \
> -input input_dir_on_HDFS \
> -output output_dir_on_HDFS \
> -mapper mapper.py \
> -reducer reducer.py \
> -file mapper.py \
> -file reducer.py
```

-file is a command option

```shell
$ mapred streaming \
> -D mapreduce.job.reduces=2 \
> -files mapper.py,reducer.py \
> -input input_dir_on_HDFS \
> -output output_dir_on_HDFS \
> -mapper mapper.py \
> -reducer reducer.py \
```

We can also use the -files option to specify a comma-separated list of files (between any adjacent files, the name of the latter file must succeed that of the former without any space in between) to be copied to the cluster. These files will get copied onto the current working directory of mapper or reducer on all nodes.
And because `-files` is a generic option (see `mapred streaming -help`), it comes before those command options.



Following up the previous word count example, how can we get the most frequent words using MapReduce? 

We know Hadoop will sort the intermediate results by the key. Let's leverage this property and design our mapper as (`swap.py`):

```python
#!/usr/bin/env python3

import sys

for line in sys.stdin:
    word, count = line.split()
    print '%07d\t%s' % (int(count), word)
```



Notes: By default, shuffling stage performs a string sorting, not integer sorting. We played a trick, `%07d`, to pad leading zeros, so that string sorting and integer sorting are equivalent in this case.

We don't need a reducer in this case. Run with Hadoop Streaming:

```shell
$ mapred streaming \
> -files swap.py \
> -input input_dir_on_HDFS \
> -output output_dir_on_HDFS \
> -mapper swap.py \
```



### Option 2

There are many parameters you can specify in the command line argument. Try the following command:

```text
hduser@master:~$ hadoop jar hadoop-streaming-2.7.1.jar \
    -D stream.num.map.output.key.fields=2 \
    -D mapred.output.key.comparator.class=org.apache.hadoop.mapred.lib.KeyFieldBasedComparator \
    -D mapred.text.key.comparator.options=-k2,2nr \
    -mapper cat -reducer cat -input /result -output /result_sorted2 \
```

- `-D property=value`: Use value for given property
- `-D stream.num.map.output.key.fields`: Specify how many fields as the key
- `-D mapred.output.key.comparator.class`: Use the library class, KeyFieldBasedComparator, as the comparator, allowing the Map/Reduce framework to compare the map outputs based on certain key fields, not the whole keys.
- `-D mapred.text.key.comparator.options`: Specify the comparator rule -- `-k2,2` means sort the second field; `n` means in numerical order; `r` means reverse.

## Other useful streaming command options

- `-partitioner org.apache.hadoop.mapred.lib.KeyFieldBasedPartitioner`: Use the library class, KeyFieldBasedPartitioner, as the Partitioner, allowing the Map/Reduce framework to partition the map outputs based on certain key fields, not the whole keys. (What's the difference between partition and comparator?)
- `-D mapred.text.key.partitioner.options`: Specify the rule to partition the intermediate tuples.
- `-D stream.map.output.field.separator`: Specify your own separator between key and value.
- `-D mapred.reduce.tasks`: Specify the number of reducers.
- `-D mapred.map.tasks`: A hint to the number of mappers. If not work, you may want to change mapred.min.split.size in mapred-site.xml.



## References:



https://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/#reduce-step-reducerpy

http://www.inf.ed.ac.uk/teaching/courses/exc/labs/hadoop_streaming.html

https://xuhappy.github.io/courses/BigData/tutorial/tutorial3/
