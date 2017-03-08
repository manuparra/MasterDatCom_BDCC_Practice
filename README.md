# Máster en Ciencia de Datos e Ingeniería de Computadores. Prácticas de BigData y Cloud Computing. Curso 2016-2017. 

![Header](https://sites.google.com/site/manuparra/home/headerdicits.png)

Manuel J. Parra Royón (manuelparra@decsai.ugr.es) &  José. M. Benítez Sánchez (j.m.benitez@decsai.ugr.es)

[UGR](http://www.ugr.es) | [DICITS](http://dicits.ugr.es) | [SCI2S](http://sci2s.ugr.es) | [DECSAI](http://decsai.ugr.es)


Table of Contents
=================


   * [1. Environment of the practice](#environment-of-the-practice)
      * [Connecting to Docker Server UGR](#connecting-to-docker-server-ugr)
      * [Connecting to Docker Containers](#connecting-to-docker-containers)
      * [Infrastructure](#infrastructure)
      * [Docker Cluster port assignment for VIRTUAL MACHINES](#on-docker-cluster-this-is-the-account-port-assignment)
      * [Hadoop Cluster port assignment for DOCKER CONTAINERS](#on-hadoop-cluster-this-is-the-account-port-assignment-for-docker-containers)

   * [2. Starting with OPENNEBULA](#starting-with-opennebula)
   * [3. Starting with DOCKER](#starting-with-docker)


# Environment of the practice

The working environment consists of the following structure for each user:

- Two Clusters:
   - docker cluster  (15 nodes)
   - hadoop cluster  (15 nodes)

- docker cluster provides for each user:
   - Max 1 Virtual machines (and 1 IPs)
   - Max 1 ports redirected from external to internal Virtual Machine network

- hadoop cluster provides for each user:
   - Max 2 Docker container working at the same time
   - Max 2 ports redirected from external to internal Docker Containers


![ClusterImage](https://sites.google.com/site/manuparra/home/clusterhadoop.jpg)

## Connecting to Docker Cluster UGR

Log in docker ugr server with your credentials

```
ssh manuparra@docker...
```

For this course use the next login credential:

- If your CardID number is spanish and similar to 77464888L,  your login will be: ``ssh mdat77464888@docker....``
- If your CardID number is not spanish or you are using your passport similar to YM5664883L, your login will be: ``ssh YM5664883@docker......``

The  general rule is:
- Remove your last letter of your CardID number


After login, **PLEASE CHANGE** your password using command:

```
passwd
```

**Try to use a strong password mixing: upper, lower, and punctiation**



## Connecting to Hadoop Cluster UGR

Log in hadoop ugr server with your credentials (the same way):

```
ssh manuparra@hadoop...
```

For this course use the next login credential:

- If your CardID number is spanish and similar to 77464888L,  your login will be: ``ssh mdat77464888@hadoop....``
- If your CardID number is not spanish or you are using your passport similar to YM5664883L, your login will be: ``ssh YM5664883@hadoop......``

The  general rule is:
- Remove your last letter of your CardID number


After login, **PLEASE CHANGE** your password using command:

```
passwd
```


**Try to use a strong password mixing: upper, lower, and punctiation**


## Infrastructure

What is the infrastructure of the practice?

The complete infraestructure for the practice is the next:

![CompleteStruct](https://sites.google.com/site/manuparra/home/ArchitectureBDCC.png)

As you see, inside docker you will work with VIRTUAL MACHINES and inside hadoopyou will work with CONTAINERS.

*IMPORTANT*: Ports assignment

### On Docker Cluster this is the account port assignment

*Account ending in ....XXX*

### On Hadoop Cluster this is the account port assignment for DOCKER CONTAINERS

### dockerports

*Account ending in ....XXX*

```
--> DOCKER PORTS redirected. User mdat......638 
Port: 16000 
Port: 16001
--> DOCKER PORTS redirected. User mdat......968 
Port: 16002 
Port: 16003
--> DOCKER PORTS redirected. User mdat......179 
Port: 16004 
Port: 16005
--> DOCKER PORTS redirected. User mdat......515 
Port: 16006 
Port: 16007
--> DOCKER PORTS redirected. User mdat......940 
Port: 16008 
Port: 16009
--> DOCKER PORTS redirected. User mdat......981 
Port: 16010 
Port: 16011
--> DOCKER PORTS redirected. User mdat......685 
Port: 16012 
Port: 16013
--> DOCKER PORTS redirected. User mdat......356 
Port: 16014 
Port: 16015
--> DOCKER PORTS redirected. User mdat......955 
Port: 16016 
Port: 16017
--> DOCKER PORTS redirected. User mdat......478 
Port: 16018 
Port: 16019
--> DOCKER PORTS redirected. User mdat......592 
Port: 16020 
Port: 16021
--> DOCKER PORTS redirected. User mdat......415 
Port: 16022 
Port: 16023
--> DOCKER PORTS redirected. User mdat......285 
Port: 16024 
Port: 16025
--> DOCKER PORTS redirected. User mdat......607 
Port: 16026 
Port: 16027
--> DOCKER PORTS redirected. User mdat......505 
Port: 16028 
Port: 16029
--> DOCKER PORTS redirected. User mdat......474 
Port: 16030 
Port: 16031
--> DOCKER PORTS redirected. User mdat......124 
Port: 16032 
Port: 16033
--> DOCKER PORTS redirected. User mdat......356 
Port: 16034 
Port: 16035
--> DOCKER PORTS redirected. User mdat......738 
Port: 16036 
Port: 16037
--> DOCKER PORTS redirected. User mdat......107 
Port: 16038 
Port: 16039
--> DOCKER PORTS redirected. User mdat......924 
Port: 16040 
Port: 16041
--> DOCKER PORTS redirected. User mdat......914 
Port: 16042 
Port: 16043
--> DOCKER PORTS redirected. User mdat......509 
Port: 16044 
Port: 16045
--> DOCKER PORTS redirected. User mdat......923 
Port: 16046 
Port: 16047
--> DOCKER PORTS redirected. User mdat......129 
Port: 16048 
Port: 16049
--> DOCKER PORTS redirected. User mdat......207 
Port: 16050 
Port: 16051
--> DOCKER PORTS redirected. User mdat......844 
Port: 16052 
Port: 16053
--> DOCKER PORTS redirected. User mdat......612 
Port: 16054 
Port: 16055
--> DOCKER PORTS redirected. User mdat......479 
Port: 16056 
Port: 16057
--> DOCKER PORTS redirected. User mdat......609 
Port: 16058 
Port: 16059
--> DOCKER PORTS redirected. User mdat......403 
Port: 16060 
Port: 16061
--> DOCKER PORTS redirected. User mdat......929 
Port: 16062 
Port: 16063
--> DOCKER PORTS redirected. User mdat......546 
Port: 16064 
Port: 16065
--> DOCKER PORTS redirected. User mdat......027 
Port: 16066 
Port: 16067
--> DOCKER PORTS redirected. User mdat......473 
Port: 16068 
Port: 16069
--> DOCKER PORTS redirected. User mdat......161 
Port: 16070 
Port: 16071
--> DOCKER PORTS redirected. User mdat......522 
Port: 16072 
Port: 16073
--> DOCKER PORTS redirected. User mdat......629 
Port: 16074 
Port: 16075
--> DOCKER PORTS redirected. User mdat......209 
Port: 16076 
Port: 16077
--> DOCKER PORTS redirected. User mdat......107 
Port: 16078 
Port: 16079
--> DOCKER PORTS redirected. User mdat......824 
Port: 16080 
Port: 16081
--> DOCKER PORTS redirected. User mdat......974 
Port: 16082 
Port: 16083
--> DOCKER PORTS redirected. User mdat......625 
Port: 16084 
Port: 16085
--> DOCKER PORTS redirected. User mdat......982 
Port: 16086 
Port: 16087
--> DOCKER PORTS redirected. User mdat......510 
Port: 16088 
Port: 16089
--> DOCKER PORTS redirected. User mdat......836 
Port: 16090 
Port: 16091
--> DOCKER PORTS redirected. User mdat......564 
Port: 16092 
Port: 16093
--> DOCKER PORTS redirected. User mdat......369 
Port: 16094 
Port: 16095
--> DOCKER PORTS redirected. User mdat......443 
Port: 16096 
Port: 16097
--> DOCKER PORTS redirected. User mdat......057 
Port: 16098 
Port: 16099
--> DOCKER PORTS redirected. User mdat......166 
Port: 16100 
Port: 16101
--> DOCKER PORTS redirected. User mdat......202 
Port: 16102 
Port: 16103
--> DOCKER PORTS redirected. User mdat......044 
Port: 16104 
Port: 16105
--> DOCKER PORTS redirected. User mdat......450 
Port: 16106 
Port: 16107
```



# Starting with OPENNEBULA

Go to: [First steps with OpenNebula](./starting_OpenNebula.md)

# Starting with DOCKER

Go to: [First steps with Docker Containers](./starting_docker.md)

