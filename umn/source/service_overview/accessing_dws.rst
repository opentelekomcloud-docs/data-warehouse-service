:original_name: dws_01_0009.html

.. _dws_01_0009:

Accessing DWS
=============

The following figure shows how to use DWS.


.. figure:: /_static/images/en-us_image_0000002270374529.png
   :alt: **Figure 1** Process for using DWS

   **Figure 1** Process for using DWS

Accessing a Cluster
-------------------

DWS provides a web-based management console and HTTPS-compliant APIs for you to manage DWS clusters.

.. note::

   In cluster deployment, if a single node is faulty, the abnormal node is automatically skipped when DWS is accessed. However, the cluster performance will be affected.

Accessing the Database in a Cluster
-----------------------------------

DWS supports database access using the following methods:

-  DWS client

   Use the DWS client to access the database in the cluster. For details, see "Cluster Connection" in the *Data Warehouse Service (DWS) User Guide*.

-  JDBC and ODBC API calling

   You can call standard APIs, such as JDBC and ODBC, to access databases in DWS clusters.

   For details, see "Using the JDBC and ODBC Drivers to Connect to a Cluster" in the *Data Warehouse Service (DWS) User Guide*.

-  psycopg2 and PyGreSQL drivers

   After creating a data warehouse cluster, you can use the third-party function library psycopg2 or PyGreSQL to connect to the cluster, and use Python to access DWS and perform various operations on data tables. For details, see "Using the Third-Party Function Library psycopg2 of Python to Connect to a Cluster" and "Using the Python Library PyGreSQL to Connect to a Cluster" in *Data Warehouse Service User Guide*.

   .. note::

      Currently, DWS does not support cross-database access. Schemas can be used to isolate resources.

End-to-End Data Analysis Process
--------------------------------

DWS has been seamlessly integrated with other services on cloud, helping you rapidly deploy end-to-end data analysis solutions.

The following figure shows the end-to-end data analysis process. Services in use during each process are also displayed.


.. figure:: /_static/images/en-us_image_0000002270374533.png
   :alt: **Figure 2** End-to-end data analysis process

   **Figure 2** End-to-end data analysis process
