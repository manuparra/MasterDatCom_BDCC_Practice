# Máster en Ciencia de Datos e Ingeniería de Computadores. Prácticas de BigData y Cloud Computing. Curso 2016-2017. 

![Header](https://sites.google.com/site/manuparra/home/headerdicits.png)

Manuel J. Parra Royón (manuelparra@decsai.ugr.es) &  José. M. Benítez Sánchez (j.m.benitez@decsai.ugr.es)

[UGR](http://www.ugr.es) | [DICITS](http://dicits.ugr.es) | [SCI2S](http://sci2s.ugr.es) | [DECSAI](http://decsai.ugr.es)


Table of Contents
==================

   * [Starting with OpenNebula](#starting-with-opennebula)
      * [First steps with OpenNebula](#first-steps-with-opennebula)
         * [onemarket](#onemarket)
         * [oneimage](#oneimage)
         * [onevnet](#onevnet)
         * [onetemplate](#onetemplate)
         * [onevm](#onevm)
      * [onehost](#onehost)
      * [Deploy Virtual Machines with OpenNebula](#deploy-virtual-machines-with-opennebula)
         * [Authentication in OpenNebula](#authentication-in-opennebula)
         * [List available images on our OpenNebula system](#list-available-images-on-our-opennebula-system)
         * [Listing Virtual Networks for your user](#listing-virtual-networks-for-your-user)
         * [Create a Virtual Machine instance template](#create-a-virtual-machine-instance-template)
         * [Launch Virtual Machine instance](#launch-virtual-machine-instance)
         * [How to connect  (SSH) to the created Virtual Machine](#how-to-connect--ssh-to-the-created-virtual-machine)
      * [Manage Virtual Machine on SunStone Website](#manage-virtual-machine-on-sunstone-website)


# Starting with OpenNebula

OpenNebula enable two ways of interaction:

- Command line applications
  - Using [putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) (from windows) or ssh command (from *NIX systems)
- Web Application
  - From your browser

## First steps with OpenNebula command line

Firstly try to access to the docker ugr server:

```
ssh manuparra@docker....
```

Change `manuparra` with your `user login`: 

```
ssh mdatXXXXXXX@docker....
```

Use your previously changed password, when asked.


**The first action when login is the next:**

``
oneuser login mccXXXXXXX —-ssh —-force
``


This command is very important, due to,  it creates the session with OpenNebula and it allows to use all commands on OpenNebula.





**OpenNebula commands**:

```
onemarket         onevm             oneacct           onedatastore
onegroup          onetemplate       onevnet           oneacl
onedb             onehost           oneuser           onezone
onecluster        oneimage          onevcenter        ...
```

### onemarket

It allows to list the images of Operating systems, data, etc. that we can import to add to our cloud available from a MarketPlace

List of OSs available to run:

```
onemarket list
```

### oneimage

It allows to list the images that are in the system ready to be used for a display of virtual machines

List of imported images available to work:


```
oneimage list
```

### onevnet

Allows you to manage the virtual networks that have been created.

List of virtual networks created:

```
onevnet show <ID>
```

### onetemplate

It allows to manage templates of generation of virtual machines, so that we can create templates that have certain attributes of memory, capacity, etc. etc.

List of available templates

```
onetemplate list
onetemplate show <ID>
```

It is normal to appear empty since we have not yet made any template. We will create it later.

### onevm

It allows to manage the virtual machines launched, to know the state, to modify the state, to know the IP / Network, resources, etc., etc.

List of virtual machines available:

```
onevm list
```

It is normal to appear empty since we have not yet launched any virtual machine. We will create it later.

### onehost

Allows you to manage the hosts and resources of the cloud created with OpenNebula

List of hosts of the system and its state:

```
onehost list
```

It appears empty since HOSTS are managed from a privileged user profile.


## Working and Deploy Virtual Machines with OpenNebula

Access to the docker ugr server:

```
ssh manuparra@docker....
```

### Authentication in OpenNebula

This is the first step before working with OpenNebula:

```
oneuser login <youruserlogin> --ssh --force
```

for example:

```
oneuser login manuparra --ssh --force
```

or in your case:

```
oneuser login mccXXXXXX --ssh --force
```

If this command works fine, you are allow to use and connect with OpenNebula.

### List available images on our OpenNebula system

```
oneimage list
```

It will show:


```
 ID USER       GROUP      NAME            DATASTORE     SIZE TYPE PER STAT RVMS
   8 oneadmin   users      CentOS-6.5-one- default        10G OS    No used    1
   9 oneadmin   users      CentOS-7        default        10G OS    No rdy     0
  10 oneadmin   users      Ubuntu-14.04    default        10G OS    No rdy     0
  11 oneadmin   users      Hadoop 1.2 Mast default       1.3G OS    No rdy     0
  12 oneadmin   users      Hadoop 1.2 Slav default       1.3G OS    No rdy     0
  ...
```

Images are referenced by *"ID"* or *"NAME"*. You can use those IDENTIFIERS to reference list elements.

If we want to use CENTOS7, we need the ID => 9 o NAME => "CentOS-7".

### Listing Virtual Networks for your user

Now we have to verify that we have created our Virtual Network to be able to launch the machines within a correct IP address space:

```
onevnet list
```

and look forward for your Virtual Network

```
 ID USER            GROUP        NAME                CLUSTER    BRIDGE   LEASES
   0 oneadmin        oneadmin     private_vnet             -     br0           0
   1 usertest        users        private1_vnet            -     br0           0
   2 openstack       users        openstack_vnet           -     br0           0
   3 datavisuali     users        datavisuali_vnet         -      br0           0
   ...
```

Remember your Virtual Network **ID** or Virtual Network **NAME**.

### Create a Virtual Machine instance template

Schema:

![OpenNebulatemplate](https://sites.google.com/site/manuparra/home/opennebulatemplate.png)

We use ``onetemplate`` with this syntax:

```
onetemplate create --name <nametemplate> --cpu <numcpu> --vcpu <numvcpu> --memory <mem> --arch x86_64 --disk <imageid> --nic <idvnet> --vnc --ssh --net_context
```

for example:

```
onetemplate create --name "Plantilla_CentOS" --cpu 1 --vcpu 1 --memory 512 --arch x86_64 --disk 9 --nic "manuelparra_vnet" --vnc --ssh --net_context
```

Those are the parameters explained:

- --name :   Name of the template to indentify on OpenNebula
- --cpu : Number of CPUs. Integer.
 - --vcpu : Number of Virtual CPUs: can be 0.5, 1, 1.5 .... etc.
- --memory : RAM in MB, GB
- --arch:  Architecture: x86_64, x86, … It should be the same than the image.
- --disk: ID of the DISK. This ID can be searched from 		oneimage list command.
- --nic : Name or ID of the VirtualNetwork
- --ssh: Access from SSH.
- --net_context: Network Context

To verify if the template was created:


```
onetemplate list
```



If we want to delete the template, first get the selected ID or NAME and then:

```
onetemplate delete <ID>
```

### Launch Virtual Machine instance

First, list your Virtual Machines list:

```
onevm list
```

and check again the list of my templates:

```
onetemplate list
```

And now, we create a new instance of a Virtual Machine:

```
onetemplate instantiate <IDtemplate>
```

for example:


```
onetemplate instantiate 63
```

It will returns an ID of the created Virtual Machine.


Because the start of the virtual machine takes time, we will see the state at each moment, so check list of your Virtual Machines:

```
onevm list
```

It will show:

```
ID USER     GROUP    NAME            STAT UCPU    UMEM HOST             TIME
5 manuelpa users    Plantilla_CentO runn    0    512M noded07      0d 01h13
```

In the STAT column we have the current state. RUNNING = RUN
And information about Memory, HOST, etc.

### How to connect (SSH) to the created Virtual Machine


Check the ID of your Virtual Machine:

```
onevm list
```

and get your Virtual Machine ID:

```
onevm show <ID> 
```

```
onevm show 60
```

It will returns all information about your Virtual Machine.

The part that interests us of the details is to know that IP has been assigned to this machine created:

```
CONTEXT=[
  DISK_ID="1",
  ETH0_DNS="150.214.191.10",
  ETH0_GATEWAY="192.168.10.1",
  ETH0_IP="192.168.12.148",
  ETH0_MAC="02:00:c0:a8:0c:94",
  NETWORK=“YES",
  ....
```

So, our Virtual Machine IP to connect with SSH is: ``192.168.10.148``

We try to connect to the Virtual Machine:

```
ssh root@192.168.12.148
```

NOTE: It doesn't need password. WHY ?

Verify if you are connected to Internet:

```
ping -c2 google.com
```

## HOW to Manage Virtual Machines Platform from SunStone Web Application

Go to: http://docker.ugr.es:9869/ and follow the next steps:

- Connect to docker server ugr: ``ssh manuparra@.....``
- Execute: ``cat .one/one_auth`` it will show your SunStone credentials in this format: ``<user>:<password>`` , for example: ``manuparra:7374j31g74hd7234``
- Copy the corresponding part of password and paste http://docker.ugr.es:9869/ login and password.






