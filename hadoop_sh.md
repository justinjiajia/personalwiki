

**start-dfs.sh** https://github.com/apache/hadoop/blob/trunk/hadoop-hdfs-project/hadoop-hdfs/src/main/bin/start-dfs.sh


```bash


# let's locate libexec...

# test if the length of $HADOOP_HOME is greater than zero or not



if [[ -n "${HADOOP_HOME}" ]]; then

  # if so, set HADOOP_DEFAULT_LIBEXEC_DIR to "${HADOOP_HOME}/libexec"
  # HADOOP_HOME is set in .bashrc

  HADOOP_DEFAULT_LIBEXEC_DIR="${HADOOP_HOME}/libexec"
else
  HADOOP_DEFAULT_LIBEXEC_DIR="${bin}/../libexec"
fi

# set HADOOP_LIBEXEC_DIR to $HADOOP_DEFAULT_LIBEXEC_DIR if HADOOP_LIBEXEC_DIR is unset

HADOOP_LIBEXEC_DIR="${HADOOP_LIBEXEC_DIR:-$HADOOP_DEFAULT_LIBEXEC_DIR}"
# shellcheck disable=SC2034
HADOOP_NEW_CONFIG=true

# test if ${HADOOP_LIBEXEC_DIR}/hdfs-config.sh exists and is a regular file (e.g., not a device file or a directory)

if [[ -f "${HADOOP_LIBEXEC_DIR}/hdfs-config.sh" ]]; then

  # then run the shell script
  # One line in it is HADOOP_HDFS_HOME="${HADOOP_HDFS_HOME:-$HADOOP_HOME}"

  . "${HADOOP_LIBEXEC_DIR}/hdfs-config.sh"
else
  echo "ERROR: Cannot execute ${HADOOP_LIBEXEC_DIR}/hdfs-config.sh." 2>&1
  exit 1
fi






#---------------------------------------------------------
# secondary namenodes (if any)

# redirect the stderr to /dev/null

SECONDARY_NAMENODES=$("${HADOOP_HDFS_HOME}/bin/hdfs" getconf -secondarynamenodes 2>/dev/null)

# if the above command did not produce an error

if [[ -n "${SECONDARY_NAMENODES}" ]]; then

  if [[ "${NAMENODES}" =~ , ]]; then

    hadoop_error "WARNING: Highly available NameNode is configured."
    hadoop_error "WARNING: Skipping SecondaryNameNode."

  else

    if [[ "${SECONDARY_NAMENODES}" == "0.0.0.0" ]]; then

      # set SECONDARY_NAMENODES to the output of the command hostname
      SECONDARY_NAMENODES=$(hostname)
    fi

    echo "Starting secondary namenodes [${SECONDARY_NAMENODES}]"

    hadoop_uservar_su hdfs secondarynamenode "${HADOOP_HDFS_HOME}/bin/hdfs" \
      --workers \
      --config "${HADOOP_CONF_DIR}" \
      --hostnames "${SECONDARY_NAMENODES}" \
      --daemon start \
      secondarynamenode
    (( HADOOP_JUMBO_RETCOUNTER=HADOOP_JUMBO_RETCOUNTER + $? ))
  fi
fi

#---------------------------------------------------------

```

in **hadoop/bin/hdfs** https://github.com/apache/hadoop/blob/trunk/hadoop-hdfs-project/hadoop-hdfs/src/main/bin/hdfs

```
getconf)
  HADOOP_CLASSNAME=org.apache.hadoop.hdfs.tools.GetConf
```  

the java source code for this class

https://github.com/apache/hadoop/blob/trunk/hadoop-hdfs-project/hadoop-hdfs/src/main/java/org/apache/hadoop/hdfs/tools/GetConf.java


```Java

/**
 * Handler for {@link Command#SECONDARY}
 */
static class SecondaryNameNodesCommandHandler extends CommandHandler {
  @Override
  public int doWorkInternal(GetConf tool, String []args) throws IOException {
    tool.printMap(DFSUtil.getSecondaryNameNodeAddresses(tool.getConf()));
    return 0;
  }
}
```

in https://github.com/apache/hadoop/blob/trunk/hadoop-hdfs-project/hadoop-hdfs/src/main/java/org/apache/hadoop/hdfs/DFSUtil.java

```Java

...
import static org.apache.hadoop.hdfs.DFSConfigKeys.DFS_NAMENODE_SECONDARY_HTTP_ADDRESS_KEY;
...


/**
 * Returns list of InetSocketAddresses of corresponding to secondary namenode
 * http addresses from the configuration.
 *
 * @param conf configuration
 * @return list of InetSocketAddresses
 * @throws IOException on error
 */
public static Map<String, Map<String, InetSocketAddress>> getSecondaryNameNodeAddresses(
    Configuration conf) throws IOException {
  Map<String, Map<String, InetSocketAddress>> addressList = DFSUtilClient.getAddresses(
      conf, null, DFS_NAMENODE_SECONDARY_HTTP_ADDRESS_KEY);
  if (addressList.isEmpty()) {
    throw new IOException("Incorrect configuration: secondary namenode address "
        + DFS_NAMENODE_SECONDARY_HTTP_ADDRESS_KEY + " is not configured.");
  }
  return addressList;
}
```

in https://github.com/apache/hadoop/blob/trunk/hadoop-hdfs-project/hadoop-hdfs/src/main/java/org/apache/hadoop/hdfs/DFSConfigKeys.java

```Java
...
import org.apache.hadoop.hdfs.client.HdfsClientConfigKeys;
...
public static final String  DFS_NAMENODE_SECONDARY_HTTP_ADDRESS_KEY =
    HdfsClientConfigKeys.DeprecatedKeys.DFS_NAMENODE_SECONDARY_HTTP_ADDRESS_KEY;
```

in https://github.com/apache/hadoop/blob/trunk/hadoop-hdfs-project/hadoop-hdfs-client/src/main/java/org/apache/hadoop/hdfs/client/HdfsClientConfigKeys.java

```Java
/**
 * These are deprecated config keys to client code.
 */
interface DeprecatedKeys {
...

  String DFS_NAMENODE_SECONDARY_HTTP_ADDRESS_KEY =
      "dfs.namenode.secondary.http-address";
...
}
```
