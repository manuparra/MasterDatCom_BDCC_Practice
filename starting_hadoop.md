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

Write the next command:

```
hdfs dfs -ls /tmp/datasetBDCC/ECBDL14/
```

## Dataset features:

Dataset from Evolutionary Computation for Big Data and Big Learning Workshop.

The dataset select for this competition comes from the Protein Structure Prediction field, and it was originally generated to train a predictor for the residue-residue contact prediction track of the CASP9 competiton. 
The dataset has 32 million instances, 631 attributes, 2 classes. For this workshop, reduced version of the file will be provided:

 - 10 features
- 31992921 rows
- 1.2 GB



