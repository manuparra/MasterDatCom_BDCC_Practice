# Máster en Ciencia de Datos e Ingeniería de Computadores. Prácticas de BigData y Cloud Computing. Curso 2016-2017. 

![Header](https://sites.google.com/site/manuparra/home/headerdicits.png)

Manuel J. Parra Royón (manuelparra@decsai.ugr.es) 

[UGR](http://www.ugr.es) | [DICITS](http://dicits.ugr.es) | [SCI2S](http://sci2s.ugr.es) | [DECSAI](http://decsai.ugr.es)

Manuel J. Parra Royón (manuelparra@decsai.ugr.es)

# Primeros pasos con MapReduce y Hadoop

Table of Contents
=================

   * [Starting with Hadoop](#starting-with-hadoop)
      * [Verifying datasets](#verifying-datasets)
      * [Dataset features](#dataset-features)
   * [First example with simple WordCount](#first-example-with-simple-wordcount)
      * [Reading and studying the code](#read-and-study-the-code)
      * [Compiling and executing WordCount](#compiling-and-executing-wordcount)
      * [Check and manage MapReduce Jobs for this example](#check-and-manage-mapreduce-jobs-for-this-example)
   * [Calculate MIN of a big dataset](#calculate-min-of-a-big-dataset)
      * [Check source code of example of MIN](#check-source-code-of-example-of-min)
      * [Compiling and executing MIN function](#compiling-and-executing-min-function)
      * [Exercices:](#exercices)
   * [Calculate MAX of a big dataset](#calculate-max-of-a-big-dataset)
   * [Is the dataset balanced or unbalanced.](#check-if-dataset-if-balanced-or-unbalanced)
   * [Calculate AVG of a big dataset](#calculate-avg-of-a-big-dataset)
   * [References](#references)



# Starting with Hadoop

Log and connect to our hadoop cluster system with:

```
ssh manuparra@hadoop....
```

Change `manuparra` with your `user login`: 

```
ssh mdatXXXXXXX@hadoop....
```

If you don't kwnow your login, go to [Login](./README.md)


## Verifying datasets

Check if you can access to the dataset from HDFS:

Write the next sentences:

```
hdfs dfs -ls /tmp/BDCC/datasets/ECBDL14/
```

and

```
hdfs dfs -ls /tmp/BDCC/wordcount/
```

## Dataset features:

Dataset from Evolutionary Computation for Big Data and Big Learning Workshop.

The dataset select for this competition comes from the Protein Structure Prediction field, and it was originally generated to train a predictor for the residue-residue contact prediction track of the CASP9 competiton. 
The dataset has 32 million instances, 631 attributes, 2 classes. 
For this workshop, reduced version of the file will be provided:

- 10 features
- 1 class attribute
- 31.992.921 rows
- 1.2 GB

Full specs [here](http://cruncher.ncl.ac.uk/bdcomp/).


# First example with simple WordCount

Before we jump into the details, lets walk through an example MapReduce application to get a flavour for how they work.

WordCount is a simple application that counts the number of occurrences of each word in a given input set.


![ExampleMapReduce](https://wikis.nyu.edu/download/attachments/74681720/WordCount%20MapReduce%20Paradigm.PNG?version=1&modificationDate=1462902481180&api=v2)


--> PAY ATTENTION: Working on FileSystem or HDFS !

We need to copy the source code (from HDFS)  of the WordCount application to our home folder:

Create a folder in your home for this project:

```
mkdir testwordcount
```

and change to this new folder:

```
cd testwordcount
```

And now copy de source code:

```
hdfs dfs -get /tmp/BDCC/wordcount/WordCount.java ./
```

## Read and study the code

```
cat WordCount.java
```

An now we study the code:

Libraries:

```
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.conf.*;
import org.apache.hadoop.io.*;
import org.apache.hadoop.mapred.*;
import org.apache.hadoop.util.*;
```


Class WordCount containing the Mapper and the Reducer: 

- Mapper:
	```
	public static class Map extends MapReduceBase 
		implements Mapper<LongWritable, Text, Text, IntWritable> {
		… … … /*Operaciones que hace el Map*/ … … …
	}
	```

- Reducer:
	```
 	public static class Reduce extends MapReduceBase 
    	  implements Reducer<Text, IntWritable, Text, IntWritable> {
		… … /*Operaciones que hace el Reduce*/ … … 
	}
	```


Full Mapper:

```
public static class Map extends MapReduceBase 
	implements Mapper<LongWritable, Text, Text, IntWritable> {
    
    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(LongWritable key, Text value, OutputCollector<Text, IntWritable> output, Reporter 		reporter) throws IOException {

      String line = value.toString();

	  //Convertimos el trozo de texto en palabras individuales
      StringTokenizer tokenizer = new StringTokenizer(line);

	  //Para todas las palabras que tenemos, almacenamos un 1 en cada ocurrencia
      while (tokenizer.hasMoreTokens()) {
        word.set(tokenizer.nextToken());
		 //Guardamos un 1 en cada palabra
        output.collect(word, one);
      }
    }

```


Full Reducer:

```
public static class Reduce extends MapReduceBase 
      implements Reducer<Text, IntWritable, Text, IntWritable> {
    
	public void reduce(Text key, Iterator<IntWritable> values, 			
		OutputCollector<Text, IntWritable> output, Reporter reporter) throws IOException {
     
 		//Ponemos el contador a 0
		int sum = 0;
		//Contamos las ocurrencias de las palabras
      	while (values.hasNext()) {
        	sum += values.next().get();
      	}
		//Almacenamos para cada palabra las veces que aparece.
      output.collect(key, new IntWritable(sum));
    }
  }
```

Main Class or Application MapReduce:

```
	 //La clase que se va usar y que servirá de referencia para el JobTracker
 	 JobConf conf = new JobConf(WordCount.class);
    conf.setJobName("wordcount");

	 //Como será la salida de los datos
    conf.setOutputKeyClass(Text.class);
    conf.setOutputValueClass(IntWritable.class);

	 //Clase que controla el Map
    conf.setMapperClass(Map.class);
	 //Clase que controla el Reduce
    conf.setCombinerClass(Reduce.class);
    conf.setReducerClass(Reduce.class);

    //Como será la entrada de los datos
    conf.setInputFormat(TextInputFormat.class);
    conf.setOutputFormat(TextOutputFormat.class);

	//De dónde tomamos la entrada de datos y donde ponemos la salida
    FileInputFormat.setInputPaths(conf, new Path(args[0]));
    FileOutputFormat.setOutputPath(conf, new Path(args[1]));

    JobClient.runJob(conf);

```

**Remember HDFS commands [here](starting_hdfs.md)**

## Compiling and executing WordCount

After that, try to create a folder for the wordcount classes:

```
mkdir wordcount_classes 
```

Compile the source code of ```WordCount.java``` :

```
javac -cp /usr/lib/hadoop/*:/usr/lib/hadoop-0.20-mapreduce/* -d wordcount_classes WordCount.java
```

Create the JAR:

```
/usr/java/jdk1.7.0_51/bin/jar -cvf wordcount.jar -C wordcount_classes/ .
```

In this step the Hadoop Application has been created and is ready to execute the word count on a dataset:

```
See the syntax:

hadoop jar <jar file> <main class> <HDFS INPUT dataset> <OUTPUT results>
```

So, execute the next:

```
hadoop jar wordcount.jar WordCount /tmp/BDCC/wordcount/dataset/joyceexample/ /user/mdatXXXXXXX/salida_wc_01/
```

or, on full version of the text:

```
hadoop jar wordcount.jar WordCount /tmp/BDCC/wordcount/dataset/joyceexample/ /user/mdatXXXXXXX/salida_wc_02/
```

Results will be stored on HDFS salida_wc_X folder on your HDFS folder.

Let's go with the **result format**:

List results:

```
hadoop dfs -ls  /user/mdatXXXXXXX/salida_wc_01/
```

Show results
```
hadoop dfs -cat  /user/mdatXXXXXXX/salida_wc_01/part-00000
```

**Comment this results: part-XXXXXXX**


## Check and manage MapReduce Jobs for this example

List of jobs running on Hadoop:

```
hadoop job -list
```

And it returns:

```
Total jobs:3
                  JobId	     State	     StartTime	    UserName	       Queue	  Priority	 UsedContainers	 RsvdContainers	 UsedMem	 RsvdMem	 NeededMem	   AM info
 job_1489653380280_0013	   RUNNING	 1489658819956	 manuelparra	root.manuelparra	    NORMAL	              1	              1	   2048M	  21504M	    23552M	http://hadoop.ugr.es:8088/proxy/application_1489653380280_0013/
 job_1489653380280_0015	   RUNNING	 1489659346034	 manuelparra	root.manuelparra	    NORMAL	              1	              1	   2048M	   7168M	     9216M	http://hadoop.ugr.es:8088/proxy/application_1489653380280_0015/
 job_1489653380280_0012	   RUNNING	 1489658461871	 manuelparra	root.manuelparra	    NORMAL	             15	              1	 704512M	   7168M	   711680M	http://hadoop.ugr.es:8088/proxy/application_1489653380280_0012/

```

Kill or remove a Job running or stopped or failed:


```
hadoop job -kill <JobId>
```

for instance:

```
hadoop job -kill job_1489653380280_0015
```


# Calculate MIN of a big dataset

Firstly some tricks:

**How to extract data / header from the bigdata file?:**

```
hadoop dfs -cat  /tmp/BDCC/datasets/ECBDL14/ECBDL14_10tst.data | head
```

**QUESTION: Can I use ``head /tmp/BDCC/datasets/ECBDL14/ECBDL14_10tra.data`` ??**


**Extract data / tail from the bigdata file:**

```
hadoop dfs -cat   /tmp/BDCC/datasets/ECBDL14/ECBDL14_10tst.data | tail
```

Dataset has the next structure:

```
...
0.217,0.045,-5,0,-4,0,-4,9,-7,3,0
0.221,0.076,-2,-2,1,-2,1,-4,-3,1,0
0.152,0.055,-4,-8,-3,0,-6,-2,1,-4,0
0.247,0.045,-2,2,-1,1,-3,-1,-1,0,0
0.227,0.077,-4,-1,-4,-4,-1,0,-2,0,0
0.289,0.100,-1,0,-1,-2,-6,-8,1,-8,0
0.198,0.043,-2,-3,-6,-1,-4,1,-4,-2,0
0.191,0.106,2,-2,-3,-3,2,-5,3,-5,0
0.395,0.065,-4,1,2,-1,-1,-4,3,-1,0
0.225,0.070,-1,-3,-3,0,-3,-5,4,4,0
...
```

Last attribute is the class ``0`` or ``1``.

Fields/Attributes separator: ``,``


## Check source code of example of MIN

In your home, create a folder:

```
mkdir minbigdata
```

Then copy source files to this folder:

```
cp /tmp/minbigdata/* /home/mdatXXXXXXXX/minbigdata/
```

Here is the source code of a hadoop application to calculate the MIN of an attribute or column.

And change to this folder:

```
cd minbigdata
```


The Mapper code:

```
public class MinMapper extends MapReduceBase implements Mapper<LongWritable, Text, Text, DoubleWritable> {
        private static final int MISSING = 9999;
        public static int col=5;

		public void map(LongWritable key, Text value, OutputCollector<Text, DoubleWritable> output, Reporter reporter) throws IOException {
                String line = value.toString();
                String[] parts = line.split(",");
                output.collect(new Text("1"), new DoubleWritable(Double.parseDouble(parts[col])));
        }
}
```

The Reduce code:

```
public class MinReducer extends MapReduceBase implements Reducer<Text, DoubleWritable, Text, DoubleWritable> {
	

	public void reduce(Text key, Iterator<DoubleWritable> values, OutputCollector<Text, DoubleWritable> output, Reporter reporter) throws IOException {
		Double minValue = Double.MAX_VALUE;
		while (values.hasNext()) {
			minValue = Math.min(minValue, values.next().get());
		}
	output.collect(key, new DoubleWritable(minValue));
	}
}}
```

## Compiling and executing MIN function

We create classes folder:

``
mkdir minbigdata_classes
``

and compile:

```
javac -cp /usr/lib/hadoop/*:/usr/lib/hadoop-0.20-mapreduce/* -d minbigdata_classes Min.java MinMapper.java MinReducer.java
```


create the JAR:

```
/usr/java/jdk1.7.0_51/bin/jar -cvf minbigdata.jar -C minbigdata_classes / .
```

and execute hadoop application:

```
hadoop jar minbigdata.jar oldapi.Min /tmp/BDCC/datasets/ECBDL14/ECBDL14_10tst.data  /user/mdatXXXXXX/salida_minbigdata/
```

This will produce the result in HDFS ``./salida_minbigdata`` folder.

List results:

```
hdfs dfs -ls /user/mdatXXXXXX/salida_minbigdata/*
```

Check the results:

```
hdfs dfs -cat /user/mdatXXXXXX/salida_minbigdata/part-XXXXX
```

### Exercices:

- What is the MIN for the Attribute ``E`` ? 
- What is the MIN for the Attribute ``A`` ? 


# Calculate MAX of a big dataset

Change Mapper and Reducer functions in order to calculate MAX of the column ``E`` and ``A``.

We can use the same code of MIN function.

What will be the changes? On Mapper or Reducer?

**Hint: Create a new folder and copy again de MIN code, and change it.**


# Check if dataset if balanced or unbalanced.

To check if the dataset is balanced or unbalanced, we need to know how many records there are for each class variables.

The class is the last column of the data and can have values of 0 and 1.

**Hint: The key (mapper key) must be the value of the class column (11).**

# Calculate AVG of a big dataset

Average is the sum of a list of numbers divided by the number of numbers in the list. 

# References

- http://sci2s.ugr.es/sites/default/files/files/Teaching/OtherPostGraduateCourses/CienciaDatosBigData/Bloque%20III.zip
- https://github.com/geftimov/MapReduce/blob/master/readme/MedianStdDev.md
- https://github.com/geftimov/MapReduce/blob/master/readme/MedianAndStandardDeviationCommentLengthByHour.md
- https://github.com/geftimov/MapReduce/blob/master/readme/MedianAndStandardDeviationCommentLengthByHour.md
- https://svn.apache.org/repos/asf/hadoop/common/trunk/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/WordStandardDeviation.java













