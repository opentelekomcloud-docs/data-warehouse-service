:original_name: dws_04_0880.html

.. _dws_04_0880:

V$SESSION
=========

**V$SESSION** displays all session information about the current session.

.. table:: **Table 1** V$SESSION columns

   +----------+---------+-----------------------------------------------------------------------------------+
   | Name     | Type    | Description                                                                       |
   +==========+=========+===================================================================================+
   | sid      | bigint  | OID of the background process of the current activity                             |
   +----------+---------+-----------------------------------------------------------------------------------+
   | serial#  | integer | Sequence number of the active background process, which is **0** in GaussDB(DWS). |
   +----------+---------+-----------------------------------------------------------------------------------+
   | user#    | oid     | OID of the user that has logged in to the background process                      |
   +----------+---------+-----------------------------------------------------------------------------------+
   | username | name    | Name of the user that has logged in to the background process                     |
   +----------+---------+-----------------------------------------------------------------------------------+
