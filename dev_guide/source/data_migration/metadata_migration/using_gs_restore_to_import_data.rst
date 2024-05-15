:original_name: dws_04_0209.html

.. _dws_04_0209:

Using gs_restore to Import Data
===============================

Scenarios
---------

**gs_restore** is an import tool provided by GaussDB(DWS). You can use **gs_restore** to import the files exported by **gs_dump** to a database. **gs_restore** can import the files in .tar, custom, or directory format.

**gs_restore** can:

-  Import data to a database.

   If a database is specified, data is imported to the database. If multiple databases are specified, the password for connecting to each database also needs to be specified.

-  Import data to a script.

   If no database is specified, a script containing the SQL statement to recreate the database is created and written to a file or standard output. This script output is equivalent to the plain text output of **gs_dump**.

You can specify and sort the data to be imported.

Procedure
---------

.. note::

   **gs_restore** incrementally imports data by default. To prevent data exception caused by consecutive imports, use the **-e** and **-c** parameters for each import. In this way, existing data is deleted from the target database before each import; the system exists the import task with an error (error message is displayed after the import process is complete) and proceeds with the next.

#. Log in to the server as the **root** user and run the following command to go to the data storage path:

   ::

      cd /opt/bin

#. Use **gs_restore** to import all object definitions from the exported file of the whole **postgres** database to the **backupdb** database.

   ::

      gs_restore -W password -U jack /home//backup/MPPDB_backup.tar -p 8000 -h 10.10.10.100 -d backupdb -s -e -c

   .. table:: **Table 1** Common parameters

      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                                                                                                                                                                                                                                                           | Example Value         |
      +=======================+=======================================================================================================================================================================================================================================================================================================+=======================+
      | -U                    | Username for database connection.                                                                                                                                                                                                                                                                     | -U jack               |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | -W                    | User password for database connection.                                                                                                                                                                                                                                                                | -W *Password*         |
      |                       |                                                                                                                                                                                                                                                                                                       |                       |
      |                       | -  This parameter is not required for database administrators if the trust policy is used for authentication.                                                                                                                                                                                         |                       |
      |                       | -  If you connect to the database without specifying this parameter and you are not a database administrator, you will be prompted to enter the password.                                                                                                                                             |                       |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | -d                    | Database to which data will be imported.                                                                                                                                                                                                                                                              | -d backupdb           |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | -p                    | TCP port or the local Unix-domain socket file extension on which the server is listening for connections.                                                                                                                                                                                             | -p 8000               |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | -h                    | *Cluster address*: If a public network address is used for connection, set this parameter to **Public Network Address** or **Public Network Domain Name**. If a private network address is used for connection, set this parameter to **Private Network Address** or **Private Network Domain Name**. | -h 10.10.10.100       |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | -e                    | Exits the current import task and performs the next if an error occurs when you send a SQL statement in the current import task. Error messages are displayed after the import process is complete.                                                                                                   | ``-``                 |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | -c                    | Cleans existing objects from the target database before the import.                                                                                                                                                                                                                                   | ``-``                 |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | -s                    | Imports only object definitions in schemas and does not import data. Sequence values will also not be imported.                                                                                                                                                                                       | ``-``                 |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

   For details about other parameters, see "Server Tools > gs_restore" in the *Tool Reference*.

Examples
--------

Example 1: Run **gs_restore** to import data and all object definitions of the **postgres** database from the **MPPDB_backup.dmp** file (custom format).

::

   gs_restore -W password backup/MPPDB_backup.dmp -p 8000 -h 10.10.10.100 -d backupdb
   gs_restore[2017-07-21 19:16:26]: restore operation successfu
   gs_restore: total time: 13053  ms

Example 2: Run **gs_restore** to import data and all object definitions of the **postgres** database from the **MPPDB_backup.tar** file.

::

   gs_restore backup/MPPDB_backup.tar -p 8000 -h 10.10.10.100 -d backupdb
   gs_restore[2017-07-21 19:21:32]: restore operation successful
   gs_restore[2017-07-21 19:21:32]: total time: 21203  ms

Example 3: Run **gs_restore** to import data and all object definitions of the **postgres** database from the **MPPDB_backup** directory.

::

   gs_restore backup/MPPDB_backup -p 8000 -h 10.10.10.100 -d backupdb
   gs_restore[2017-07-21 19:26:46]: restore operation successful
   gs_restore[2017-07-21 19:26:46]: total time: 21003  ms

