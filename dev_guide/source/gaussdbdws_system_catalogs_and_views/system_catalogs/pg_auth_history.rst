:original_name: dws_04_0575.html

.. _dws_04_0575:

PG_AUTH_HISTORY
===============

**PG_AUTH_HISTORY** records the authentication history of the role. It is accessible only to users with system administrator rights.

.. table:: **Table 1** PG_AUTH_HISTORY columns

   +--------------+--------------------------+-------------------------------------------------------------------------------+
   | Column       | Type                     | Description                                                                   |
   +==============+==========================+===============================================================================+
   | roloid       | OID                      | Role identifier                                                               |
   +--------------+--------------------------+-------------------------------------------------------------------------------+
   | passwordtime | Timestamp with time zone | Time of password creation and change                                          |
   +--------------+--------------------------+-------------------------------------------------------------------------------+
   | rolpassword  | Text                     | Role password that is encrypted using MD5 or SHA256, or that is not encrypted |
   +--------------+--------------------------+-------------------------------------------------------------------------------+
