:original_name: dws_04_0575.html

.. _dws_04_0575:

PG_AUTH_HISTORY
===============

**PG_AUTH_HISTORY** records the authentication history of the role. It is accessible only to users with system administrator rights.

.. table:: **Table 1** PG_AUTH_HISTORY columns

   +--------------+--------------------------+-------------------------------------------------------------------------------+
   | Name         | Type                     | Description                                                                   |
   +==============+==========================+===============================================================================+
   | roloid       | oid                      | ID of the role                                                                |
   +--------------+--------------------------+-------------------------------------------------------------------------------+
   | passwordtime | timestamp with time zone | Time of password creation and change                                          |
   +--------------+--------------------------+-------------------------------------------------------------------------------+
   | rolpassword  | text                     | Role password that is encrypted using MD5 or SHA256, or that is not encrypted |
   +--------------+--------------------------+-------------------------------------------------------------------------------+
