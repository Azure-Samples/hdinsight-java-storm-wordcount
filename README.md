---
services: hdinsight
platforms: java
author: blackmist
---

# Java-based word count topology

A basic example of a Java-based Apache Storm Topology that can be used with Storm on HDInsight.

See [Develop a Java topology for Storm on HDInsight](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-storm-develop-java-topology) for a walkthrough of the steps used to create this project.

NOTE: This project assumes Storm 0.10.0, which is available with Storm on HDInsight cluster versions 3.3 and 3.4.

##To run on your development environment

1. Fork/Clone the repository to your development environment.

2. Install Java JDK 7 or higher. This was tested with Oracle Java 7 and 8, but should work under things like OpenJDK as well.

3. Install [Maven](http://maven.apache.org/)

4. Assuming Java and Maven are in the path, and everything is configured fine for JAVA_HOME, use the following to build and run the topology on the development environment:

        mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology

	As it runs, the topology will display startup information. Then it begins to display lines similar to the following as sentences are emitted from the spout and processed by the bolts.

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 51 for word i
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 51 for word at
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 51 for word with
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 51 for word nature
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 51 for word two
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 51 for word am

##To package and deploy to HDInsight

While you can package and deploy this to an HDInsight cluster, it's pretty boring since this topology doesn't generate any output files. So you can see it running, and creating multiple instances, but that's about it.

Use the following command to create a .jar package for the topology.

	mvn package

This will create a file named `WordCount-1.0-SNAPSHOT.jar` in the `target` directory.
	
Use one of the following links to learn how to deploy the jar file to a Storm on HDInsight cluster:

* [Deploy a Storm topology to a Linux-based HDInsight cluster](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-storm-deploy-monitor-topology-linux/)

* [Deploy a Storm topology to a Windows-based HDInsight cluster](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-storm-deploy-monitor-topology/)

## Project code of conduct

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.