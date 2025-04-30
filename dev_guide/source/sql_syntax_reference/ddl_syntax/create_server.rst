:original_name: dws_06_0175.html

.. _dws_06_0175:

CREATE SERVER
=============

Function
--------

**CREATE SERVER** creates an external server.

An external server stores information of HDFS clusters, OBS servers, DLI connections, or other homogeneous clusters.

Precautions
-----------

By default, only the system administrator can create a foreign server. Otherwise, creating a server requires USAGE permission on the foreign-data wrapper being used. The syntax is as follows:

::

   GRANT USAGE ON FOREIGN DATA WRAPPER fdw_name TO username;

**fdw_name** is the name of **FOREIGN DATA WRAPPER**, and **username** is the user name of creating **SERVER**.

Syntax
------

::

   CREATE SERVER server_name
       FOREIGN DATA WRAPPER fdw_name
       OPTIONS ( { option_name ' value ' } [, ...] ) ;

Parameter Description
---------------------

-  **server_name**

   Name of the foreign server to be created. The server name must be unique in a database.

   Value range: The length must be less than or equal to 63.

-  **FOREIGN DATA WRAPPER fdw_name**

   Specifies the name of the foreign data wrapper.

   Value range: **fdw_name** is the name of the data wrapper created by the system during database initialization. Currently, for the HDFS cluster, the value of **fdw_name** can be **hdfs_fdw** or **dfs_fdw**. For other homogeneous clusters, the value of **fdw_name** is **gc_fdw**. In data import and export scenarios, the GDS foreign table uses **gsmpp_server** and **fdw_name** is **dist_fdw**.

-  **OPTIONS ( { option_name ' value ' } [, ...] )**

   Specifies the parameters for the server. The detailed parameter description is as follows:

   -  address

      Specifies the IP address of the OBS service endpoint or HDFS cluster.

      OBS: Specifies the endpoint of the OBS service.

      HDFS: Specifies the IP address and port number of a NameNode (metadata node) in the HDFS cluster, or the IP address and port number of a CN in other homogeneous clusters.

      HDFS NameNodes are deployed in primary/secondary mode for HA. Add the addresses of the primary and secondary NameNodes to **ADDRESS**. When accessing HDFS, GaussDB(DWS) dynamically checks for the primary NameNode.

      If the HDFS is in federation mode, you can add all router addresses to the **address** value. When GaussDB(DWS) accesses the HDFS service, the system dynamically and randomly searches for the active router.

      .. note::

         -  The **address** option must exist. In the cross-cluster interconnection scenario, only one **address** option can be set.
         -  If the server type is DLI, the address is the OBS address stored on DLI.
         -  If the HDFS is in federation mode, that is, **fed 'rbf'**, address can be set to multiple groups of IP addresses and ports, corresponding to the address of the HDFS router.

   -  hdfscfgpath

      This parameter is available only when **type** is **HDFS**.

      You can set the **hdfscfgpath** parameter to specify the HDFS configuration file path. GaussDB(DWS) accesses the HDFS cluster based on the connection configuration mode and security mode specified in the HDFS configuration file stored in that path. If the HDFS cluster is connected in non-secure mode, data transmission encryption is not supported.

      If the **address option** is not specified, the address specified by **hdfscfgpath** in the configuration file is used by default.

   -  fed

      Indicates that **dfs_fdw** is connected to HDFS in federation mode.

      The value **rbf** indicates that HDFS uses the RBF mode.

      .. note::

         This feature is supported in 8.1.2 or later.

   -  encrypt

      Specifies whether data is encrypted. This parameter is available only when **type** is **OBS**. The default value is **off**.

      Valid value:

      -  **on** indicates that data is encrypted.
      -  **off** indicates that data is not encrypted.

   -  access_key

      Specifies the access key (AK) (obtained by users from the OBS console) used for the OBS access protocol. When you create a foreign table, its AK value is encrypted and saved to the metadata table of the database.

      .. note::

         -  If **FOREIGN DATA WRAPPER** is set to **dfs_fdw**, this parameter can be set only when type is set to OBS.
         -  For clusters of version 8.2.0 or later, this parameter can be specified when **FOREIGN DATA WRAPPER** is set to **dist_fdw**.

   -  secret_access_key

      Indicates the secret access key (SK) (obtained by users from the OBS page) used for the OBS access protocol. When you create a foreign table, its SK value is encrypted and saved to the metadata table of the database.

      .. note::

         -  If **FOREIGN DATA WRAPPER** is set to **dfs_fdw**, this parameter can be set only when type is set to OBS.
         -  For clusters of 8.2.0 or later, this parameter can be specified when **FOREIGN DATA WRAPPER** is set to **dist_fdw**.

   -  security_token

      Corresponds to the **SecurityToken** value of the temporary security credential in IAM. A temporary AK, a temporary SK, and a temporary security token form a temporary security credential. This parameter is supported by version 8.2.0 or later clusters.

      .. note::

         -  If **FOREIGN DATA WRAPPER** is set to **dfs_fdw**, this parameter can be set only when type is set to OBS.
         -  For clusters of 8.2.0 or later, this parameter can be specified when **FOREIGN DATA WRAPPER** is set to **dist_fdw**.
         -  When this parameter is used, **access_key** and **secret_access_key** correspond to the temporary AK and SK, respectively.

   -  type

      Specifies the **dfs_fdw** connection type.

      Valid value:

      -  **OBS** indicates that OBS is connected.
      -  **HDFS** indicates that HDFS is connected.
      -  **DLI** indicates that DLI is connected.

   -  dli_address

      Specifies the endpoint of the DLI service. This parameter is available only when **type** is **DLI**.

   -  dli_access_key

      Specifies the access key (AK) (obtained by users from the DLI console) used for the DLI access protocol. When you create a foreign table, its AK value is encrypted and saved to the metadata table of the database. This parameter is available only when **type** is **DLI**.

   -  dli_secret_access_key

      Specifies the secret access key (SK) (obtained by users from the DLI console) used for the DLI access protocol. When you create a foreign table, its SK value is encrypted and saved to the metadata table of the database. This parameter is available only when **type** is **DLI**.

   -  dbname

      Specifies the database name of a remote cluster to be connected. This parameter is used for collaborative analysis and cross-cluster interconnection.

   -  username

      Specifies the username of a remote cluster to be connected. This parameter is used for collaborative analysis and cross-cluster interconnection.

   -  password

      Specifies the password of a remote cluster to be connected. This parameter is used for collaborative analysis and cross-cluster interconnection.

      .. note::

         When an on-premises cluster is migrated to the cloud, the password in the server configuration exported from the on-premises cluster is in ciphertext. The encryption and decryption keys of the on-premises cluster are different from those of the cloud cluster. Therefore, if **CREATE SERVER** is executed on the cloud cluster, the execution fails and a decryption failure error is reported. In this case, you need to manually change the password in **CREATE SERVER** to a plaintext password.

   -  syncsrv

      This parameter is used only for cross-cluster interconnection and indicates the GDS service used during data synchronization. The method for setting this parameter is the same as that for setting the **location** attribute of the GDS foreign table.

