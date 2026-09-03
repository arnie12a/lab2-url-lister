USER := $(shell whoami)

##
## Configure the Hadoop classpath for the GCP Dataproc environment
##

HADOOP_CLASSPATH := $(shell hadoop classpath)

UrlCount.jar: UrlCount.java
	javac -classpath $(HADOOP_CLASSPATH) -d ./ UrlCount.java
	jar cf UrlCount.jar UrlCount*.class
	rm -f UrlCount*.class

prepare:
	hdfs dfs -mkdir -p /user/$(USER)/input
	curl https://en.wikipedia.org/wiki/Apache_Hadoop > /tmp/input1.txt
	hdfs dfs -put -f /tmp/input1.txt /user/$(USER)/input/file01
	curl https://en.wikipedia.org/wiki/MapReduce > /tmp/input2.txt
	hdfs dfs -put -f /tmp/input2.txt /user/$(USER)/input/file02

filesystem:
	hdfs dfs -mkdir -p /user/$(USER)

run: UrlCount.jar
	hdfs dfs -rm -r /user/$(USER)/output || true
	hadoop jar UrlCount.jar UrlCount /user/$(USER)/input /user/$(USER)/output

