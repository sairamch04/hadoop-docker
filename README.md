#Apache Hadoop 2.4.0 Docker image

Following on the success of our Hadoop 2.3 Docker image on https://registry.hub.docker.com/u/sequenceiq/hadoop-docker/ and aligning with the Hadoop release cycle, we have released a Hadoop 2.4 Docker image.


# Build the image

In case you'd like to try directly from the Dockerfile you can build the image as:

```
docker build  -t sequenceiq/hadoop-docker .
```

The image is also released as an official Docker image from Docker's automated build repository - you can always pull or refer the image when launching containers.

# Start a container

In order to use the Docker image you have just build or pulled use:

```
docker run -i -t sequenceiq/hadoop-docker /etc/bootstrap.sh -bash
```

## Testing

You can run one of the stock examples:

```
cd $HADOOP_PREFIX
# run the mapreduce
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.4.0.jar grep input output 'dfs[a-z.]+'

# check the output
bin/hdfs dfs -cat output/*
```

## Hadoop native libraries, build, Bintray, etc

The Hadoop build process is no easy task - requires lots of libraries and their right version, protobuf, etc and takes some time - we have simplified all these, made the build and released a 64b version of Hadoop nativelibs on this [Bintray repo](https://bintray.com/sequenceiq/sequenceiq-bin/hadoop-native-64bit/2.4.0/view/files). Enjoy. 


## Automate everything

As we have mentioned previousely, a Docker file was created and released in the official [docker repo](https://registry.hub.docker.com/u/sequenceiq/hadoop-docker/)


## Additional classpath

It is possible to add libraries to the classpath of a yarn container, by passing an environment variable as follows:

```
ACP_SRC=http://repo1.maven.org/maven2/org/apache/mahout
ACP=$ACP_SRC/mahout-core/0.9/mahout-core-0.9.jar,$ACP_SRC/mahout-math/0.9/mahout-math-0.9.jar,$ACP_SRC/mahout-integration/0.9/mahout-integration-0.9.jar
docker run -e "ACP=$ACP" -t -d -h sandbox --name sandbox  my-sandbox /etc/bootstrap.sh -bash
```
The ACP environment variable is a comma separated list of urls where the needed resources can be downloaded from.
Libraries are copied to the $HADOOP_PREFIX/share/hadoop/common/ folder.
