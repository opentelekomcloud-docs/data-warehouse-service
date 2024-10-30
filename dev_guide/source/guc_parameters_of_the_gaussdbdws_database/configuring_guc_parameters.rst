:original_name: dws_04_0885.html

.. _dws_04_0885:

Configuring GUC Parameters
==========================

To ensure the optimal performance of GaussDB(DWS), you can adjust the GUC parameters in the database.

Parameter Types and Values
--------------------------

-  The GUC parameters of GaussDB(DWS) are classified into the following types:

   -  SUSET: database administrator parameters. This type of parameters takes effect immediately after they are set. You do not need to restart the cluster. If a parameter of this type is set in the current session, the parameter takes effect only in the current session.
   -  USERSET: common user parameters. This type of parameters takes effect immediately after they are set. You do not need to restart the cluster. If a parameter of this type is set in the current session, the parameter takes effect only in the current session.
   -  POSTMASTER: database server parameters. This type of parameters takes effect only after the cluster is restarted. After you modify a parameter of this type, the system displays a message indicating that the cluster is to be restarted. You are advised to manually restart the cluster during off-peak hours for the setting to take effect.
   -  SIGHUP: global database parameters. This type of parameters takes effect globally and cannot take effect for single sessions.
   -  BACKEND: global database parameters. This type of parameters takes effect globally and cannot take effect for single sessions.

-  All parameter names are case insensitive. A parameter value can be an integer, floating point number, string, Boolean value, or enumerated value.

   -  The Boolean values can be **on**/**off**, **true**/**false**, **yes**/**no**, or **1**/**0**, and are case-insensitive.
   -  The enumerated value range is specified in the **enumvals** column of the system catalog **pg_settings**.

-  For parameters using units, specify their units during the setting, or default units are used.

   -  The default units are specified in the **unit** column of **pg_settings**.
   -  The unit of memory can be KB, MB, or GB.
   -  The unit of time can be ms, s, min, h, or d.

.. _en-us_topic_0000001510522193__s8adb68393b48467a948956afaaaf8589:

Setting GUC Parameters
----------------------

You can configure GUC parameters in the following ways:

-  Method 1: After a cluster is created, log in to the GaussDB(DWS) console and modify the database parameters of the cluster. For details, see "Modifying Database Parameters" in *Data Warehouse Service (DWS) User Guide*.

-  Method 2: Connect to a cluster and run SQL commands to configure the parameters of the SUSET or USERSET type.

   Set parameters at database, user, or session levels.

   -  Set a database-level parameter.

      ::

         ALTER DATABASE dbname SET paraname TO value;

      The setting takes effect in the next session.

   -  Set a user-level parameter.

      ::

         ALTER USER username SET paraname TO value;

      The setting takes effect in the next session.

   -  Set a session-level parameter.

      ::

         SET paraname TO value;

      Parameter value in the current session is changed. After you exit the session, the setting becomes invalid.

Procedure
---------

The following example shows how to set **explain_perf_mode**.

#. View the value of **explain_perf_mode**.

   ::

      SHOW explain_perf_mode;
       explain_perf_mode
      -------------------
       normal
      (1 row)

#. Set **explain_perf_mode**.

   Perform one of the following operations:

   -  Set a database-level parameter.

      ::

         ALTER DATABASE gaussdb SET explain_perf_mode TO pretty;

      If the following information is displayed, the setting has been modified.

      .. code-block::

         ALTER DATABASE

      The setting takes effect in the next session.

   -  Set a user-level parameter.

      ::

         ALTER USER dbadmin SET explain_perf_mode TO pretty;

      If the following information is displayed, the setting has been modified.

      .. code-block::

         ALTER USER

      The setting takes effect in the next session.

   -  Set a session-level parameter.

      ::

         SET explain_perf_mode TO pretty;

      If the following information is displayed, the setting has been modified.

      .. code-block::

         SET

#. Check whether the parameter is correctly set.

   ::

      SHOW explain_perf_mode;
       explain_perf_mode
      --------------
       pretty
      (1 row)
