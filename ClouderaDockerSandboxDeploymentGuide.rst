    |image0|

============================================
Cloudera Docker Sandbox Deployment Guide
============================================

Contents

**`Cloudera Docker Sandbox Deployment
Guide <#cloudera-docker-sandbox-deployment-guide>`__
`3 <#cloudera-docker-sandbox-deployment-guide>`__**

    `About <#about>`__ `3 <#about>`__

    `Prerequisites <#prerequisites>`__ `3 <#prerequisites>`__

**`Installation <#installation>`__ `3 <#installation>`__**

    `Step 1: Create an EC2 instance <#step-1-create-an-ec2-instance>`__
    `3 <#step-1-create-an-ec2-instance>`__

    `Step 2: Create Script to Start Docker
    Container <#step-2-create-script-to-start-docker-container>`__
    `3 <#step-2-create-script-to-start-docker-container>`__

    `Step 3: Login to the Cloudera Container and Start Cloudera
    Manager <#step-3-login-to-the-cloudera-container-and-start-cloudera-manager>`__
    `4 <#step-3-login-to-the-cloudera-container-and-start-cloudera-manager>`__

    `Step 4: Build a Cloudera Distribution of PCNG and Copy it to the
    Docker
    Container <#step-4-build-a-cloudera-distribution-of-pcng-and-copy-it-to-the-docker-container>`__
    `5 <#step-4-build-a-cloudera-distribution-of-pcng-and-copy-it-to-the-docker-container>`__

    `Step 5: Install PCNG in the Docker
    Container <#step-5-install-pcng-in-the-docker-container>`__
    `6 <#step-5-install-pcng-in-the-docker-container>`__

**`Shutting down the container when not in
use <#shutting-down-the-container-when-not-in-use>`__
`8 <#shutting-down-the-container-when-not-in-use>`__**

**`Starting up an Existing EC2 instance and Cloudera Docker
Container <#starting-up-an-existing-ec2-instance-and-cloudera-docker-container>`__
`8 <#starting-up-an-existing-ec2-instance-and-cloudera-docker-container>`__**

Cloudera Docker Sandbox Deployment Guide
========================================

About
-----

In some cases you may want to deploy a Cloudera sandbox in AWS for a
team to perform a simple proof of concept or avoid system resource usage
on your local computer. Cloudera offers a Docker image that is simliar
to the Cloudera sandbox that you would download and install to your
laptop.

+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Warning   | Once you create the docker container called "cloudera" do not remove the container unless you intend to delete all of your work and start cleanly. There are instructions below on how to start and stop an existing container to retain your data   |
+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

If you don’t know Docker and want help setting this up contact Jeremy
Merrifield.

Prerequisites
-------------

You need access to an AWS instance and permission to create an EC2
instance

Installation
============

Step 1: Create an EC2 instance
------------------------------

For this document we will configure a CoreOS AMI which is optimized for
running Docker images.

1. Choose an AMI for the region you will configure the EC2 instance in.

    https://coreos.com/os/docs/latest/booting-on-ec2.html

2. Create the EC2 instance. You might want to add more disk space than
   the default 8GB.

3. If you are on the Teradata VPN add the following to your security
   group using "All TCP" for each one. If you are not on VPN add your
   current IP address and choose "All TCP".

    136.179.11.144/29

    141.206.0.0/16

    153.64.0.0/15

    153.65.16.10/32

4. After starting up the instance, Login to the EC2 instance:

    $ ssh -i <private\_key> core@<IP\_ADDRESS>

Step 2: Create Script to Start Docker Container
-----------------------------------------------

Create a shell script to startup the Docker container. This makes it
easier to create a new container if you decided to delete it at some
point and start clean.

1. vi startCloudera.sh

2. Add the following:

    #!/bin/bash

    docker run --name cloudera \\

    --hostname=quickstart.cloudera \\

    --privileged=true -t -d \\

    -p 8888:8888 \\

    -p 7180:7180 \\

    -p 80:80 \\

    -p 7187:7187 \\

    -p 8079:8079 \\

    -p 8400:8400 \\

    -p 8161:8161 \\

    cloudera/quickstart:5.7.0-0-beta /usr/bin/docker-quickstart

3. Change permissions:

.. code-block:: shell

    $ chmod 744 startCloudera.sh

4. Start the Container:

.. code-block:: shell

    $ ./startCloudera.sh

    It will have to first download the Docker image, which is about 4GB,
    so give it some time.

Step 3: Login to the Cloudera Container and Start Cloudera Manager
------------------------------------------------------------------

1. Login to the Docker container:

.. code-block:: shell

    $ docker exec -it cloudera bash

2. Start Cloudera Manager:

.. code-block:: shell

    $ /home/cloudera/cloudera-manager --express

3. Login to Cloudera Manager at <EC2\_HOST>:7180 (username/password is
   cloudera/cloudera ).

4. Start all services in Cloudera Manager.

5. After it’s started exit the container to go back to the CoreOS host.

Step 4: Build a Cloudera Distribution of PCNG and Copy it to the Docker Container
---------------------------------------------------------------------------------

