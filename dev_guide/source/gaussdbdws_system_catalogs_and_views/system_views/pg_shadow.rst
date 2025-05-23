:original_name: dws_04_0752.html

.. _dws_04_0752:

PG_SHADOW
=========

**PG_SHADOW** displays properties of all roles that are marked as **rolcanlogin** in **PG_AUTHID**.

This view is not readable to all users because it contains passwords. :ref:`PG_USER <dws_04_0791>` is a publicly readable view on **PG_SHADOW** that blanks out the password column.

.. table:: **Table 1** PG_SHADOW columns

   +-----------------+--------------------------+----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Column          | Type                     | Reference                              | Description                                                                                                                           |
   +=================+==========================+========================================+=======================================================================================================================================+
   | usename         | Name                     | :ref:`PG_AUTHID <dws_04_0574>`.rolname | User name                                                                                                                             |
   +-----------------+--------------------------+----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | usesysid        | OID                      | :ref:`PG_AUTHID <dws_04_0574>`.oid     | ID of a user                                                                                                                          |
   +-----------------+--------------------------+----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | usecreatedb     | boolean                  | ``-``                                  | Indicates that the user can create databases.                                                                                         |
   +-----------------+--------------------------+----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | usesuper        | boolean                  | ``-``                                  | Indicates that the user is an administrator.                                                                                          |
   +-----------------+--------------------------+----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | usecatupd       | boolean                  | ``-``                                  | Indicates that the user can update system catalogs. Even the system administrator cannot do this unless this column is **true**.      |
   +-----------------+--------------------------+----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | userepl         | boolean                  | ``-``                                  | User can initiate streaming replication and put the system in and out of backup mode.                                                 |
   +-----------------+--------------------------+----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | passwd          | Text                     | ``-``                                  | Password (possibly encrypted); null if none. See :ref:`PG_AUTHID <dws_04_0574>` for details about how encrypted passwords are stored. |
   +-----------------+--------------------------+----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | valbegin        | Timestamp with time zone | ``-``                                  | Account validity start time; null if no start time                                                                                    |
   +-----------------+--------------------------+----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | valuntil        | Timestamp with time zone | ``-``                                  | Password expiry time; null if no expiration                                                                                           |
   +-----------------+--------------------------+----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | respool         | Name                     | ``-``                                  | Resource pool used by the user                                                                                                        |
   +-----------------+--------------------------+----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | parent          | OID                      | ``-``                                  | Parent resource pool                                                                                                                  |
   +-----------------+--------------------------+----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | spacelimit      | Text                     | ``-``                                  | The storage space of the permanent table.                                                                                             |
   +-----------------+--------------------------+----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | tempspacelimit  | Text                     | ``-``                                  | The storage space of the temporary table.                                                                                             |
   +-----------------+--------------------------+----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | spillspacelimit | Text                     | ``-``                                  | The operator disk flushing space.                                                                                                     |
   +-----------------+--------------------------+----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | useconfig       | text[ ]                  | ``-``                                  | Session defaults for runtime configuration variables                                                                                  |
   +-----------------+--------------------------+----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
