    |image0|

============================================
Kylo Deployment Guide
============================================

Data Lake Accelerator Deployment Guide
======================================

+---------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **Warning**   | There is an issue with the spark client using the SQL context with Orc tables using HDP 2.4.2. Please see this troubleshooting tip to configure the spark client to work around the issue: https://wiki.thinkbiganalytics.com/display/RD/Spark+SQL+fails+on+empty+ORC+table%2C+HDP+2.4.2   |
+---------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

About
=====

This document explains how to install the Data Lake Accelerator
framework as well as Elasticsearch, NiFi, and ActiveMQ. There are a few
different ways you can install it depending on whether or not you are
installing all components on one edge node vs. multiple nodes.

System Requirements
===================

**Dependencies**

The Data Lake Accelerator services should be installed on an edge node.
The following should be available prior to the installing the Data Lake
Starter.

See the Dependencies section in the deployment checklist: `Deployment
Checklist <https://github.com/ThinkBigAnalytics/data-lake-accelerator/blob/master/docs/latest/deployment/deployment-checklist.adoc>`__

Table 1. Tested Platforms

+-----------------------+-------------------------------------------------------------+----------------+
| **Platform**          | **URL**                                                     | **Version**    |
+-----------------------+-------------------------------------------------------------+----------------+
| Hortonworks Sandbox   | http://hortonworks.com/products/hortonworks-sandbox/        | HDP 2.3, 2.4   |
+-----------------------+-------------------------------------------------------------+----------------+
| Cloudera Sandobx      | http://www.cloudera.com/downloads/quickstart_vms/5-7.html   | 5.7            |
+-----------------------+-------------------------------------------------------------+----------------+

Prerequisites
=============

Hortonworks Sandbox
-------------------

If installing in a new Hortonworks sandbox make sure to do the following
first before running through the installation steps below.

`Configure Hortonworks
Sandbox <https://github.com/ThinkBigAnalytics/data-lake-accelerator/blob/master/docs/latest/hortonworks-sandbox.adoc>`__

Java Requirements
-----------------

Data Lake Accelerator requires Java 8 for NiFi, thinkbig-ui, and
thinkbig-services. If you already have Java 8 installed as the system
level Java you have the option to leverage that.

In some cases, such as with an HDP install, Java 7 is the system version
and you likely will not want to change it to Java 8. In this case you
can leverage the mentioned scripts below to download and configure Java
8 in the /opt/java directory. The scripts will also modify the startup
scripts for NiFi, thinkbig-ui and thinkbig-services to reference the
/opt/java JAVA\_HOME.

If you already have Java 8 installed in a different location you will
have the option to use that as well.

+------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **Note**   | When installing the RPM the applications are defaulted to use the /opt/java/current location. This default saves a step for developers so that they can uninstall and re-install the RPM without having to run any other scripts.   |
+------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Linux Users
-----------

If you will be creating Linux users as part of the install the commands
will be documented. If using an external user management system for
adding users to a linux box you must have those users created before
installing the Kylo stack. You will need a user/group including:

-  activemq

-  elasticsearch

-  thinkbig

-  nifi

+------------+--------------------------------------------------------+
| **Note**   | Those exact names are required (note the lowercase).   |
+------------+--------------------------------------------------------+

Installation
============

There are multiple procedures you can follow to deploy Kylo. In a test
and production environment you will likely want to follow the manual
installation guide as it has more detailed instructions on how to
install each individual component. For local development and 1 node
development boxes you can leverage the setup wizard procedure to quickly
bootstrap your environment.

+--------+---------------------------------------------------------------------------------------------------------------------+
| Note   | If you are installing Kylo on SUSE please read the following doc to work around ActiveMQ and Elasticsearch issues   |
+--------+---------------------------------------------------------------------------------------------------------------------+

`SUSE Configuration
Changes <https://github.com/ThinkBigAnalytics/data-lake-accelerator/blob/master/docs/latest/deployment/suse/suse-configuration-changes.adoc>`__

Install Procedure 1:
---------------------

Installing all components on one edge node with internet access using
the wizard.

Follow the steps below to install the data lake accelerator using the
installation wizard script. This is convenient for local sandboxes
(HDP/Cloudera) and 1 node development boxes. The WGET command is used to
download binaries so internet access is required.

Click on the below link to go to the wizard driven deployment
instructions

`Wizard Driven Deployment
Guide <https://github.com/ThinkBigAnalytics/data-lake-accelerator/blob/master/docs/latest/deployment/wizard-deployment-guide.adoc>`__

Install Procedure 2:
---------------------

Installing each component manually.

Click on the below link to go to the manual deployment instructions

`Manual Deployment
Guide <https://github.com/ThinkBigAnalytics/data-lake-accelerator/blob/master/docs/latest/deployment/manual-deployment-guide.adoc>`__

