:original_name: dws_16_0029.html

.. _dws_16_0029:

Verification
============

Verification After Migration
----------------------------

After DSC converts the source sql files, execute the converted files on target GaussDB(DWS) and provide a report with details of number of statements succeeded and failed.

After the DSC finishes the translation, it will invoke (controlled through a configuration item) post migration verification script. The verification script (for details about the configuration, see the configuration file) is connected to the target GaussDB database and executed.

The post migration verification script will connect to the target gauss database (details are configured in a configuration file) and executes the scripts.

#. **application.properties** in config folder

   Execute migrated script on Gauss DB: true/false, default value = false

   executesqlingauss=true

   true: It will execute the migrated script on gaussdb

#. **gaussdb.properties** in config folder

   #Target Database configurations

   .. code-block::

      #gauss database user with all privileges
       gaussdb-user=
       gaussdb-port=
       #Database name for GaussDBA
       gaussdb-name=
       #gaussdb ip
       gaussdb-ip=

   **Dependency on the gsql client:**

   a. gsql (GaussDB(DWS)) is required for executing scripts on GaussDB(DWS). Therefore, to ensure the smooth running of DSC, DSC is required to run on a node installed with a GaussDB(DWS) instance or client (gsql), and the user that performs verification must have the permission for executing commands using gsql.

   b. Since the Gauss DB Instance/Client can be installed on a Linux OS only, this can be used to verify functionality only on a Linux environment.

   c. To execute the gsql command on a remote GaussDB instance, it is advised to add the client system IP/hostname in the following configuration file of Gauss DB instance.

      .. code-block::

         /home/gsmig/database/coordinator
         ---pg_hba.conf

**Response**

**GaussDB(DWS)**

.. code-block::

   ********************** Verification Started ******************************
   Sql script execution on Gauss DB start time : Wed Jan 22 17:27:07 CST 2020
   Sql script execution on Gauss DB end time : Wed Jan 22 17:27:44 CST 2020

   Summary of Verification :
   ==================================================================================================================================
   Statement                | Total               | Passed              | Failed              | Success Rate(%)
   -----------------------------------------------------------------------------------------------------------------------------------
   COMMENT                  | 15                  | 15                  | 0                   | 100
   CREATE VIEW              | 4                   | 3                   | 1                   | 75
   CREATE INDEX             | 4                   | 3                   | 1                   | 75
   CREATE TABLE             | 6                   | 6                   | 0                   | 100
   ALTER TABLE              | 3                   | 3                   | 0                   | 100
   ---------------------------------------------------------------------------------------------------------------------------------
   Total                    | 32                  | 30                  | 2                   | 93

   Gauss Execution Log file : /home/gsmig/18Jan/DSC/DSC/log/gaussexecutionlog.log
   Gauss Execution Error Log file : /home/gsmig/18Jan/DSC/DSC/log/gaussexecutionerror.log
   Verification finished in 38 seconds

   ********************** Verification Completed ****************************