Example 4: Run **gs_restore** to import all object definitions of the **postgres** database from the **MPPDB_backup.tar** file. Table data is not imported.

::

   gs_restore -W password /home//backup/MPPDB_backup.tar -p 8000 -h 10.10.10.100 -d backupdb -s -e -c
   gs_restore[2017-07-21 19:46:27]: restore operation successful
   gs_restore[2017-07-21 19:46:27]: total time: 32993  ms

Example 5: Run **gs_restore** to import data and all definitions in the **PUBLIC** schema from the **MPPDB_backup.dmp** file. Existing objects are deleted from the target database before the import. If an existing object references to an object in another schema, you need to manually delete the referenced object first.

::

   gs_restore backup/MPPDB_backup.dmp -p 8000 -h 10.10.10.100 -d backupdb -e -c -n PUBLIC
   gs_restore: [archiver (db)] Error while PROCESSING TOC:
   gs_restore: [archiver (db)] Error from TOC entry 313; 1259 337399 TABLE table1 gaussdba
   gs_restore: [archiver (db)] could not execute query: ERROR:  cannot drop table table1 because other objects depend on it
   DETAIL:  view t1.v1 depends on table table1
   HINT:  Use DROP ... CASCADE to drop the dependent objects too.
   Command was: DROP TABLE public.table1;

Manually delete the referenced object and create it again after the import is complete.

::

   gs_restore backup/MPPDB_backup.dmp -p 8000 -h 10.10.10.100 -d backupdb -e -c -n PUBLIC
   gs_restore[2017-07-21 19:52:26]: restore operation successful
   gs_restore[2017-07-21 19:52:26]: total time: 2203  ms

Example 6: Run **gs_restore** to import the definition of the **hr.staffs** table in the **PUBLIC** schema from the **MPPDB_backup.dmp** file. Before the import, the **hr.staffs** table does not exist.

::

   gs_restore backup/MPPDB_backup.dmp -p 8000 -h 10.10.10.100 -d backupdb -e -c -s -n PUBLIC -t hr.staffs
   gs_restore[2017-07-21 19:56:29]: restore operation successful
   gs_restore[2017-07-21 19:56:29]: total time: 21000  ms

Example 7: Run **gs_restore** to import data of the **hr.staffs** table in **PUBLIC** schema from the **MPPDB_backup.dmp** file. Before the import, the **hr.staffs** table is empty.

::

   gs_restore backup/MPPDB_backup.dmp -p 8000 -h 10.10.10.100 -d backupdb -e -a -n PUBLIC -t hr.staffs
   gs_restore[2017-07-21 20:12:32]: restore operation successful
   gs_restore[2017-07-21 20:12:32]: total time: 20203  ms

Example 8: Run **gs_restore** to import the definition of the **hr.staffs** table. Before the import, the **hr.staffs** table already exists.

::

   human_resource=# select * from hr.staffs;
    staff_id | first_name  |  last_name  |  email   |    phone_number    |      hire_date      | employment_id |  salary  | commission_pct | manager_id | section_id
   ----------+-------------+-------------+----------+--------------------+---------------------+---------------+----------+----------------+------------+------------
         200 | Jennifer    | Whalen      | JWHALEN  | 515.123.4444       | 1987-09-17 00:00:00 | AD_ASST       |  4400.00 |                |        101 |         10
         201 | Michael     | Hartstein   | MHARTSTE | 515.123.5555       | 1996-02-17 00:00:00 | MK_MAN        | 13000.00 |                |        100 |         20

   gsql -d human_resource -p 8000
   gsql ((GaussDB 8.1.3 build 39137c2d) compiled at 2022-04-01 15:43:11 commit 3629 last mr 5138 release)
   Non-SSL connection (SSL connection is recommended when requiring high-security)
   Type "help" for help.

   human_resource=# drop table hr.staffs CASCADE;
   NOTICE:  drop cascades to view hr.staff_details_view

   gs_restore -W password /home//backup/MPPDB_backup.tar -p 8000 -h 10.10.10.100-d human_resource -n hr -t staffs -s -e
   restore operation successful
   total time: 904  ms

   human_resource=# select * from hr.staffs;
    staff_id | first_name | last_name | email | phone_number | hire_date | employment_id | salary | commission_pct | manager_id | section_id
   ----------+------------+-----------+-------+--------------+-----------+---------------+--------+----------------+------------+------------
   (0 rows)

