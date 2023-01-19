:original_name: dws_04_0889.html

.. _dws_04_0889:

Connection Settings
===================

This section describes parameters related to the connection mode between the client and server.

.. _en-us_topic_0000001099134530__s2d671f584b5647a19255e7c6a3d116aa:

max_connections
---------------

**Parameter description**: Specifies the maximum number of allowed parallel connections to the database. This parameter influences the concurrent processing capability of the cluster.

**Type**: POSTMASTER

**Value range**: an integer. For CNs, the ranges from 1 to 16384. For DNs, the value ranges from 1 to 262143. Because there are internal connections in the cluster, the maximum value is rarely reached. If **invalid value for parameter "max_connections"** is displayed in the log, you need to decrease the **max_connections** value for DNs.

**Default value**: **800** for CNs and **5000** for DNs. If the default value is greater than the maximum value supported by kernel (determined when the **gs_initdb** command is executed), an error message will be displayed.

**Setting suggestions**:

Retain the default value of this parameter on the CN. Set this parameter on the DN to the following calculation result: Number of CNs x Value of this parameter on the CN.

If the parameter is set to a large value, GaussDB(DWS) requires more SystemV shared memories or semaphores, which may exceed the maximum default configuration of the OS. In this case, modify the value as needed.

.. important::

   The value of **max_connections** is related to :ref:`max_prepared_transactions <en-us_topic_0000001145694783__s7f44489cfdce4bbea287150fb7333b9e>`. Before setting **max_connections**, ensure that the value of **max_prepared_transactions** is greater than or equal to that of **max_connections**. In this way, each session has a prepared transaction in the waiting state.

sysadmin_reserved_connections
-----------------------------

**Parameter description**: Specifies the minimum number of connections reserved for administrators.

**Type**: POSTMASTER

**Value range**: an integer ranging from 0 to 262143

**Default value**: **3**

application_name
----------------

**Parameter description**: Specifies the name of the client program connecting to the database.

**Type**: USERSET

**Value range**: a string

**Default value**: **gsql**

.. _en-us_topic_0000001099134530__section4834457114318:

connection_info
---------------

**Parameter description**: Specifies the database connection information, including the driver type, driver version, driver deployment path, and process owner. (This is an O&M parameter. Do not configure it by yourself.)

**Type**: USERSET

**Value range**: a string

**Default value**: an empty string

.. note::

   -  An empty string indicates that the driver connected to the database does not support automatic setting of the **connection_info** parameter or the parameter is not set by users in applications.

   -  The following is an example of the concatenated value of **connection_info**:

      ::

         {"driver_name":"ODBC","driver_version": "(GaussDB 8.1.1 build af002019) compiled at 2020-01-10 05:43:20 commit 6995 last mr 11566 debug","driver_path":"/usr/local/lib/psqlodbcw.so","os_user":"dbadmin"}

      **driver_name** and **driver_version** are displayed by default. Whether **driver_path** and **os_user** are displayed is determined by users.