Install Procedure 3:
---------------------

Cloudera EC2 Docker Sandbox.

This is an option for those who want to deploy PCNG to a single node
Cloudera sandbox in AWS. This is useful when you need to get a quick
Cloudera instance running to test PCNG but don’t have the resources to
install a Cloudera cluster

`Cloudera EC2 Docker Sandbox Deployment
Guide <https://github.com/ThinkBigAnalytics/data-lake-accelerator/blob/master/docs/latest/deployment/cloudera-docker-sandbox.adoc>`__

Install Procedure 4:
---------------------

Tar file install (instead of RPM).

This is optional for those who have to install Kylo in a different
folder than /opt/thinkbig or run as a different user.

`Kylo TAR File Installation
Guide <https://github.com/ThinkBigAnalytics/data-lake-accelerator/blob/master/docs/latest/deployment/kylo-tar-install.adoc>`__

Install Procedure 5:
---------------------

HDP 2.5 Cluster Ranger/Kerberos with 2 Edge Nodes.

This document provides an example of how to install Kylo on an HDP 2.5
cluster with minimal admin privileges and shows how to configure
installation with NiFi on a separate edge node.

`HDP 2.5 Ranger/Kerberos Deployment
Guide <https://github.com/ThinkBigAnalytics/data-lake-accelerator/blob/master/docs/latest/deployment/hdp-2.5-ranger-kerberos-deployment.adoc>`__

Configuration
=============

Ranger / Sentry
---------------

If you’ve changed the default Ranger or Sentry permissions then you will
need to add permissions for Kylo and NiFi.

`Ranger Authorization
Guide <https://github.com/ThinkBigAnalytics/data-lake-accelerator/blob/master/docs/latest/security/authorization/ranger/EnableRangerAuthorization.adoc>`__

`Sentry Authorization
Guide <https://github.com/ThinkBigAnalytics/data-lake-accelerator/blob/master/docs/latest/security/authorization/sentry/EnableSentryAuthorization.adoc>`__

Kerberos
--------

If you are installing Kylo on a kerberos cluster you will need to
configure the applications before certain features will work

Optional: Configure Kerberos For Your Local HDP Sandbox
-------------------------------------------------------

This guide will help you enabled kerberos for your local development
sandbox for development and testing

`HDP 2.4 Sandbox Kerberos Setup
Example <https://github.com/ThinkBigAnalytics/data-lake-accelerator/blob/master/docs/latest/security/kerberos/kerberos-installation-example-hdp-2.4.adoc>`__

Step 1: Configure Kerberos for NiFi
-----------------------------------

Some additional configuration is required for allowing the NiFi
components to work with a Kerberos cluster.

`Configure NiFi for
Kerberos <https://github.com/ThinkBigAnalytics/data-lake-accelerator/blob/master/docs/latest/security/kerberos/nifi-configuration-kerboros-cluster.adoc>`__

**Step 2: Configure Kerberos for Kylo Applications**

Additional configuration is required for allowing some features in the
Kylo applications to work with a Kerberos cluster.

`Configure Kylo for
Kerberos <https://github.com/ThinkBigAnalytics/data-lake-accelerator/blob/master/docs/latest/security/kerberos/kylo-configuration-kerberos-cluster.adoc>`__

Configuration Files
===================

Configuration for the data lake accelerator services are located under
the following files:

/opt/thinkbig/thinkbig-ui/conf/application.properties

/opt/thinkbig/thinkbig-services/conf/application.properties

Encrypting Configuration Property Values
----------------------------------------

By default, a new Kylo installation does not have any of its
configuration properties encrypted. Once you have started Kylo for the
first time, the easiest way to derive encrypted versions of property
values is to post values to the Kylo services /encrypt endpoint to have
it generate an encrypted form for you. You could then paste the
encrypted value back into your properties file and mark it as encrypted
by prepending the values with {cipher}. For instance, if you wanted to
encrypt the Hive datasource password specified in
applicaition.properties (assuming the password is “mypassword”), you can
get it’s encrypted form using the curl command like this:

.. code-block:: shell

    $ curl localhost:8420/encrypt –d mypassword

    29fcf1534a84700c68f5c79520ecf8911379c8b5ef4427a696d845cc809b4af0

You would then copy that value and replace the clear text password
string in the properties file with the encrypted value:

    hive.datasource.password={cipher}29fcf1534a84700c68f5c79520ecf8911379c8b5ef4427a696d845cc809b4af0

The benefit of this approach is that you will be getting a value that is
guaranteed to work with the encryption settings of the server where that
configuration value is being used. Once you have replaced all properties
you wish encrypted in the properties files you can restart the Kylo the
services to use them.

Optimizing Performance
======================

