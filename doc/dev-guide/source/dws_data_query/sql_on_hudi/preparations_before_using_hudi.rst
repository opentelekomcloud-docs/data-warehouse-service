:original_name: dws_04_1071.html

.. _dws_04_1071:

Preparations Before Using Hudi
==============================

Prerequisites
-------------

-  You have created an OBS agency and OBS data source. For details, see section "Managing OBS Data Sources" in the *Data Warehouse Service User Guide*.
-  Before using SQL on Hudi, ensure that the **enable_dws_bigdata** parameter is enabled. If an error message similar to "flight into error" is displayed, contact technical support to enable this parameter.

Authorizing the Use of Data Sources
-----------------------------------

Run the **GRANT** command to grant a user the permission to use data sources.

::

   GRANT USAGE ON FOREIGN SERVER server_name TO role_name;

Example:

Grant the user **u1** the permission to access the data source **hudi_server**.

::

   GRANT USAGE ON FOREIGN SERVER hudi_server TO u1;

Granting Permissions for Using Foreign Tables
---------------------------------------------

Run the following command to grant a user the permission to use foreign tables:

::

   ALTER USER role_name USEFT;

Example:

Grant the user **u1** the permission to access foreign tables.

::

   ALTER USER u1 USEFT;
