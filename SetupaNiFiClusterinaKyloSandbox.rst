
======================================
Setup A NiFi Cluster in a Kylo Sandbox
======================================

Purpose
=======

This document describes the procedure for setting up a NiFi cluster in
the Kylo sandbox.

Prerequisite
============

You will need to have installed NiFi with the Kylo deployment, set up a Kylo sandbox, and have Java 8 installed on a node.

Setup a NiFi Cluster
====================

The first step is to either rename an existing instance of NiFi, or to
install a new instance either manually or by using the Kylo Install
Wizard. You will then copy, configure, and start the new NiFi cluster.

1. Rename an existing /opt/nifi:

..code-block:: shell

    Example:

    /opt/nifi-temp

..

    Or, install a new instance of NiFi:

    -  By using the Kylo Install wizard.

    -  Manually install NiFi and then perform the post-installation Kylo setup steps. (See the Kylo Manual Deployment Guide.) 

2. Rename the newly installed /opt/nifi:

..code-block:: shell

    Example:

    /opt/nifi-2

..

3. Make a copy of the new instance by doing one of the following:

   -  Rename /opt/nifi-temp (from the example in Step 1), back to
          /opt/nifi.

   -  Or, copy /etc/init.d/nifi to /etc/init.d/nifi-2

4. Edit /etc/init.d/nifi-2 by changing the NiFi path (at the top) to:

..code-block:: shell

    /opt/nifi-2

..

5. Edit /opt/nifi-2/current/bin/nifi-env.sh by changing the log
   directory to:

..code-block:: shell

    /var/log/nifi-2

..

6. Make the directory:

..code-block:: shell

    /var/log/nifi-2

    chown nifi:nifi /var/log/nifi-2

..

7. Edit /opt/nifi-2/current/conf/nifi.properties.

-  Replace all references to /opt/nifi with /opt/nifi-2  

    (in vi:  :1,$s#/opt/nifi#/opt/nifi-2#g )

-  Update the cluster-wide settings (all NiFi installations will have
   the same values).

..code-block:: shell

    nifi.cluster.is.node=true
    nifi.clusdter.node.address=localhost
    nifi.zookeeper.connect.string=localhost:2181

..

8. Update the instance specific settings (port numbers unique to this
   instance).

..code-block:: shell

    nifi.web.http.port=8077

    nifi.cluster.node.protocol.port=8076

..

+---------+------------------------------------------------------------------+
| NOTE:   | This is only necessary when all instances are on the same node   |
+---------+------------------------------------------------------------------+

9. Copy/overwrite  /opt/nifi/data/conf/flow.xml.gz to:

..code-block:: shell

    /opt/nifi-2/conf/flow

..

+-------------+--------------------------------------------------------------------------------------------+
| **NOTE:**   | Make sure to update the origin NiFi configuration with the equivalent properties above.    |
+-------------+--------------------------------------------------------------------------------------------+

 

Start Each NiFi
===============

Now that your instance is created and configured, start the services:

..code-block:: shell

    $ service nifi start
    $ service nifi-2 start  (if you created it in /etc/init.d)

..

Don’t forget to open up the nifi.web.http.port property's port number in
your VM.

You should be able to open the NiFi UI under
either \ `*http://localhost:8079* <http://localhost:8079/>`__ or `*http://localhost:8077* <http://localhost:8077/>`__ (or
whatever port you used on your second instance above) and see in the
upper left a cluster icon and 2/2.
