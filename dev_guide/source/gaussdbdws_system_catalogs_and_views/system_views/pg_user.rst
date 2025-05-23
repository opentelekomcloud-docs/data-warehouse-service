:original_name: dws_04_0791.html

.. _dws_04_0791:

PG_USER
=======

**PG_USER** displays information about users who can access the database.

.. table:: **Table 1** PG_USER columns

   +-----------------+--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Column          | Type                     | Description                                                                                                                                                              |
   +=================+==========================+==========================================================================================================================================================================+
   | usename         | Name                     | User name                                                                                                                                                                |
   +-----------------+--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | usesysid        | OID                      | ID of this user                                                                                                                                                          |
   +-----------------+--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | usecreatedb     | boolean                  | Whether the user has the permission to create databases                                                                                                                  |
   +-----------------+--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | usesuper        | boolean                  | whether the user is the initial system administrator with the highest rights.                                                                                            |
   +-----------------+--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | usecatupd       | boolean                  | whether the user can directly update system tables. Only the initial system administrator whose usesysid is 10 has this permission. It is not available for other users. |
   +-----------------+--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | userepl         | boolean                  | Whether the user has the permission to duplicate data streams                                                                                                            |
   +-----------------+--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | passwd          | Text                     | Encrypted user password. The value is displayed as ``********.``                                                                                                         |
   +-----------------+--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | valbegin        | Timestamp with time zone | Account validity start time; null if no start time                                                                                                                       |
   +-----------------+--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | valuntil        | Timestamp with time zone | Password expiry time; null if no expiration                                                                                                                              |
   +-----------------+--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | respool         | Name                     | Resource pool where the user is in                                                                                                                                       |
   +-----------------+--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | parent          | OID                      | Parent user OID                                                                                                                                                          |
   +-----------------+--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spacelimit      | Text                     | The storage space of the permanent table.                                                                                                                                |
   +-----------------+--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tempspacelimit  | Text                     | The storage space of the temporary table.                                                                                                                                |
   +-----------------+--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spillspacelimit | Text                     | The operator disk flushing space.                                                                                                                                        |
   +-----------------+--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | useconfig       | Text[]                   | Session defaults for run-time configuration variables                                                                                                                    |
   +-----------------+--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | nodegroup       | Name                     | Name of the logical cluster associated with the user. If no logical cluster is associated, this column is left blank.                                                    |
   +-----------------+--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Query the current database user list.

::

   SELECT usename  FROM pg_user;
     usename
   -----------
    dbadmin
    u1
    u2
    u3
   (4 rows)
