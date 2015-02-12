## Spark 

[Apache Spark](https://spark.apache.org) is a relatively new cluster computing framework, developed originally at UC Berkeley. It significantly generalizes
the 2-stage Map-Reduce paradigm popularized by Google and Apache Hadoop, and is instead based on the abstraction of **resilient distributed datasets (RDDs)**. An RDD is basically a distributed collection 
of items, that can be created in a variety of ways. Spark provides a set of operations to transform one or more RDDs into an output RDD, and analysis tasks are written as
chains of these operations.

Spark can be used with the Hadoop ecosystem, including the HDFS file system and the YARN resource manager. 

### Installing Spark

1. Download the Spark package at http://spark.apache.org/downloads.html. We will use **Version 1.2.1, Pre-built for CDH 4**.

2. Move the extract file to the lab6 directory in the git repository, and uncompress it using: 

`tar zxvf spark-1.2.1-bin-cdh4.tgz`

3. This will create a new directory: `spark-1.2.1-bin-cdh4` -- `cd` into that directory.

We are ready to use Spark. 

### Using Spark

Spark provides support for three languages: Scala (Spark is written in Scala), Java, and Python. We will use Scala here -- you can follow the instructions at the tutorial
and quick start (http://spark.apache.org/docs/latest/quick-start.html) for other languages. 

1. `./bin/spark-shell`: This will start a Scala shell (it will also output a bunch of stuff about what Spark is doing). The relevant variables are initialized in this shell, but otherwise it is just a standard Scala shell.

2. `> val textFile = sc.textFile("README.md")`: This creates a new RDD, called `textFile`, by reading data from a local file. The `sc.textFile` commands create an RDD
containing one entry per line in the file.

3. You can see some information about the RDD by doing `textFile.count()` or `textFile.first()`, or `textFile.take(5)` (which prints an array containing 5 items from the
        RDD).

4. We recommend you follow the rest of the commands in the quick start guide (http://spark.apache.org/docs/latest/quick-start.html). Here we will simply do the Word Count
application.

### Word Count Application

`val counts = textFile.flatMap(line => line.split(" ")).map(word => (word, 1)).reduceByKey((a, b)  => a + b)`

The `flatmap` splits each line into words, and the following `map` and `reduce` basically do the word count (in a similar fashion to standard MapReduce wordcount -- see, e.g., [link](http://kickstarthadoop.blogspot.com/2011/04/word-count-hadoop-map-reduce-example.html).

### Running it as an Application

Instead of using a shell, you can also write your code as a Scala file, and *submit* that to the spark cluster. The Assignment2 directory contains the appropriate files (`simple.sbt` and `src/main/scala/SimpleApp.scala`) for doing this. First you need to assemble a jar file using `sbt package` command. 
This creates a jar file containing the app.

Then the following command executes the Spark job in a local manner (a simple change to the command can do this on a cluster, assuming you have
the cluster already running).

`~/Downloads/spark-1.2.1-bin-cdh4/bin/spark-submit --class "SimpleApp" --master "local[4]"   target/scala-2.10/simple-project_2.10-1.0.jar`

### More...

We encourage you to look at the [Spark Programming Guide](https://spark.apache.org/docs/latest/programming-guide.html) and play with the other RDD manipulation commands. 

### Assignment 

Modify the `SimpleApp.scala` to: (1) count bigrams instead of words, and (2) for each word, find the top 5 bigrams it is part of, along with their counts.

###Submission

Submit the following files:

	- Your python script.

	- The output file.
