|image0|

===============================
Developer Getting Started Guide
===============================

This guide should help you get your local development environment up and
running quickly. Development in an IDE is usually done in conjunction
with a Hortonworks sandbox in order to have a cluster to communicate
with.

Dependencies
------------

To run the Kylo project locally the following tools must be installed:

-  Maven 3

-  RPM (for install)

-  Java 1.8 (or greater)

-  Hadoop 2.3+ Sandbox

-  Virtual Box or other virtual machine manager

The assumption is that you are installing on a Mac or Linux box. You can
do most activities below on a Windows box, except to perform a maven
build with the RPM install. At some point, we could add a maven profile
to allow you to build but skip the final RPM step.

Install Maven 3
---------------

This project requires Maven to execute a build. Use this link to
download to the Maven installation file:

    `*https://maven.apache.org/install.html* <https://maven.apache.org/install.html>`__

Optional - Add Java 8 to Bash Profile
-------------------------------------

To build from the command line, you need to add Java 8 and Maven to your
$PATH variable.

Edit ~/.bashrc and add the following:

    export MVN\_HOME=/Users/<HomeFolderName>/tools/apache-maven-3.3.3

    export MAVEN\_OPTS="-Xms256m -Xmx512m"

    export
    JAVA\_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0\_45.jdk/Contents/Home

    export PATH=$JAVA\_HOME/bin:$MVN\_HOME/bin:$PATH

To test, run the following:

.. code-block:: shell

    $ mvn -v

    $ java -version

Install Virtual Box
-------------------

Use this link to download and install the DMG file to install Virtual
Box:

    `*https://www.virtualbox.org/wiki/Downloads* <https://www.virtualbox.org/wiki/Downloads>`__

Install the RPM Tool on your Mac
--------------------------------

The RPM library is required for building the RPM file as part of the
maven build. This can be done using Home Brew or Mac Ports.

.. code-block:: shell

    $ brew install rpm

Clone Project from Github
-------------------------

Clone the Kylo project to your host. You can do this in your IDE or from
the command line.

1. From the command line,run the "git clone" command.

   a. cd to the directory you want to install the project to.

   b. Type "git
      clone \ `*https://github.com/ThinkBigAnalytics/data-lake-accelerator.git"* <https://github.com/ThinkBigAnalytics/data-lake-accelerator.git>`__.

2. Import from your IDE using the
   "`*https://github.com/ThinkBigAnalytics/data-lake-accelerator.git* <https://github.com/ThinkBigAnalytics/data-lake-accelerator.git>`__"
   URL.

Import the Project into your IDE
--------------------------------

Import the project into your favorite IDE as a maven project.

+------------+----------------------------------------+
| **Note**   | Configure the project to use Java 8.   |
+------------+----------------------------------------+

Perform a Maven Build
---------------------

Perform a Maven build to download all of the artifacts and verify that
everything is setup correctly.

.. code-block:: shell

    $ mvn clean install

**TIP:** For faster maven builds you can run in offline mode by typing:

    "mvn clean install -o".

Add "-DskipTests" to skip unit testing for faster builds.

Install and Configure the Hortonworks Sandbox
---------------------------------------------

Follow the guide below to install and configure the Hortonworks sandbox:

    `*Configure Hortonworks
    Sandbox* <https://github.com/ThinkBigAnalytics/data-lake-accelerator/blob/master/docs/latest/hortonworks-sandbox.adoc>`__

Install the Think Big Applications
----------------------------------

To install the Think Big apps, NiFi, ActiveMQ, and Elasticsearch in the
VM you can use the deployment wizard instructions found here.

    `*Wizard Driven Deployment
    Guide* <https://github.com/ThinkBigAnalytics/data-lake-accelerator/blob/master/docs/latest/deployment/wizard-deployment-guide.adoc>`__

Instead of downloading the RPM file on the first step from Artifactory,
copy the RPM file from your project folder after running a maven build.

.. code-block:: shell

    $ cd /opt

    $ cp
    /media/sf\_data-lake-accelerator/install/target/rpm/thinkbig-datalake-accelerator/RPMS/noarch/thinkbig-datalake-accelerator-<version>.noarch.rpm
    .

    $ rpm -ivh thinkbig-datalake-accelerator-<version>.noarch.rpm

Follow the rest of the deployment wizard steps to install the rest of
the tools in the VM.

+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **Important!**   | You only need to install Elasticsearch, NiFi, and ActiveMQ once. During development you will frequently uninstall the Think Big RPM and re-install it for testing.   |
+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+

You now have a distribution of the stack running in your Hortonworks
sandbox.

Running in the IDE
------------------

You can run thinkbig-ui and thinbig-services in the IDE. If you plan to
run the apps in the IDE, you should shut down the services in your
sandbox so you aren’t running two instances at the same time.

.. code-block:: shell

    $ service thinkbig-services stop

    $ service thinkbig-ui stop

The applications are configured using Spring Boot.

IntelliJ Configuration
----------------------

1. Install the Spring Boot plugin.

2. Create the thinkbig-services application run configuration.

   a. Open the Run configurations.

   b. Create a new Spring Boot run configuration.

   c. Give it a name like "ThinkbigServerApplication".

   d. Set "use classpath of module" property to "thinkbig-service-app"
      module.

   e. Set the "Main Class" property to
      "com.thinkbiganalytics.server.ThinkbigServerApplication".

3. Create the thinkbig-ui application run configuration.

   a. Open the Run configurations.

   b. Create a new Spring Boot run configuration.

   c. Give it a name like "ThinkbigDataLakeUiApplication".

   d. Set "use classpath of module" property to "thinkbig-ui-app"
      module.

   e. Set the "Main Class" property to
      "com.thinkbiganalytics.ThinkbigDataLakeUiApplication".

4. Run both applications.

Eclipse Configuration
---------------------

`*http://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-running-your-application.html* <http://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-running-your-application.html>`__

1. Open Eclipse

2. Import the Data Lake Accelerator project

   a. File - Import

   b. Choose "maven" and "Existing Maven Projects" then choose next

   c. Choose the Data Lake Accelerator root folder. You should see all
      maven modules checked

   d. Click finish

   e. Import takes a bit - got error about scala plugin - just clicked
      finish

3. Find and open the
   "com.thinkbiganalytics.server.ThinkbigServerApplication" class

4. Right click and choose to debug as a Java application

5. Repeat for "com.thinkbiganalytics.ThinkbigDataLakeUiApplication"

    OPTIONAL: Install the spring tools suite and run as a spring boot
    option

.. |image0| image:: media/common/thinkbig-logo.png
   :width: 3.09891in
   :height: 2.03724in