Examples
--------

Create the **hdfs_server** server, in which **hdfs_fdw** is the foreign-data wrapper:

::

   CREATE SERVER hdfs_server FOREIGN DATA WRAPPER HDFS_FDW OPTIONS
      (address '10.10.0.100:25000,10.10.0.101:25000',
       hdfscfgpath '/opt/hadoop_client/HDFS/hadoop/etc/hadoop',
       type 'HDFS'
   ) ;

Create the **obs_server** server, in which **dfs_fdw** is the foreign-data wrapper:

::

   CREATE SERVER obs_server FOREIGN DATA WRAPPER DFS_FDW OPTIONS (
     address 'obs.example.com',
      access_key 'xxxxxxxxx',
     secret_access_key 'yyyyyyyyyyyyy',
     type 'obs'
   );

Create the **dli_server** server, in which **dfs_fdw** is the foreign-data wrapper:

::

   CREATE SERVER dli_server FOREIGN DATA WRAPPER DFS_FDW OPTIONS (
     address 'obs.example.com',
     access_key 'xxxxxxxxx',
     secret_access_key 'yyyyyyyyyyyyy',
     type 'dli',
     dli_address 'dli.example.com',
     dli_access_key 'xxxxxxxxx',
     dli_secret_access_key 'yyyyyyyyyyyyy'
   );

You are advised to create another server in the homogeneous cluster, where **gc_fdw** is the foreign data wrapper in the database:

::

   CREATE SERVER server_remote FOREIGN DATA WRAPPER GC_FDW OPTIONS
      (address '10.10.0.100:25000,10.10.0.101:25000',
     dbname 'test',
     username 'test',
     password 'xxxxxxxx'
   );

Create a server whose **FOREIGN DATA WRAPPER** is **dist_fdw** for importing and exporting text data on OBS.

::

   CREATE SERVER import_server FOREIGN DATA WRAPPER DIST_FDW OPTIONS
   (
     access_key 'ak_string',
     secret_access_key 'sk_string'
   );

Helpful Links
-------------

:ref:`ALTER SERVER <dws_06_0138>` :ref:`DROP SERVER <dws_06_0206>`
