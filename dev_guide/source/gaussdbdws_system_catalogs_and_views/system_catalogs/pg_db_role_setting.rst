:original_name: dws_04_0583.html

.. _dws_04_0583:

PG_DB_ROLE_SETTING
==================

**PG_DB_ROLE_SETTING** records the default values of configuration items bonded to each role and database when the database is running.

.. table:: **Table 1** PG_DB_ROLE_SETTING columns

   +-------------+--------+--------------------------------------------------------------------------------------------------------+
   | Name        | Type   | Description                                                                                            |
   +=============+========+========================================================================================================+
   | setdatabase | oid    | Database corresponding to the configuration items; the value is **0** if the database is not specified |
   +-------------+--------+--------------------------------------------------------------------------------------------------------+
   | setrole     | oid    | Role corresponding to the configuration items; the value is **0** if the role is not specified         |
   +-------------+--------+--------------------------------------------------------------------------------------------------------+
   | setconfig   | text[] | Default value of configuration items when the database is running                                      |
   +-------------+--------+--------------------------------------------------------------------------------------------------------+
