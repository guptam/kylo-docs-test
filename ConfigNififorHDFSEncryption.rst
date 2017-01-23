|image0|

Configure Nifi for HDFS Encryption

Key Creation Process
====================

1. Log in to Ranger KMS UI.

    <Hostname>:6080

    |image1|

2. Provide **Username** as 'keyadmin' and password for user.

3. Go to the Encryption tab and click **Key Manager**.

   |image2|

4. Select the appropriate defined service from list.

   |image3|

5. Click **Add New Key**.

6. Fill out the Key Detail fields.

   |image4|

7. Click **Save**.

Now Key has been successfull got created which can be used for creating
encryption zone.

Permission Definition
=====================

Now next task is provide necessary permission to user who will run nifi
application. In our case we are using nifi user for running application
and hdfs as super user operation.

1. Click on **Service**.

   |image5|

2. Click on the edit icon present at right side.

   |image6|

3. Go to bottom of page , you will see User and Group Permissions tab.

   |image7|

4. Provide appropriate permissions to the NiFi user.

Configure CreateHDFSFolder Processor
====================================

1. Right-click **Processor** and select **Configure**.

2. Configure the highlighted property for the processor.

    Directory To Be Encrypted : /model.db/${source}

    /app/warehouse/${source}

    /etl/${source}

    /archive/${source}

    Encryption Key : nifikey

    Encryption Required : Y

    |image8|

3. Click **OK** and start the processor.

You have successfully configured Nifi DataLake Platform for HDFS
Encryption.

.. |image0| image:: media/common/thinkbig-logo.png
   :width: 3.09375in
   :height: 2.03385in
.. |image1| image:: media/Config_NiFi/E1.png
   :width: 2.86302in
   :height: 2.48958in
.. |image2| image:: media/Config_NiFi/E2.png
   :width: 6.13542in
   :height: 2.09430in
.. |image3| image:: media/Config_NiFi/E3.png
   :width: 6.13542in
   :height: 1.94223in
.. |image4| image:: media/Config_NiFi/E4.png
   :width: 5.91667in
   :height: 4.48238in
.. |image5| image:: media/Config_NiFi/E5.png
   :width: 6.01042in
   :height: 2.64368in
.. |image6| image:: media/Config_NiFi/E5.5.png
   :width: 5.98958in
   :height: 1.52811in
.. |image7| image:: media/Config_NiFi/E6.png
   :width: 5.97917in
   :height: 2.44788in
.. |image8| image:: media/Config_NiFi/E7.png
   :width: 5.98958in
   :height: 2.76314in
