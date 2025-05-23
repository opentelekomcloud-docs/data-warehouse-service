:original_name: dws_06_0026.html

.. _dws_06_0026:

Constant and Macro
==================

:ref:`Table 1 <en-us_topic_0000001764675306__t556b0d4fd31645a3aaaa95c4dfe3c89c>` lists the constants and macros that can be used in GaussDB(DWS).

.. _en-us_topic_0000001764675306__t556b0d4fd31645a3aaaa95c4dfe3c89c:

.. table:: **Table 1** Constants and macros

   +-----------------------+-------------------------------------------------------------------+----------------------------+
   | Parameter             | Description                                                       | Example SQL Statements     |
   +=======================+===================================================================+============================+
   | CURRENT_CATALOG       | Specifies the current database.                                   | ::                         |
   |                       |                                                                   |                            |
   |                       |                                                                   |    SELECT CURRENT_CATALOG; |
   +-----------------------+-------------------------------------------------------------------+----------------------------+
   | CURRENT_ROLE          | Current role                                                      | ::                         |
   |                       |                                                                   |                            |
   |                       |                                                                   |    SELECT CURRENT_ROLE;    |
   +-----------------------+-------------------------------------------------------------------+----------------------------+
   | CURRENT_SCHEMA        | Current database model                                            | ::                         |
   |                       |                                                                   |                            |
   |                       |                                                                   |    SELECT CURRENT_SCHEMA;  |
   +-----------------------+-------------------------------------------------------------------+----------------------------+
   | CURRENT_USER          | Current user                                                      | ::                         |
   |                       |                                                                   |                            |
   |                       |                                                                   |    SELECT CURRENT_USER;    |
   +-----------------------+-------------------------------------------------------------------+----------------------------+
   | LOCALTIMESTAMP        | Current session time (without time zone)                          | ::                         |
   |                       |                                                                   |                            |
   |                       |                                                                   |    SELECT LOCALTIMESTAMP;  |
   +-----------------------+-------------------------------------------------------------------+----------------------------+
   | NULL                  | This parameter is left blank.                                     | ``-``                      |
   +-----------------------+-------------------------------------------------------------------+----------------------------+
   | SESSION_USER          | Current system user                                               | ::                         |
   |                       |                                                                   |                            |
   |                       |                                                                   |    SELECT SESSION_USER;    |
   +-----------------------+-------------------------------------------------------------------+----------------------------+
   | SYSDATE               | Current system date                                               | ::                         |
   |                       |                                                                   |                            |
   |                       |                                                                   |    SELECT SYSDATE;         |
   |                       |                                                                   |                            |
   |                       |                                                                   | or                         |
   |                       |                                                                   |                            |
   |                       |                                                                   | .. code-block::            |
   |                       |                                                                   |                            |
   |                       |                                                                   |    SELECT now()::DATE;     |
   +-----------------------+-------------------------------------------------------------------+----------------------------+
   | USER                  | Current user, which is the same as the value of **CURRENT_USER**. | ::                         |
   |                       |                                                                   |                            |
   |                       |                                                                   |    SELECT USER;            |
   +-----------------------+-------------------------------------------------------------------+----------------------------+
