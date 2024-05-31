:original_name: dws_04_0192.html

.. _dws_04_0192:

Preparing Source Data
=====================

Scenario
--------

Generally, the data to be imported has been uploaded to the data server. In this case, you only need to check the communication between the data server and GaussDB(DWS), and record the data storage directory on the data server before the import.

If the data has not been uploaded to the data server, perform the following operations to upload it:

Procedure
---------

#. Log in to the data server as user **root**.

#. Create the directory **/input_data**.

   .. code-block::

      mkdir -p /input_data

#. Upload the source data files to the created directory.

   GDS parallel import supports source data only in CSV or TEXT format.
