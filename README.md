# hadoop-docker
Docker container for quickstart with Hadoop.


# Ubuntu
The Ubuntu version uses the [official images](https://hub.docker.com/_/ubuntu) as a base.
Steps to [create a Docker container](https://docs.docker.com/get-started/part2/):
- create Dockerfile
- run `docker build` (e.g. `docker build --tag=ngeorge/ubuntu-hadoop-quickstart .`)
- push to docker hub `docker push ngeorge/ubuntu-hadoop-quickstart`
- run the container: `docker run -it ngeorge/ubuntu-hadoop-quickstart`


# Installing Hadoop
Generally this follows [these instructions](https://hadoop.apache.org/docs/r3.1.2/hadoop-project-dist/hadoop-common/SingleCluster.html).  Steps are:

- install `wget`, `ssh`, `pdsh`, and `git` (`apt install wget ssh pdsh git -y`)
- install Java 8 (`apt install apt install openjdk-8-jdk -y`)
- set the `JAVA_HOME` environment variable (`export JAVA_HOME=/usr`); the Java executable is installed at /usr/bin/java; Hadoop will look for it at JAVA_HOME/bin/java
- download and unzip Hadoop (`wget http://apache.claz.org/hadoop/common/hadoop-3.1.2/hadoop-3.1.2.tar.gz`, `tar -zxvf hadoop-3.1.2.tar.gz -C /opt`)
- add hadoop to our path so we can run it from anywhere: `export PATH=/opt/hadoop-3.1.2/bin:$PATH`

# Test running Hadoop within the container

Uses [mapreduce streaming](https://hadoop.apache.org/docs/r3.1.2/hadoop-streaming/HadoopStreaming.html) with Python files:

`docker run -it ngeorge/ubuntu-hadoop-quickstart`

`git clone https://github.com/Regis-University-Data-Science/simple_Hadoop_MapReduce_example.git`

`wget http://norvig.com/ngrams/shakespeare.txt`

Move text file to HDFS:
```bash
$ hdfs dfs -mkdir /shakespeare
$ hdfs dfs -mkdir /shakespeare/input
$ hdfs dfs -copyFromLocal shakespeare.txt /shakespeare/input
$ hdfs dfs -ls /shakespeare/input
```

Run the MapReduce Python code:
```bash
$ cd simple_Hadoop_MapReduce_example
$ mapred streaming \
    -mapper mapper.py \
    -reducer reducer.py \
    -input /shakespeare/input \
    -output /shakespeare/output
```

Check results:

`hdfs dfs -ls /shakespeare/output`

`hdfs dfs -cat /shakespeare/output/part-00000`

Copy to local disk:

`hdfs dfs -copyToLocal /shakespeare/output/part-00000 result`

View sorted word count:
`sort -gr -k 2 result | head`

To share data between the docker container and the host OS, use volumes:

`docker run -v path_in:container_path -it`
