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

Full specs ![here](http://cruncher.ncl.ac.uk/bdcomp/).


# First example with simple WordCount

Before we jump into the details, lets walk through an example MapReduce application to get a flavour for how they work.

WordCount is a simple application that counts the number of occurrences of each word in a given input set.


--> PAY ATTENTION: Working on FileSystem or HDFS !

We need to copy the source code of the WordCount application to our home folder:

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
jar -cvf wordcount.jar -C wordcount_classes/ .
```

In this step the Hadoop Application has been created and is ready to execute the word count on a dataset:

```
Syntax:

hadoop jar <jar file> <main class> <HDFS INPUT dataset> <OUTPUT results>
```

So, execute the next:

```
hadoop jar wordcount.jar WordCount /tmp/BDCC/wordcount/dataset/joyceexample/ salida_1
````

or, on full version of the text:

```
hadoop jar wordcount.jar WordCount /tmp/BDCC/wordcount/dataset/joyce/ salida_2
```

Results will be stored on HDFS salida_X folder on your HDFS folder.











