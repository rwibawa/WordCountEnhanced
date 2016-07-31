[https://hadoop.apache.org/docs/r2.7.2/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html]

$ javac -classpath $(hadoop classpath) -d . WordCount2.java
$ jar cf wc3.jar com

$ hdfs dfs -rm -r -f /user/ryan/wordcount/output/.
$ hadoop jar wc3.jar com.navisow.WordCountEnhanced.WordCount2 \
-Dwordcount.case.sensitive=true -Dwordcount.skip.patterns=false \
/user/ryan/wordcount/input /user/ryan/wordcount/output

$ hdfs dfs -cat /user/ryan/wordcount/output/part-r-00000
16/07/31 09:58:51 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Bye     2
Goodbye 2
Hadoop  2
Hadoop, 1
Hello   4
World   2
World!  1
World,  1
hadoop. 1
to      1

$ hdfs dfs -ls -R /user
16/07/31 10:05:20 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
drwxr-xr-x   - ryan supergroup          0 2016-07-30 22:11 /user/ryan
drwxr-xr-x   - ryan supergroup          0 2016-07-31 09:58 /user/ryan/wordcount
drwxr-xr-x   - ryan supergroup          0 2016-07-31 09:50 /user/ryan/wordcount/input
-rw-r--r--   1 ryan supergroup         22 2016-07-30 22:12 /user/ryan/wordcount/input/file01
-rw-r--r--   1 ryan supergroup         28 2016-07-30 22:12 /user/ryan/wordcount/input/file02
-rw-r--r--   1 ryan supergroup         24 2016-07-31 09:50 /user/ryan/wordcount/input/file03
-rw-r--r--   1 ryan supergroup         33 2016-07-31 09:50 /user/ryan/wordcount/input/file04
drwxr-xr-x   - ryan supergroup          0 2016-07-31 09:58 /user/ryan/wordcount/output
-rw-r--r--   1 ryan supergroup          0 2016-07-31 09:58 /user/ryan/wordcount/output/_SUCCESS
-rw-r--r--   1 ryan supergroup         84 2016-07-31 09:58 /user/ryan/wordcount/output/part-r-00000
-rw-r--r--   1 ryan supergroup         13 2016-07-31 09:38 /user/ryan/wordcount/patterns.txt

$ export HADOOP_CONF_DIR=/usr/local/Cellar/hadoop/2.7.2/libexec/etc/hadoop/
$ spark-submit --class com.navisow.WordCountEnhanced.WordCount2 \
--master yarn --deploy-mode client --executor-memory 2G --num-executors 10 \
wc3.jar \
-Dwordcount.case.sensitive=true -Dwordcount.skip.patterns=false \
/user/ryan/wordcount/input /user/ryan/wordcount/output


$ hadoop jar wc3.jar com.navisow.WordCountEnhanced.WordCount2 -Dwordcount.case.sensitive=true -Dwordcount.skip.patterns=true /user/ryan/wordcount/input /user/ryan/wordcount/output -skip wordcount/patterns.txt

$ hdfs dfs -cat /user/ryan/wordcount/output/part-r-00000
Bye     2
Goodbye 2
Hadoop  3
Hello   4
World   4
hadoop  1

$ spark-submit --class com.navisow.WordCountEnhanced.WordCount2 \
--master yarn --deploy-mode client --executor-memory 2G --num-executors 10 \
wc3.jar \
-Dwordcount.case.sensitive=false -Dwordcount.skip.patterns=true \
/user/ryan/wordcount/input /user/ryan/wordcount/output \
-skip wordcount/patterns.txt

$ hdfs dfs -cat /user/ryan/wordcount/output/part-r-00000
bye     2
goodbye 2
hadoop  4
hello   4
world   4

$ hadoop classpath
/usr/local/Cellar/hadoop/2.7.2/libexec/etc/hadoop/:
/usr/local/Cellar/hadoop/2.7.2/libexec/share/hadoop/common/lib/*:
/usr/local/Cellar/hadoop/2.7.2/libexec/share/hadoop/common/*:
/usr/local/Cellar/hadoop/2.7.2/libexec/share/hadoop/hdfs:
/usr/local/Cellar/hadoop/2.7.2/libexec/share/hadoop/hdfs/lib/*:
/usr/local/Cellar/hadoop/2.7.2/libexec/share/hadoop/hdfs/*:
/usr/local/Cellar/hadoop/2.7.2/libexec/share/hadoop/yarn/lib/*:
/usr/local/Cellar/hadoop/2.7.2/libexec/share/hadoop/yarn/*:
/usr/local/Cellar/hadoop/2.7.2/libexec/share/hadoop/mapreduce/lib/*:
/usr/local/Cellar/hadoop/2.7.2/libexec/share/hadoop/mapreduce/*:
/contrib/capacity-scheduler/*.jar

export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib"
