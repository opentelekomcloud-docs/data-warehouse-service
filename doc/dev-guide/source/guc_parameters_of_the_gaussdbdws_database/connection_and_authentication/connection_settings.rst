:original_name: dws_04_0889.html

.. _dws_04_0889:

Connection Settings
===================

This section describes parameters related to the connection mode between the client and server.

.. _en-us_topic_0000001764491936__s2d671f584b5647a19255e7c6a3d116aa:

max_connections
---------------

**Parameter description**: Specifies the maximum number of allowed parallel connections to the database. This parameter influences the concurrent processing capability of the cluster.

**Type**: POSTMASTER

**Value range**: an integer. For CNs, the value ranges from 100 to 16384. For DNs, the value ranges from 100 to 262143. Because there are internal connections in the cluster, the maximum value is rarely reached. If **invalid value for parameter "max_connections"** is displayed in the log, you need to decrease the **max_connections** value for DNs.

**Default value**: **800** for CNs and **5000** for DNs. If the default value is greater than the maximum value supported by kernel (determined when the **gs_initdb** command is executed), an error message will be displayed.

**Setting suggestions**:

Retain the default value of this parameter on CNs. On a DN, the value of this parameter is calculated as follows:

dop_limit x 20 x 6 + 24: **dop_limit** indicates the number of CPUs of each DN in the cluster. It is calculated as follows: **dop_limit** = Number of logical CPU cores of a single server/Number of DNs of a single server.

The minimum value is 5000.

If the parameter is set to a large value, GaussDB(DWS) requires more SystemV shared memories or semaphores, which may exceed the maximum default configuration of the OS. In this case, modify the value as needed.

.. important::

   The value of **max_connections** is related to :ref:`max_prepared_transactions <en-us_topic_0000001811610373__s7f44489cfdce4bbea287150fb7333b9e>`. Before setting **max_connections**, ensure that the value of **max_prepared_transactions** is greater than or equal to that of **max_connections**. In this way, each session has a prepared transaction in the waiting state.

application_name
----------------

**Parameter description**: Specifies the name of the client program connecting to the database.

**Type**: USERSET

**Value range**: a string

**Default value**: **gsql**

.. _en-us_topic_0000001764491936__section4834457114318:

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

         {"driver_name":"ODBC","driver_version": "(GaussDB x.x.x build 39137c2d) compiled at 2022-09-23 15:43:11 commit 3629 last mr 5138 debug","driver_path":"/usr/local/lib/psqlodbcw.so","os_user":"omm"}

      By default, **driver_name**, **driver_version**, **driver_path**, and **os_user** are displayed for ODBC, JDBC, and gsql connections. For other connections, **driver_name** and **driver_version** are displayed by default. The display of **driver_path** and **os_user** is controlled by users (see :ref:`Connecting to a Database <dws_04_0093>` and :ref:`Configuring a Data Source in the Linux OS <dws_04_0119>`).
