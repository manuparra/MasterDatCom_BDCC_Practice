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
      * [Docker Cluster port assignment for VIRTUAL MACHINES](# On Docker Cluster this is the account port assignment)
      * [Hadoop Cluster port assignment for DOCKER CONTAINERS](# On Hadoop Cluster this is the account port assignment for DOCKER CONTAINERS)

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

**Try tu use a strong password mixing: upper, lower, and punctiation**



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

´´´
passwd
´´´

**Try tu use a strong password mixing: upper, lower, and punctiation**


## Infrastructure

What is the infrastructure of the practice?

The complete infraestructure for the practice is the next:

![CompleteStruct](https://sites.google.com/site/manuparra/home/architecturecc.png)

As you see, inside docker you will work with VIRTUAL MACHINES and inside hadoopyou will work with CONTAINERS.

*IMPORTANT*: Ports assignment

### On Docker Cluster this is the account port assignment

*Account ending in ....XXX*

### On Hadoop Cluster this is the account port assignment for DOCKER CONTAINERS

*Account ending in ....XXX*


# Starting with OPENNEBULA

Go to: [First steps with OpenNebula](./starting_OpenNebula.md)

# Starting with DOCKER

Go to: [First steps with Docker Containers](./starting_docker.md)