Example 9: Run **gs_restore** to import data and definitions of the **staffs** and **areas** tables. Before the import, the **staffs** and **areas** tables do not exist.

::

   human_resource=# \d
                                    List of relations
    Schema |        Name        | Type  |  Owner   |             Storage
   --------+--------------------+-------+----------+----------------------------------
    hr     | employment_history | table |  | {orientation=row,compression=no}
    hr     | employments        | table |  | {orientation=row,compression=no}
    hr     | places             | table |  | {orientation=row,compression=no}
    hr     | sections           | table |  | {orientation=row,compression=no}
    hr     | states             | table |  | {orientation=row,compression=no}
   (5 rows)

   gs_restore -W password /home/mppdb/backup/MPPDB_backup.tar -p 8000 -h 10.10.10.100 -d human_resource -n hr -t staffs -n hr -t areas
   restore operation successful
   total time: 724  ms

   human_resource=# \d
                                    List of relations
    Schema |        Name        | Type  |  Owner   |             Storage
   --------+--------------------+-------+----------+----------------------------------
    hr     | areas              | table |  | {orientation=row,compression=no}
    hr     | employment_history | table |  | {orientation=row,compression=no}
    hr     | employments        | table |  | {orientation=row,compression=no}
    hr     | places             | table |  | {orientation=row,compression=no}
    hr     | sections           | table |  | {orientation=row,compression=no}
    hr     | staffs             | table |  | {orientation=row,compression=no}
    hr     | states             | table |  | {orientation=row,compression=no}
   (7 rows)

   human_resource=# select * from hr.areas;
    area_id |       area_name
   ---------+------------------------
          4 | Iron
          1 | Wood
          2 | Lake
          3 | Desert
   (4 rows)

Example 10: Run **gs_restore** to import data and all object definitions in the **hr** schema.

::

   gs_restore -W password /home//backup/MPPDB_backup1.sql -p 8000 -h 10.10.10.100 -d backupdb -n hr -e -c
   restore operation successful
   total time: 702  ms

Example 11: Run **gs_restore** to import all object definitions in the **hr** and **hr1** schemas to the **backupdb** database.

::

   gs_restore -W password /home//backup/MPPDB_backup2.dmp -p 8000 -h 10.10.10.100 -d backupdb -n hr -n hr1 -s
   restore operation successful
   total time: 665  ms

Example 12: Run **gs_restore** to decrypt the files exported from the **human_resource** database and import them to the **backupdb** database.

::

   create database backupdb;


   gs_restore /home//backup/MPPDB_backup.tar -p 8000 -h 10.10.10.100 -d backupdb --with-key=1234567812345678
   restore operation successful
   total time: 23472  ms

   gsql -d backupdb -p 8000 -r
   gsql ((GaussDB 8.1.3 build 39137c2d) compiled at 2022-04-01 15:43:11 commit 3629 last mr 5138 release)
   Non-SSL connection (SSL connection is recommended when requiring high-security)
   Type "help" for help.

   backupdb=# select * from hr.areas;
    area_id |       area_name
   ---------+------------------------
          4 | Iron
          1 | Wood
          2 | Lake
          3 | Desert
   (4 rows)

Example 13: **user 1** does not have the permission to import data from an exported file to the **backupdb** database and **role1** has this permission. To import the exported data to the **backupdb** database, you can set **--role** to **role1** in the **gs_restore** command.

::

   human_resource=# CREATE USER user1 IDENTIFIED BY 'password';

   gs_restore -U user1 -W password /home//backup/MPPDB_backup.tar -p 8000 -h 10.10.10.100 -d backupdb --role role1 --rolepassword password
   restore operation successful
   total time: 554  ms

   gsql -d backupdb -p 8000 -r
   gsql ((GaussDB 8.1.3 build 39137c2d) compiled at 2022-04-01 15:43:11 commit 3629 last mr 5138 release)
   Non-SSL connection (SSL connection is recommended when requiring high-security)
   Type "help" for help.

   backupdb=# select * from hr.areas;
    area_id |       area_name
   ---------+------------------------
          4 | Iron
          1 | Wood
          2 | Lake
          3 | Desert
   (4 rows)
