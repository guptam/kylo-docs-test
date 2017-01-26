
=========================================
Kylo Configuration for a Kerberos Cluster
=========================================

The Kylo applications contain features that leverage the thrift server
connection to communicate with the cluster. In order for them to work in
a Kerberos cluster, some configuration is required. Some examples are:

-  Profiling Statistics

-  Tables page

-  Wrangler

Prerequisites
=============

Below are the list of prerequisites to enable Kerberos for Kylo data
lake platform.

1. Running Hadoop cluster

2. Kerberos should be enabled

3. Running Kylo 0.4.0 or higher

Configuration Steps
===================

1. Create a Headless Keytab File for the Hive and Think Big User.

+-------------+--------------------------------------------------------------------------------------+
| **Note:**   | Perform the following as root. Replace "sandbox.hortonworks.com" with your domain.   |
+-------------+--------------------------------------------------------------------------------------+

    [root]# kadmin.local

    kadmin.local: addprinc -randkey "thinkbig@sandbox.hortonworks.com"

    kadmin.local: xst -norandkey -k
    /etc/security/keytabs/thinkbig.headless.keytab
    thinkbig@sandbox.hortonworks.com

    kadmin.local: xst -norandkey -k
    /etc/security/keytabs/hive-thinkbig.headless.keytab
    hive/sandbox.hortonworks.com@sandbox.hortonworks.com

    kadmin.local: exit

    [root]# chown thinkbig:hadoop
    /etc/security/keytabs/thinkbig.headless.keytab

    [root]# chmod 440 /etc/security/keytabs/thinkbig.headless.keytab

    [root]# chown thinkbig:hadoop
    /etc/security/keytabs/hive-thinkbig.headless.keytab

    [root]# chmod 440
    /etc/security/keytabs/hive-thinkbig.headless.keytab

1. Validate that the Keytabs Work.

    [root]# su – thinkbig

    [root]# kinit -kt /etc/security/keytabs/thinkbig.headless.keytab
    thinkbig

    [root]# klist

    [root]# su – hive

    [root]# kinit -kt
    /etc/security/keytabs/hive-thinkbig.headless.keytab
    hive/sandbox.hortonworks.com

    [root]# klist

1. Modify the thinkbig-spark-shell configuration.

    [root]# vi /opt/thinkbig/thinkbig-services/conf/spark.properties

    kerberos.thinkbig.kerberosEnabled=true

    [root]# vi
    /opt/thinkbig/thinkbig-services/bin/run-thinkbig-spark-shell.sh

    #!/bin/bash

    spark-submit --principal 'thinkbig@sandbox.hortonworks.com' --keytab
    /etc/security/keytabs/thinkbig.headless.keytab ...

1. Modify the thinkbig-services configuration.

+------------+-------------------------------------------------------+
| **TIP**:   | Replace "sandbox.hortonworks.com" with your domain.   |
+------------+-------------------------------------------------------+

    To add Kerberos support to thinkbig-services you must enable the
    feature and update hive connection URL to support Kerberos.

    [root]# vi
    /opt/thinkbig/thinkbig-services/conf/application.properties

    # This property is for the hive thrift connection used by
    thinkbig-services

    hive.datasource.url=jdbc:hive2://localhost:10000/default;principal=hive/sandbox.hortonworks.com@sandbox.hortonworks.com

    # This property will default the URL when importing a template using
    the thrift connection

    nifi.service.hive\_thrift\_service.database\_connection\_url=jdbc:hive2://localhost:10000/default;principal=hive/sandbox.hortonworks.com@sandbox.hortonworks.com

    # Set Kerberos to true for the thinkbig-services application and set
    the 3 required properties

    kerberos.hive.kerberosEnabled=true

    kerberos.hive.hadoopConfigurationResources=/etc/hadoop/conf/core-site.xml,/etc/hadoop/conf/hdfs-site.xml

    kerberos.hive.kerberosPrincipal=hive/sandbox.hortonworks.com

    kerberos.hive.keytabLocation=/etc/security/keytabs/hive-thinkbig.headless.keytab

    # uncomment these 3 properties to default all NiFi processors that
    have these fields. Saves time when importing a template

    nifi.all\_processors.kerberos\_principal=nifi

    nifi.all\_processors.kerberos\_keytab=/etc/security/keytabs/nifi.headless.keytab

    nifi.all\_processors.hadoop\_configuration\_resources=/etc/hadoop/conf/core-site.xml,/etc/hadoop/conf/hdfs-site.xml

1. Restart the thinkbig-services and thinkbig-spark-shell.

[root]# service thinkbig-services restart

    [root]# service thinkbig-spark-shell restart

Kylo is now configured for a Kerberos cluster. You can test that it is
configured correctly by looking at profile statistics (if applicable):
go to the Tables page and drill down into a hive table, and go to the
Wrangler feature and test that it works.

.. |image0| image:: media/common/thinkbig-logo.png
   :width: 3.09891in
   :height: 2.03724in
