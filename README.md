# hdinsight-java-storm-wordcount
A basic example of a Java-based Apache Storm Topology that can be used with Storm on HDInsight.

See [Develop a Java topology for Storm on HDInsight](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-storm-develop-java-topology) for a walkthrough of the steps used to create this project.

##To run on your development environment

1. Fork/Clone the repository to your development environment.

2. Install Java JDK 7 or higher. This was tested with Oracle Java 7 and 8, but should work under things like OpenJDK as well.

3. Install [Maven](http://maven.apache.org/)

4. Assuming Java and Maven are in the path, and everything is configured fine for JAVA_HOME, use the following to build and run the topology on the development environment:

        mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology

	As it runs, the topology will display startup information. Then it begins to display lines similar to the following as sentences are emitted from the spout and processed by the bolts.

		15398 [Thread-16-split] INFO  backtype.storm.daemon.executor - Processing received message source: spout:10, stream: default, id: {}, [an apple a day keeps thedoctor away]]
		15398 [Thread-16-split] INFO  backtype.storm.daemon.task - Emitting: split default [an]
		15399 [Thread-10-count] INFO  backtype.storm.daemon.executor - Processing received message source: split:6, stream: default, id: {}, [an]
		15399 [Thread-16-split] INFO  backtype.storm.daemon.task - Emitting: split default [apple]
		15400 [Thread-8-count] INFO  backtype.storm.daemon.executor - Processing received message source: split:6, stream: default, id: {}, [apple]
		15400 [Thread-16-split] INFO  backtype.storm.daemon.task - Emitting: split default [a]
		15399 [Thread-10-count] INFO  backtype.storm.daemon.task - Emitting: count default [an, 53]
		15400 [Thread-12-count] INFO  backtype.storm.daemon.executor - Processing received message source: split:6, stream: default, id: {}, [a]
		15400 [Thread-16-split] INFO  backtype.storm.daemon.task - Emitting: split default [day]
		15400 [Thread-8-count] INFO  backtype.storm.daemon.task - Emitting: count default [apple, 53]
		15401 [Thread-10-count] INFO  backtype.storm.daemon.executor - Processing received message source: split:6, stream: default, id: {}, [day]
		15401 [Thread-16-split] INFO  backtype.storm.daemon.task - Emitting: split default [keeps]
		15401 [Thread-12-count] INFO  backtype.storm.daemon.task - Emitting: count default [a, 53]

##To package and deploy to HDInsight

While you can package and deploy this to an HDInsight cluster, it's pretty boring since this topology doesn't generate any output files. So you can see it running, and creating multiple instances, but that's about it.

Use the following command to create a .jar package for the topology.

	mvn package

This will create a file named `WordCount-1.0-SNAPSHOT.jar` in the `target` directory.
	
Use one of the following links to learn how to deploy the jar file to a Storm on HDInsight cluster:

* [Deploy a Storm topology to a Linux-based HDInsight cluster](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-storm-deploy-monitor-topology-linux/)

* [Deploy a Storm topology to a Windows-based HDInsight cluster](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-storm-deploy-monitor-topology/)
