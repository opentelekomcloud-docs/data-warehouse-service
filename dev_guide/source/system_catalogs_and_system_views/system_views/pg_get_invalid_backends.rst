:original_name: dws_04_0729.html

.. _dws_04_0729:

PG_GET_INVALID_BACKENDS
=======================

**PG_GET_INVALID_BACKENDS** displays the information about backend threads on the CN that are connected to the current standby DN.

.. table:: **Table 1** PG_GET_INVALID_BACKENDS columns

   +---------------+--------------------------+--------------------------------------------------+
   | Name          | Type                     | Description                                      |
   +===============+==========================+==================================================+
   | pid           | bigint                   | Thread ID                                        |
   +---------------+--------------------------+--------------------------------------------------+
   | node_name     | text                     | Node information connected to the backend thread |
   +---------------+--------------------------+--------------------------------------------------+
   | dbname        | name                     | Name of the connected database                   |
   +---------------+--------------------------+--------------------------------------------------+
   | backend_start | timestamp with time zone | Backend thread startup time                      |
   +---------------+--------------------------+--------------------------------------------------+
   | query         | text                     | Query statement performed by the backend thread  |
   +---------------+--------------------------+--------------------------------------------------+
