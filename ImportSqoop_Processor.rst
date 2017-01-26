
======================
Import Sqoop Processor
======================

About
=====

The ImportSqoop processor allows loading data from a relational system
into HDFS. This document discusses the setup required to use this
processor.

Starter Template
================

A starter template for using the processor is provided at:

.. code-block:: html

    *samples/templates/nifi-1.0/template-starter-sqoop-import.xml*

Configuration update
====================

For use with Kylo UI, configure values in the section starting with the
below header in the deployment
location: \ */opt/thinkbig/thinkbig-services/conf/application.properties*

    *### Sqoop import*

    *# DB Connection password*

    *# config.sqoop.hdfs.ingest.root*

Drivers
=======

Sqoop requires the JDBC drivers for the specific database server in
order to transfer data. The processor has been tested on MySQL, Oracle
and Teradata databases, using Sqoop v1.4.6.

The drivers need to be downloaded, and the .jar files must be copied
over to Sqoop’s /lib directory.

As an example, consider that the MySQL driver is downloaded and
available in a file named: \ **mysql-connector-java.jar.**

This would have to be copied over into Sqoop’s /lib directory, which may
be in a location similar to: \ **/usr/hdp/current/sqoop-client/lib.**

The driver class must then be referred to in the property \ ***Source.
Driver*** in **StandardSqoopConnectionService** controller service
configuration. For example: \ **com.mysql.jdbc.Driver.**

Passwords
=========

The processor allows passwords used for connecting to source system to
be encrypted and stored on HDFS.

Encrypt the password by providing the:

1. password to encrypt

2. passphrase

3. location to write encrypted file to

Say, the file containing encrypted password is
named: \ **/user/home/sec-pwd.enc.**

Put this file in HDFS and secure it by restricting permissions to be
only read by \ **nifi** user.

Provide the file location and passphrase via the \ ***Source Password
File*** and ***Source Password Passphrase*** properties in
the \ **StandardSqoopConnectionService** controller service
configuration.

During the processor execution, it will decrypt the password and use it
to connect to the source system.

Generation of the encrypted password, and putting it into HDFS, is a
pre-setup task. The following command can be used to generate the
encrypted password:

**/opt/thinkbig/bin/encryptSqoopPassword.sh**



.. |image0| image:: media/common/thinkbig-logo.png
   :width: 3.09375in
   :height: 2.03385in