You can adjust the memory setting for each services using the below
environment variables

    /opt/thinkbig/thinkbig-ui/bin/run-thinkbig-ui.sh

    export THINKBIG\_UI\_OPTS= -Xmx4g

    /opt/thinkbig/thinkbig-services/bin/run-thinkbig-services.sh

    export THINKBIG\_SERVICES\_OPTS= -Xmx4g

The setting above would set the Java maximum heap size to 4 GB.

Change the Java Home
--------------------

By default the thinkbig-services and thinkbig-ui application set the
JAVA\_HOME location to /opt/java/current. This can easily be changed by
editing the JAVA\_HOME environment variable in the following two files:

    /opt/thinkbig/thinkbig-ui/bin/run-thinkbig-ui.sh

    /opt/thinkbig/thinkbig-services/bin/run-thinkbig-services.sh

In addition, if you run the script to modify the NiFI JAVA\_HOME
variable you will need to edit:

    /opt/nifi/current/bin/nifi.sh

S3 Support For Data Transformations
-----------------------------------

Spark requires additional configuration in order to read Hive tables
located in S3. Please see the `Accessing S3 from the data
wrangler <https://wiki.thinkbiganalytics.com/display/RD/Accessing+S3+from+the+data+wrangler>`__
how-to article.

Starting and Stopping the Services Manually
===========================================

If you follow the instructions for the installations steps above all of
the below applications will be set to startup automatically if you
restart the server. In the Hortonworks sandbox the services for thinkbig
and NiFI are set to start after all of the services managed by Ambari
start up.

For starting and stopping the 3 data lake accelerator services there you
can run the following scripts.

    /opt/thinkbig/start-thinkbig-apps.sh

    /opt/thinkbig/stop-thinkbig-apps.sh

To Start Individual Services
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1.  $ service activemq start

2.  $ service elasticsearch start

3.  $ service nifi start

4.  $ service thinkbig-spark-shell start

5.  $ service thinkbig-services start

6.  $ service thinkbig-ui start  

7.  To Stop individual services $ service activemq stop

8.  $ service elasticsearch stop

9.  $ service nifi stop

10. $ service thinkbig-spark-shell stop

11. $ service thinkbig-services stop

12. $ service thinkbig-ui stop  

13. To get the status of individual services $ service activemq status

14. $ service elasticsearch status

15. $ service nifi status

16. $ service thinkbig-spark-shell status

17. $ service thinkbig-services status

18. $ service thinkbig-ui status  

Log Output
==========

Configuring Log Output
----------------------

Log output for the services mentioned above are configured at:

    /opt/thinkbig/thinkbig-ui/conf/log4j.properties

    /opt/thinkbig/thinkbig-services/conf/log4j.properties

You may place logs where desired according to the
'log4j.appender.file.File' property. Note the configuration line:

    log4j.appender.file.File=/var/log/<app>/<app>.log

Viewing Log Output
------------------

The default log locations for the various applications are located at:

    /var/log/<service\_name>

Web and REST Access
===================

Below are the default URL’s and ports for the services:

Feed Manager and Operations UI

http://127.0.0.1:8400

username: dladmin

password: thinkbig

NiFi UI

http://127.0.0.1:8079/nifi

Elasticsearch REST API

http://127.0.0.1:9200

ActiveMQ Admin

http://127.0.0.1:8161/admin

Appendix: Cleanup scripts
=========================

For development and sandbox environments you can leverage the cleanup
script to remove all of the Think Big services as well as Elasticsearch,
ActiveMQ, and NiFi.

.. code-block:: shell

    $ /opt/thinkbig/setup/dev/cleanup-env.sh

IMPORTANT Only run this in a DEV environment. This will delete all
application and the MySQL schema

In addition there is a script for cleaning up the hive schema and HDFS
folders that are related to a specific "category" that is defined in the
UI.

.. code-block:: shell

    $ /opt/thinkbig/setup/dev/cleanupCategory.sh [categoryName]

Example: /opt/thinkbig/setup/dev/cleanupCategory.sh customers

Appendix: Postgres Integration
==============================

`Postgres Hive Metadata
Configuration <https://github.com/ThinkBigAnalytics/data-lake-accelerator/blob/master/docs/latest/postgres/postgres-hive-metadata-configuration.adoc>`__

Icons and Icon Colors
=====================

Icons and the colors can be configured using 2 JSON files found in the
/opt/thinkbig/thinkbig-services/conf directory

**icons.json** This is an array of valid icon names. Valid names that
can be used can be found here:
https://klarsys.github.io/angular-material-icons/. Kylo is currently
using the 0.7.1 version of this icon package.

**icon-colors.json** This is an array of objects indicating the display
name and respective Hex color code.

.. |image0| image:: media/common/thinkbig-logo.png
   :width: 3.04822in
   :height: 2.00392in
