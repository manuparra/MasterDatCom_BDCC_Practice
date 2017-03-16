# Máster en Ciencia de Datos e Ingeniería de Computadores. Prácticas de BigData y Cloud Computing. Curso 2016-2017. 

![Header](https://sites.google.com/site/manuparra/home/headerdicits.png)

Manuel J. Parra Royón (manuelparra@decsai.ugr.es) &  José. M. Benítez Sánchez (j.m.benitez@decsai.ugr.es)

[UGR](http://www.ugr.es) | [DICITS](http://dicits.ugr.es) | [SCI2S](http://sci2s.ugr.es) | [DECSAI](http://decsai.ugr.es)

Manuel J. Parra Royón (manuelparra@decsai.ugr.es) & José. M. Benítez Sánchez (j.m.benitez@decsai.ugr.es)

# Primeros pasos con MapReduce y Hadoop

Table of Contents
=================



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

```
hdfs dfs -ls /tmp/BDCC/wordcount/
```

and 

```
hdfs dfs -ls /tmp/BDCC/statsexample/
```


## Dataset features:

Dataset from Evolutionary Computation for Big Data and Big Learning Workshop.

The dataset select for this competition comes from the Protein Structure Prediction field, and it was originally generated to train a predictor for the residue-residue contact prediction track of the CASP9 competiton. 
The dataset has 32 million instances, 631 attributes, 2 classes. 
For this workshop, reduced version of the file will be provided:

- 10 features
- 31.992.921 rows
- 1.2 GB

Full specs [here](http://cruncher.ncl.ac.uk/bdcomp/).


# First example with simple WordCount

Before we jump into the details, lets walk through an example MapReduce application to get a flavour for how they work.

WordCount is a simple application that counts the number of occurrences of each word in a given input set.


![ExampleMapReduce](https://wikis.nyu.edu/download/attachments/74681720/WordCount%20MapReduce%20Paradigm.PNG?version=1&modificationDate=1462902481180&api=v2)

--> PAY ATTENTION: Working on FileSystem or HDFS !

We need to copy the source code (from HDFS)  of the WordCount application to our home folder:

Create a folder in your home:

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

**Remember HDFS commands [here](starting_hdfs.md)**


Then create a folder in your home folder:

```
mkdir wordcount_classes 
```

Compile the source code of WordCount.java :

```
javac -cp /usr/lib/hadoop/*:/usr/lib/hadoop-0.20-mapreduce/* -d wordcount_classes WordCount.java
```

Create the JAR:

```
/usr/java/jdk1.7.0_51/bin/jar -cvf wordcount.jar -C wordcount_classes/ .
```

In this step the Hadoop Application has been created and is ready to execute the word count on a dataset:

```
Syntax:

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

Let's go:

List results:

```
hadoop dfs -ls  /user/mdatXXXXXXX/salida_wc_01/
```

Show results
```
hadoop dfs -cat  /user/mdatXXXXXXX/salida_wc_01/part-00000
```

**Comment part-XXXXXXX**


# Calculate MIN of a big dataset

Extract data / header from the bigdata file:

```
hadoop dfs -cat  /tmp/BDCC/datasets/ECBDL14/ECBDL14_10tst.data | head
```

**QUESTION: Can I use ``head /tmp/BDCC/datasets/ECBDL14/ECBDL14_10tra.data`` ??**


Extract data / tail from the bigdata file:

```
hadoop dfs -cat   /tmp/BDCC/datasets/ECBDL14/ECBDL14_10tst.data | tail
```

Dataset have the next structure:

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

This will produce the result in HDFS ´´./salida_minbigdata´´ folder.

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


# Check if dataset if balanced or unbalanced.

To check if the dataset is balanced or unbalanced, we need to know how many records there are for each class variables.

The class is the last column of the data and can have values of 0 and 1.

Hint: The key (mapper key) must be the value of the class column (11).

# Calculate AVG of a big dataset

Average is the sum of a list of numbers divided by the number of numbers in the list. 

# References

- http://sci2s.ugr.es/sites/default/files/files/Teaching/OtherPostGraduateCourses/CienciaDatosBigData/Bloque%20III.zip

- https://github.com/geftimov/MapReduce/blob/master/readme/MedianStdDev.md

- https://github.com/geftimov/MapReduce/blob/master/readme/MedianAndStandardDeviationCommentLengthByHour.md

- https://github.com/geftimov/MapReduce/blob/master/readme/MedianAndStandardDeviationCommentLengthByHour.md

- https://svn.apache.org/repos/asf/hadoop/common/trunk/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/WordStandardDeviation.java













