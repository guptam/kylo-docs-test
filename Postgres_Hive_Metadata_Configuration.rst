
====================================
Postgres Hive Metadata Configuration
====================================

Introduction
============

Kylo currently requires MySQL for the Thinkbig schema. However, you can
configure Kylo to work with a cluster that uses Postgres. We need to
make some modifications to support Hive.

Thinkbig Services Configuration
===============================

For Kylo to connect to a Postgres databases for the hive metadata you
need to change the following section of the thinkbig-services
application.properties file.

    hive.metastore.datasource.driverClassName=org.postgresql.Driver
    hive.metastore.datasource.url=jdbc:postgresql://<hostname>:5432/hive
    hive.metastore.datasource.username=hive
    hive.metastore.datasource.password=
    hive.metastore.datasource.validationQuery=SELECT 1
    hive.metastore.datasource.testOnBorrow=true

Elasticsearch NiFi Template Changes
===================================

The index\_schema\_service template is used to query out feed metadata
from the hive tables, which is then stored in elasticsearch so it can be
searched for in Kylo. The following steps need to be taken to the
template to support Postgres:

Step 1: Copy the Postgres JAR file to NiFi
------------------------------------------

    mkdir /opt/nifi/postgres
    cp /opt/thinkbig/thinkbig-services/lib/postgresql-9.1-901-1.jdbc4.jar
    /opt/nifi/postgres
    chown -R nifi:users /opt/nifi/postgres

Step 2: Create a Controller Service for Postgres Connection
-----------------------------------------------------------

You will need to create an additional database controller services to
connect to the second database.

Contoller Service Properties:

    Controller Service Type: DBCPConnectionPool
    Database Connection URL: jdbc:postgresql://<host>:5432/hive
    Database Driver Class Name: org.postgresql.Driver
    Database Driver Jar URL:
    file:///opt/nifi/postgres/postgresql-9.1-901-1.jdbc4.jar Database
    User: hive
    Password: <password>

Enable the Controller Service.

Step 3: Update “Query Hive Table Metadata” Processor
----------------------------------------------------

Edit the “Query Hive Table Schema” processor and make two changes:

1. Disable the “Query Hive Table Metadata” processor.

2. Change the Database Connection Pooling Service to the Postges Hive
   controller service created above.

3. Update the “SQL select Query” to be a Postgres query.

    SELECT d."NAME", d."OWNER\_NAME", t."CREATE\_TIME", t."TBL\_NAME",
    t."TBL\_TYPE",
      c."COLUMN\_NAME", c."TYPE\_NAME"
      FROM "COLUMNS\_V2" c
      JOIN "TBLS" t ON c."CD\_ID"=t."TBL\_ID"
      JOIN "DBS" d on d."DB\_ID" = t."DB\_ID"
      where d."NAME" = '${category}'and t."TBL\_NAME" like '${feed}';

4. Enable the “Query Hive Table Metadata” processor.

5. Test a feed to make sure the data is getting indexed.



.. |image0| image:: media/common/thinkbig-logo.png
   :width: 3.09375in
   :height: 2.03385in
