---
services: hdinsight
platforms: java
author: blackmist
---

# Java-based word count topology

A basic example of a Java-based Apache Storm Topology that can be used with Storm on HDInsight. This project demonstrates two ways of defining a Java-based Storm topology; one defines the topology programatically in Java, while the other defines the topology using [Flux](https://storm.apache.org/releases/0.10.0/flux.html).

The primary difference between the two projects is that defining a topology using Flux separates configuration from implementation. With Flux, the topology (including configuration parameters,) are defined in a YAML file that is provided when you start the topology. This allows you to easily change the configuration without having to recompile the project.

NOTE: Flux is available with Storm 0.10.x, which is included with Storm on HDInsight 3.3 and 3.4. If you are using an older version of Storm on HDinsight, you cannot use Flux and should instead use the project in the `Java` directory.

See [Develop a Java topology for Storm on HDInsight](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-storm-develop-java-topology) for a walkthrough of the steps used to create this project.

NOTE: This project assumes Storm 0.10.0, which is available with Storm on HDInsight cluster versions 3.3 and 3.4.

## Flux topology

### To run on your development environment

1. Fork/Clone the repository to your development environment.

2. Install Java JDK 7 or higher. This was tested with Oracle Java 7 and 8, but should work under things like OpenJDK as well.

3. Install [Maven](http://maven.apache.org/).

4. Assuming Java and Maven are both in the path, and everything is configured fine for JAVA_HOME, use the following to build and run the topology on the development environment:

        mvn compile exec:java -Dexec.args="--local -R /topology.yaml"

    If you are using PowerShell, use the following:
    
        mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"

    NOTE: If you are on a Linux/Unix/OS X system, and have [installed Storm in your development environment](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), you can use the following commands instead:

        mvn compile package
        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml

    The `--local` parameter runs the topology in local mode on your development environment. The `-R /topology.yaml` parameter uses the `topology.yaml` file resource from the jar file to define the topology.

    As it runs, the topology will display startup information. Then it begins to display lines similar to the following as sentences are emitted from the spout and processed by the bolts.

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    
    There will be a 10 second delay between batches of logged information, as the WordCount component waits on a tick tuple before emitting, and the default timeout defined in the YAML file is 10 seconds.

5. Make a copy of the `topology.yaml` file from the project. Call it something like `newtopology.yaml`. In the file, find the following section and change the value of `10` to `5`. This changes the interval between emitting batches of word counts from 10 seconds to 5.

          - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 10
            parallelism: 1

6. To run the topology, use the following command:

        mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"

    Or, if you have Storm on your Linux/Unix/OS X development environment:

        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml

    Change the `/path/to/newtopology.yaml` to the path to the newtopology.yaml file you created in the previous step. This command will use the newtopology.yaml as the topology definition.

    Once the topology starts, you should notice that the time between emitted batches has changed to reflect the value in newtopology.yaml. So you can see that you can change your configuration through a YAML file without having to recompile the topology.

## Java topology

### To run on your development environment

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

### To package and deploy to HDInsight

While you can package and deploy this to an HDInsight cluster, it's pretty boring since this topology doesn't generate any output files. So you can see it running, and creating multiple instances, but that's about it.

Use the following command to create a .jar package for the topology.

	mvn package

This will create a file named `WordCount-1.0-SNAPSHOT.jar` in the `target` directory.
	
Use one of the following links to learn how to deploy the jar file to a Storm on HDInsight cluster:

* [Deploy a Storm topology to a Linux-based HDInsight cluster](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-storm-deploy-monitor-topology-linux/)

* [Deploy a Storm topology to a Windows-based HDInsight cluster](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-storm-deploy-monitor-topology/)

## Project code of conduct

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.