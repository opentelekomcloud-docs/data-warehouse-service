:original_name: dws_04_0884.html

.. _dws_04_0884:

Viewing GUC Parameters
======================

GaussDB(DWS) GUC parameters can control database system behaviors. You can check and adjust the GUC parameters based on your business scenario and data volume.

-  After a cluster is installed, you can check database parameters on the GaussDB(DWS) management console.

   |image1|

-  You can also connect to a cluster and run SQL commands to check the GUC parameters.

   -  Run the **SHOW** command.

      .. note::

         Method 2 can only be used to check the GUC parameter values of CNs, while the GUC parameter values of DNs can be viewed through Method 1: by using the management console.

      To view a certain parameter, run the following command:

      ::

         SHOW server_version;

      *server_version* indicates the database version.

      Run the following command to view values of all parameters:

      ::

         SHOW ALL;

   -  Use the **pg_settings** view.

      To view a certain parameter, run the following command:

      ::

         SELECT * FROM pg_settings WHERE NAME='server_version';

      Run the following command to view values of all parameters:

      ::

         SELECT * FROM pg_settings;

.. |image1| image:: /_static/images/en-us_image_0000001811610577.png