1. Modify the pom.xml file for the thinkbig-services-app module. Change:

    <dependency> 

    <groupId>com.thinkbiganalytics.datalake</groupId> 

    <artifactId>thinkbig-service-monitor-ambari</artifactId> 

    <version>0.3.0-SNAPSHOT</version> 

    </dependency/>

    To

    <dependency> 

    <groupId>com.thinkbiganalytics.datalake</groupId> 

    <artifactId>thinkbig-service-monitor-cloudera</artifactId> 

    <version>0.3.0-SNAPSHOT</version> 

    </dependency/>

2. From the data-lake-accelerator root folder, run:

.. code-block:: shell

    $ mvn clean install -o -DskipTests

3. Copy the new RPM file to the CoreOS box.

.. code-block:: shell

    $ scp -i ~/.ssh/<EC2\_PRIVATE\_KEY>
    <DLA\_HOME>/install/target/rpm/thinkbig-datalake-accelerator/RPMS/noarch/thinkbig-datalake-accelerator
    core@<EC2\_IP\_ADDRESS>:/home/core

4. From the CoreOS host, copy the RPM file to the Docker container.

.. code-block:: shell

    $ docker cp
    /home/core/thinkbig-datalake-accelerator-<VERSION>.noarch.rpm
    cloudera:/tmp

Step 5: Install PCNG in the Docker Container
--------------------------------------------

1. Login to the cloudera Docker container.

.. code-block:: shell

    $ docker exec -it cloudera bash

    $ cd /tmp

2. Create Linux Users and Groups.

    Creation of users and groups is done manually because many
    organizations have their own user and group management system. Therefore we cannot script it as part of the RPM
    install.

.. code-block:: shell

    $ useradd -r -m -s /bin/bash nifi

    $ useradd -r -m -s /bin/bash thinkbig

    $ useradd -r -m -s /bin/bash activemq

    Validate the above commands created a group as well by looking at
    /etc/group. Some operating systems

    may not create them by default.

.. code-block:: shell

    $ cat /etc/group

    If the groups are missing then run the following:

.. code-block:: shell

    $ groupadd thinkbig

    $ groupadd nifi

    $ groupadd activemq

3. Follow the instructions in the Deployment Wizard guide to install the
   RPM and other components.

    NOTE: There is an issue installing the database script so say No to
    the wizard step asking to install the database script. We will do
    that manually. I will update this section when it's fixed.

4. Follow these steps, that are not in the wizard deployment guide but
   are required to run PCNG in this environment:

   a. Run the database scripts:

      $ /opt/thinkbig/setup/sql/mysql/setup-mysql.sh root cloudera

   b. Edit /opt/thinkbig/thinkbig-services/conf/application.properties:

      Make the following changes in addition to the Cloudera specific
      changes described in the Appendix section of the wizard deployment
      guide for Cloudera

      ###Ambari Services Check

      #ambariRestClientConfig.username=admin

      #ambariRestClientConfig.password=admin

      #ambariRestClientConfig.serverUrl=http://127.0.0.1:8080/api/v1

      #ambari.services.status=HDFS,HIVE,MAPREDUCE2,SQOOP

      ###Cloudera Services Check

      clouderaRestClientConfig.username=cloudera

      clouderaRestClientConfig.password=cloudera

      clouderaRestClientConfig.serverUrl=127.0.0.1

      cloudera.services.status=HDFS/[DATANODE,NAMENODE],HIVE/[HIVEMETASTORE,HIVESERVER2],YARN

      ##HDFS/[DATANODE,NAMENODE,SECONDARYNAMENODE],HIVE/[HIVEMETASTORE,HIVESERVER2],YARN,SQOOP

   c. Add the "thinkbig" user to the supergroup:

.. code-block:: shell

      $ usermod -a -G supergroup thinkbig

   d. Run the following commands to address an issue with the Cloudera Sandbox and fix permissions.

.. code-block:: shell

      $ su - hdfs 

      $ hdfs dfs -chmod 775 /

5. Start up the Think Big Apps:

.. code-block:: shell

    $ /opt/thinkbig/start-thinkbig-apps.sh

6. Try logging into <EC2\_HOST>:8400 and <EC2\_HOST>:8079.

Shutting down the container when not in use
===========================================

EC2 instance can get expensive to run. If you don’t plan to use the
sandbox for a period of time we recommend shutting down the EC2
instance. Here are instructions on how to safely shut down the Cloudera
sandbox and CoreOS host.

1. Login to Cloudera Manager and tell it to stop all services.

2. On the CoreOS host type "docker stop cloudera".

3. Shutdown the EC2 Instance.

Starting up an Existing EC2 instance and Cloudera Docker Container
==================================================================

1. Start the EC2 instance.

2. Login to the CoreOS host.

3. Type "docker start cloudera" to start the container.

4. SSH into the docker container.

.. code-block:: shell

    $ docker exec -it cloudera bash

5. Start Cloudera Manager.

.. code-block:: shell

    $ /home/cloudera/cloudera-manager --express

6. Login to Cloudera Manager and start all services.

.. |image0| image:: media/common/thinkbig-logo.png
   :width: 3.04822in
   :height: 2.00392in
