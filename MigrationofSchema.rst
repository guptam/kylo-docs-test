|image0|

=========================================================
Migration of Schema from thinkbig → kylo (MySQL Database)
=========================================================

Purpose
=======

Kylo versions 0.6 and below use the \ **thinkbig** schema in MySQL.
Starting from version 0.7, Kylo uses the \ **kylo** schema.

This guide is intended for installations that are already on Kylo 0.6,
and want to upgrade to Kylo 0.7. It outlines the procedure for migrating
the current \ **thinkbig** schema to \ **kylo** schema, so that Kylo 0.7
can work.

Migration Procedure
===================

1. Uninstall Kylo 0.6 (Refer to deployment guide and release notes for
   details).

2. Install Kylo 0.7 (Refer to deployment guide and release notes for
   details).

    Do \ **not** yet start Kylo services.

3. Log into MySQL instance used by Kylo, and list the schemas:

    mysql> show databases

4. Verify that:

-  **thinkbig** schema exists.

-  **kylo** schema does not exist.

5. Navigate to Kylo’s setup directory for MySQL.

    cd /opt/thinkbig/setup/sql/mysql

6. Execute the migration script. It takes 3 parameters. For no password,
   provide the 3rd parameter as ''.

    ./migrate-schema-thinkbig-to-kylo-mysql.sh <host> <user> <password>

-  Step 1 of migration: \ **kylo** schema is set up.

-  Step 2 of migration: \ **thinkbig** schema is migrated
   to \ **kylo** schema.

7. Start Kylo services. Verify that Kylo starts and runs successfully.
   At this point, there are two schemas in MySQL: kylo and thinkbig.

    Once Kylo is running normally and migration is verified,
    the \ **thinkbig** schema can be dropped.

8. Navigate to Kylo’s setup directory for MySQL.

    cd /opt/thinkbig/setup/sql/mysql

9. Execute the script to drop thinkbig schema. It takes 3 parameters.
   For no password, provide the 3rd parameter as ''.

    ./drop-schema-thinkbig-mysql.sh <host> <user> <password>

10. Verify that only kylo schema now exists in MySQL.

     mysql> show databases

This completes the migration procedure.

.. |image0| image:: media/common/thinkbig-logo.png
   :width: 3.09891in
   :height: 2.03724in
