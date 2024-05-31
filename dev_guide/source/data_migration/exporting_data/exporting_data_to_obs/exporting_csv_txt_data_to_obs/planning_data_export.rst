:original_name: dws_04_0252.html

.. _dws_04_0252:

Planning Data Export
====================

Scenarios
---------

Plan the storage location of exported data in OBS.

Planning OBS Save Path and File
-------------------------------

You need to specify the OBS path (to directory) for storing data that you want to export. The exported data can be saved to a file in CSV format. The system also supports TEXT so that you can import the exported data to various applications.

The target directory cannot contain any files.

Planning OBS Bucket Permissions
-------------------------------

The user used to export data must:

-  Have OBS enabled.

-  Have the write permission on the OBS bucket where the data export path is located.

   You can configure ACL permissions for the OBS bucket to grant the write permission to a specific user.

   For details, see :ref:`Granting Write Permission to OBS Storage Location and OBS Bucket as Planned <en-us_topic_0000001188642118__en-us_topic_0000001098811002_en-us_topic_0117407697_s7fcbf9744bc440899b56c22be4fae4f3>`.

Planning Data to Be Exported and Foreign Tables
-----------------------------------------------

You must prepare data to be exported in the database table, and the data volume per row must be less than 1 GB. Based on the data to be exported, plan foreign tables whose attributes such as columns, column types, and length match those of user data.

.. _en-us_topic_0000001188642118__en-us_topic_0000001098811002_en-us_topic_0117407697_s7fcbf9744bc440899b56c22be4fae4f3:

Granting Write Permission to OBS Storage Location and OBS Bucket as Planned
---------------------------------------------------------------------------

#. Create an OBS bucket and a folder in the OBS bucket as the directory for storing exported data.

   a. Log in to the OBS management console.

      Click **Service List** and choose **Object Storage Service** to open the OBS management console.

   b. Create a bucket.

      For details about how to create an OBS bucket, see "OBS Console Operation Guide > Managing Buckets > Creating a Bucket" in the *Object Storage Service User Guide*.

      For example, create two buckets named **mybucket** and **mybucket02**.

   c. Create a folder.

      For details about how to create an OBS bucket, see "OBS Console Operation Guide > Managing Objects > Creating a Folder" in the *Object Storage Service User Guide*.

      Example:

      -  Create a folder named **output_data** in the **mybucket** OBS bucket.
      -  Create a folder named **output_data** in the **mybucket02** OBS bucket.

#. Determine the path of the created OBS folder.

   Specify the OBS path for storing exported data files. This path is the value of the **location** parameter used for creating a foreign table.

   The OBS folder path in the **location** parameter consists of **obs://**, a bucket name, and a file path. Example:

   In this example, the OBS folder path is as follows:

   ::

      obs://mybucket/output_data/

   .. note::

      The OBS directory to be used for storing data files must be empty.

#. Grant the OBS bucket write permission to the user who wants to export data.

   When exporting data, a user must have the write permission on the OBS bucket where the data export path is located. You can configure ACL permissions for the OBS bucket to grant the write permission to a specific user.

   For details, see "OBS Console Operation Guide > Permission Control > Configuring a Bucket ACL" in the *Object Storage Service User Guide*.
