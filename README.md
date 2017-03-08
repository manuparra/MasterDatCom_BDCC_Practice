# Máster Profesional Ingeniería Informática. Prácticas de Cloud Computing. Curso 2016-2017. 

![Header](https://sites.google.com/site/manuparra/home/headerdicits.png)

Manuel J. Parra Royón (manuelparra@decsai.ugr.es), Alberto Fernandez (alberto@decsai.ugr.es)  &  José. M. Benítez Sánchez (j.m.benitez@decsai.ugr.es)

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

- Two Cluster:

   - docker cluster  (15 nodes)
   - hadoop cluster  (15 nodes)

- docker cluster provides for each user:
   - Max 2 Virtual machines (and 2 IPs)
   - Max 2 ports redirected from external to internal Virtual Machine network

- hadoop cluster provides for each user:
   - Max 5 Docker container working at the same time
   - Max 5 ports redirected from external to internal Docker Containers


## Connecting to Docker Cluster UGR

Log in docker ugr server with your credentials

```
ssh manuparra@docker...
```

## Connecting to Hadoop Cluster UGR

Log in hadoop ugr server with your credentials (the same way):

```
ssh manuparra@hadoop...
```

## Infrastructure

The complete infraestructure for the practice is the next:

![CompleteStruct](https://sites.google.com/site/manuparra/home/architecturecc.png)

As you see, inside docker you will work with VIRTUAL MACHINES and inside hadoopyou will work with CONTAINERS.

*IMPORTANT*: Ports assignment

### On Docker Cluster this is the account port assignment

*Account ending in ....XXX*

ACCOUNT: ....998 
- docker cluster port 15060 go to 192.168.10.60 port 80
- docker cluster port 15061 go to 192.168.10.61 port 80

ACCOUNT: ....895 
- docker cluster port 15062 go to 192.168.10.62 port 80
- docker cluster port 15063 go to 192.168.10.63 port 80

ACCOUNT: ....731 
- docker cluster port 15064 go to 192.168.10.64 port 80
- docker cluster port 15065 go to 192.168.10.65 port 80

ACCOUNT: ....571 
- docker cluster port 15066 go to 192.168.10.66 port 80
- docker cluster port 15067 go to 192.168.10.67 port 80

ACCOUNT: ....287 
- docker cluster port 15068 go to 192.168.10.68 port 80
- docker cluster port 15069 go to 192.168.10.69 port 80

ACCOUNT: ...799 
- docker cluster port 15070 go to 192.168.10.70 port 80
- docker cluster port 15071 go to 192.168.10.71 port 80

ACCOUNT: ...176 
- docker cluster port 15072 go to 192.168.10.72 port 80
- docker cluster port 15073 go to 192.168.10.73 port 80

ACCOUNT: ...687 
- docker cluster port 15074 go to 192.168.10.74 port 80
- docker cluster port 15075 go to 192.168.10.75 port 80

ACCOUNT: ...432 
- docker cluster port 15076 go to 192.168.10.76 port 80
- docker cluster port 15077 go to 192.168.10.77 port 80

ACCOUNT: ...810 
- docker cluster port 15078 go to 192.168.10.78 port 80
- docker cluster port 15079 go to 192.168.10.79 port 80

ACCOUNT: ...323 
- docker cluster port 15080 go to 192.168.10.80 port 80
- docker cluster port 15081 go to 192.168.10.81 port 80

ACCOUNT: ...800 
- docker cluster port 15082 go to 192.168.10.82 port 80
- docker cluster port 15083 go to 192.168.10.83 port 80

ACCOUNT: ...141 
- docker cluster port 15084 go to 192.168.10.84 port 80
- docker cluster port 15085 go to 192.168.10.85 port 80

ACCOUNT: ...915 
- docker cluster port 15086 go to 192.168.10.86 port 80
- docker cluster port 15087 go to 192.168.10.87 port 80

ACCOUNT: ...773 
- docker cluster port 15088 go to 192.168.10.88 port 80
- docker cluster port 15089 go to 192.168.10.89 port 80

ACCOUNT: ...306 
- docker cluster port 15090 go to 192.168.10.90 port 80
- docker cluster port 15091 go to 192.168.10.91 port 81

ACCOUNT: ...532 
- docker cluster port 15092 go to 192.168.10.92 port 80
- docker cluster port 15093 go to 192.168.10.93 port 80

ACCOUNT: ...417 
- docker cluster port 15094 go to 192.168.10.94 port 80
- docker cluster port 15095 go to 192.168.10.95 port 80

ACCOUNT: ...575 
- docker cluster port 15096 go to 192.168.10.96 port 80
- docker cluster port 15097 go to 192.168.10.97 port 80


### On Hadoop Cluster this is the account port assignment for DOCKER CONTAINERS

*Account ending in ....XXX*

ACCOUNT: ....998 
- 5 ports: hadoop cluster port 14000-14004 to internal 14000-14004

ACCOUNT: ....895 
- 5 ports: hadoop cluster port 14005-14009 to internal 14005-14009

ACCOUNT: ....731 
- 5 ports: hadoop cluster port 14010-14014 to internal 14010-14014

ACCOUNT: ....571 
- 5 ports: hadoop cluster port 14015-14019 to internal 14015-14019

ACCOUNT: ....287 
- 5 ports: hadoop cluster port 14020-14024 to internal 14020-14024

ACCOUNT: ...799 
- 5 ports: hadoop cluster port 14025-14029 to internal 14025-14029

ACCOUNT: ...176 
- 5 ports: hadoop cluster port 14030-14034 to internal 14030-14034

ACCOUNT: ...687 
- 5 ports: hadoop cluster port 14035-14039 to internal 14035-14039

ACCOUNT: ...432 
- 5 ports: hadoop cluster port 14040-14044 to internal 14040-14044

ACCOUNT: ...810 
- 5 ports: hadoop cluster port 14045-14049 to internal 14045-14049

ACCOUNT: ...323 
- 5 ports: hadoop cluster port 14050-14054 to internal 14050-14054

ACCOUNT: ...800 
- 5 ports: hadoop cluster port 14055-14059 to internal 14055-14059

ACCOUNT: ...141 
- 5 ports: hadoop cluster port 14060-14064 to internal 14060-14064

ACCOUNT: ...915 
- 5 ports: hadoop cluster port 14065-14069 to internal 14065-14069

ACCOUNT: ...773 
- 5 ports: hadoop cluster port 14070-14074 to internal 14070-14074

ACCOUNT: ...306 
- 5 ports: hadoop cluster port 14075-14079 to internal 14075-14079

ACCOUNT: ...532 
- 5 ports: hadoop cluster port 14080-14084 to internal 14080-14084

ACCOUNT: ...417 
- 5 ports: hadoop cluster port 14085-14089 to internal 14085-14089

ACCOUNT: ...575 
- 5 ports: hadoop cluster port 14090-14094 to internal 14090-14094


# Starting with OPENNEBULA

Go to: [First steps with OpenNebula](./starting_OpenNebula.md)

# Starting with DOCKER

Go to: [First steps with Docker Containers](./starting_docker.md)

