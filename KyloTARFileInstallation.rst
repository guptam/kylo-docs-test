
==========================
Kylo TAR File Installation
==========================

Introduction
============

At this time an RPM file is the only artifact built in Kylo. An RPM
installation is meant to be an opioninated way of installing an
application and reduces the number of steps required during
installation. However, some clients have strict requirements as to where
they need install Kylo and what user to run Kylo under. These
instructions will guide you on an alternative way to install Kylo if
required.

Determine Service Account and Kylo Install Location
---------------------------------------------------

Let’s assume, for this example, that Kylo will run under an account name
"thinkbig\_user", and it will be installed in /opt/apps/.

Step 1: Install the RPM and copy the /opt/thinkbig folder to a temporary location
---------------------------------------------------------------------------------

    cp -R /opt/thinkbig /opt/tb-test

Step 2: Copy init.d scripts to the same temporary location
----------------------------------------------------------

    cp /etc/init.d/thinkbig-services /opt/tb-test
    cp /etc/init.d/thinkbig-spark-shell /opt/tb-test
    cp /etc/init.d/thinkbig-ui /opt/tb-test

Your temp location should look like this

    [root@sandbox tb-test]# ls -l /opt/tb-test
    drwxr-xr-x 8 root root 4096 2016-10-27 20:13 thinkbig
    -rwxr-xr-x 1 root root 1561 2016-10-27 20:20 thinkbig-services
    -rwxr-xr-x 1 root root 1281 2016-10-27 20:21 thinkbig-spark-shell
    -rwxr-xr-x 1 root root 1447 2016-10-27 20:21 thinkbig-ui

Step 3 (Optional): Tar up the folder and copy it to the edge node if you aren’t already on it
---------------------------------------------------------------------------------------------

Step 4: Install the files
-------------------------

1. Copy the thinkbig folder to /opt/apps

2. Copy the 3 init.d scripts to /etc/init.d

Step 5: Create Log Folders
--------------------------

    [root@sandbox tb-test]# mkdir /var/log/thinkbig-services
    [root@sandbox tb-test]# chown thinkbig\_user:thinkbig\_user
    /var/log/thinkbig-services
    [root@sandbox tb-test]# mkdir /var/log/thinkbig-ui
    [root@sandbox tb-test]# chown thinkbig\_user:thinkbig\_user
    /var/log/thinkbig-ui
    [root@sandbox tb-test]# mkdir /var/log/thinkbig-spark-shell
    [root@sandbox tb-test]# chown thinkbig\_user:thinkbig\_user
    /var/log/thinkbig-spark-shell/

Step 6: Modify the user in the init.d scripts
---------------------------------------------

Set this line to be the correct user.

    RUN\_AS\_USER=thinkbig\_user

Also change any reference of /opt/thinkbig to /opt/apps/thinkbig.

Step 7: Modify the bin scripts for the 3 thinkbig apps
------------------------------------------------------

Modify the following files and change /opt/thinkbig references to
/opt/thinkbig/apps

    /opt/apps/thinkbig/thinkbig-ui/bin/run-thinkbig-ui.sh
    /opt/apps/thinkbig/thinkbig-services/bin/run-thinkbig-services.sh
    /opt/apps/thinkbig/thinkbig-spark-shell/bin/run-thinkbig-spark-shell.sh

Step 8: Start up Kylo and test
------------------------------

.. |image0| image:: media/common/thinkbig-logo.png
   :width: 3.03125in
   :height: 1.99277in
