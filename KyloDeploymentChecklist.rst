    |image0|


Deployment Checklist
====================

This document provides a sample checklist to help prepare for a Kylo
deployment.

Edge Node Resource Requirements
-------------------------------

-  Minimum production recommendation is 4 cores CPU, 16 GB RAM.

-  Preferred production recommendation is 8 cores CPU, 32 GB RAM.

Dependencies
------------

+----------------------------------------------+
| Redhat/GNU/Linux distributions               |
+----------------------------------------------+
| RPM (for install)                            |
+----------------------------------------------+
| Java 1.8 (or greater)                        |
+----------------------------------------------+
| Hadoop 2.4+                                  |
+----------------------------------------------+
| Spark 1.5.x+                                 |
+----------------------------------------------+
| Apache NiFi 0.5+ (or Hortonworks DataFlow)   |
+----------------------------------------------+
| Hive                                         |
+----------------------------------------------+
| MySQL                                        |
+----------------------------------------------+

Linux User/Group Creation
-------------------------

There are three Linux users accounts that need to exist before
installing the Kylo stack. If an external user management tool is used,
these user accounts need to be created ahead of time. If not, there are
instructions in the deployment guide on how to create the users and
groups.

-  thinkbig

-  nifi

-  activemq

In addition, and group need to be created with the same names.

Edge Node Ports
---------------

The following ports are required to be open for browser access

-  Think Big UI – 8400

-  NiFi – 8079

-  ActiveMQ JMS – 61616 (only if on a different edge node than NiFi or
   Kylo)

The following is optional:

-  ActiveMQ Admin – 8161

Cluster Host Names, User Names, and Ports
-----------------------------------------

Collect the following information to speed up configuration of Kylo

-  Hive Hostname/IP Address:

-  Ambari IP Hostname/IP Address:

-  Ambari "kylo" user username/password:

-  KDC Hostname/IP Address:

-  MySQL Hostname/IP Address:

-  Kylo Edge Hostname/IP Address:

-  NiFi Edge Hostname/IP Address:

-  Kylo MySQL Installation User username/password (Create Schema
   Required):

-  Kylo MySQL application username/password (For the thinkbig-services
   application and Hive metadata access):

Kerberos Principals
-------------------

Note the following Kerberos principals after the step of creating the
Keytabs

-  Kerberos Principal for "thinkbig":

-  Kerberos Principal for "nifi":

-  Kerberos Principal for "hive" on the Hive Server2 Host:

Feed Categories
---------------

If you are installing Kylo using non-priviledged users you may need to
grant access to HDFS folders and hive databases ahead of time. If so,
determine what the initial category names in Kylo should be.

-  List of feed categories:

.. |image0| image:: media/common/thinkbig-logo.png
   :width: 3.04822in
   :height: 2.00392in
