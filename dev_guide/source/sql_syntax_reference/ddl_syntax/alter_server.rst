:original_name: dws_06_0138.html

.. _dws_06_0138:

ALTER SERVER
============

Function
--------

Adds, modifies, and deletes the definition of a foreign server.

Existing foreign servers can be queried from the **pg_foreign_server** system catalog.

Precautions
-----------

Only the owner of a server or a system administrator can run this statement.

Syntax
------

-  Change the parameters for an external server.

::

   ALTER SERVER server_name [ VERSION 'new_version' ]
       [ OPTIONS ( {[ ADD | SET | DROP ] option ['value']} [, ... ] ) ];

In **OPTIONS**, **ADD**, **SET**, and **DROP** are operations to be executed. If these operations are not specified, **ADD** operations will be performed by default. **option** and **value** are corresponding operation parameters.

Currently, only **SET** is supported on an HDFS server. **ADD** and **DROP** are not supported. The syntax for SET and DROP operations is retained for later use.

-  Change the owner of an external server.

::

   ALTER SERVER server_name
       OWNER TO new_owner;

-  Change the name of an external server.

::

   ALTER SERVER server_name
       RENAME TO new_name;

-  Refresh the HDFS configuration file. Only 8.0.0.10 and later versions (except 8.1.0) support this function.

::

   ALTER SERVER server_name REFRESH OPTIONS;

Parameter Description
---------------------

-  **server_name**

   Specifies the name of the server to be modified.

-  **new_version**

   Indicates the new version of the server.

-  The server parameters in **OPTIONS** are as follows:

   -  address

      Specifies the endpoint of the OBS service.

      Specifies the IP address and port number of the primary and standby nodes of the HDFS cluster.

      .. note::

         -  **address** is mandatory for HDFS servers. Therefore, **ADD** and **DROP** operations are not supported.
         -  **address** only supports IPv4 addresses in dot-decimal notation, and an address string cannot contain spaces. Groups of addresses are separated by commas (,). An IP address and a port number are separated by a colon (:). You are advised to configure two IP address and port pairs in an HDFS cluster. One is used as the socket address of the primary HDFS NameNode and another is used as that of the secondary HDFS NameNode.
         -  If the server type is DLI, the address is the OBS address stored on DLI.

   -  hdfscfgpath

      Specifies the HDFS cluster configuration file.

      .. note::

         -  If HDFS is in security mode, **hdfscfgpath** is mandatory.
         -  If you set **hdfscfgpath**, you can only set one value for **path**.

   -  fed

      Indicates that **dfs_fdw** is connected to HDFS in federation mode.

      The value **rbf** indicates that HDFS uses the Router-based Federation (RBF) mode.

      .. note::

         This parameter is supported in 8.1.2 or later. In 8.0.0, this parameter is supported only in 8.0.0.10 or later.

   -  encrypt

      Specifies whether data is encrypted. This parameter is available only when **type** is **OBS**. The default value is **off**.

      Valid value:

      -  **on** indicates that data is encrypted.
      -  **off** indicates that data is not encrypted.

   -  access_key

      Indicates the access key (AK) (obtained by users from the OBS page) used for the OBS access protocol. When you create a foreign table, its AK value is encrypted and saved to the metadata table of the database. This parameter is available only when **type** is **OBS**.

   -  secret_access_key

      Indicates the secret access key (SK) (obtained by users from the OBS page) used for the OBS access protocol. When you create a foreign table, its SK value is encrypted and saved to the metadata table of the database. This parameter is available only when **type** is **OBS**.

   -  dli_address

      Specifies the endpoint of the DLI service. This parameter is available only when **type** is **DLI**.

   -  dli_access_key

      Specifies the access key (AK) (obtained by users from the DLI console) used for the DLI access protocol. When you create a foreign table, its AK value is encrypted and saved to the metadata table of the database. This parameter is available only when **type** is **DLI**.

   -  dli_secret_access_key

      Specifies the secret access key (SK) (obtained by users from the DLI console) used for the DLI access protocol. When you create a foreign table, its SK value is encrypted and saved to the metadata table of the database. This parameter is available only when **type** is **DLI**.

   -  region

      Indicates the IP address or domain name of the OBS server. This parameter is available only when **type** is **OBS**.

   -  dbname

      Specifies the database name of a remote cluster to be connected. This parameter is used for collaborative analysis and cross-cluster interconnection.

   -  username

      Specifies the username of a remote cluster to be connected. This parameter is used for collaborative analysis and cross-cluster interconnection.

   -  password

      Specifies the password of a remote cluster to be connected. This parameter is used for collaborative analysis and cross-cluster interconnection.

   -  syncsrv

      This parameter is used only for cross-cluster interconnection and indicates the GDS service used during data synchronization. The method for setting this parameter is the same as that for setting the **location** attribute of the GDS foreign table. This feature is supported only in 8.1.2 or later.

-  **new_owner**

   Indicates the new owner of the server. To change the owner, you must be the owner of the external server and a direct or indirect member of the new owning role, and must have the **USAGE** permission on the encapsulator of the external server.

-  **new_name**

   Indicates the new name of the server.

-  **REFRESH OPTIONS**

   Refreshes the HDFS configuration file. This command is executed when the configuration file is modified. If this command is not executed, an access error may be reported.

Examples
--------

Change the IP address of the **hdfs_server** server.

::

   ALTER SERVER hdfs_server OPTIONS ( SET address '10.10.0.110:25000,10.10.0.120:25000');

Change the **hdfscfgpath** of the **hdfs_server** server.

::

   ALTER SERVER hdfs_server OPTIONS ( SET hdfscfgpath '/opt/bigdata/hadoop');

Helpful Links
-------------

:ref:`CREATE SERVER <dws_06_0175>` :ref:`DROP SERVER <dws_06_0206>`
