:original_name: dws_04_1071.html

.. _dws_04_1071:

Preparations Before Using Hudi
==============================

Prerequisites
-------------

You have created an OBS agency and OBS data source. For details, see section "Managing OBS Data Sources" in the *Data Warehouse Service User Guide*.

Authorizing the Use of OBS Data Sources
---------------------------------------

Run the **GRANT** command to grant a user the permission to use OBS data sources.

::

   GRANT USAGE ON FOREIGN SERVER server_name TO role_name;

Example:

Run the following command to grant user **sbi_fnd** the permission to access data source **obs_hudi**:

::

   GRANT USAGE ON FOREIGN SERVER obs_hudi TO sbi_fnd;

Granting Permissions for Using Foreign Tables
---------------------------------------------

Run the following command to grant a user the permission to use foreign tables:

::

   ALTER USER role_name USEFT;

Example:

Run the following command to grant the foreign table access permission to user **sbi_fnd**:

::

   ALTER USER sbi_fnd USEFT;
