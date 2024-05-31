:original_name: dws_04_0602.html

.. _dws_04_0602:

PG_OBSSCANINFO
==============

**PG_OBSSCANINFO** defines the OBS runtime information scanned in cluster acceleration scenarios. Each record corresponds to a piece of runtime information of a foreign table on OBS in a query.

.. table:: **Table 1** PG_OBSSCANINFO columns

   +--------------+-----------+-----------+---------------------------------------------+
   | Name         | Type      | Reference | Description                                 |
   +==============+===========+===========+=============================================+
   | query_id     | bigint    | ``-``     | Query ID                                    |
   +--------------+-----------+-----------+---------------------------------------------+
   | user_id      | text      | ``-``     | Database user who performs queries          |
   +--------------+-----------+-----------+---------------------------------------------+
   | table_name   | text      | ``-``     | Name of a foreign table on OBS              |
   +--------------+-----------+-----------+---------------------------------------------+
   | file_type    | text      | ``-``     | Format of files storing the underlying data |
   +--------------+-----------+-----------+---------------------------------------------+
   | time_stamp   | time_stam | ``-``     | Scanning start time                         |
   +--------------+-----------+-----------+---------------------------------------------+
   | actual_time  | double    | ``-``     | Scanning execution time, in seconds         |
   +--------------+-----------+-----------+---------------------------------------------+
   | file_scanned | bigint    | ``-``     | Number of files scanned                     |
   +--------------+-----------+-----------+---------------------------------------------+
   | data_size    | double    | ``-``     | Size of data scanned, in bytes              |
   +--------------+-----------+-----------+---------------------------------------------+
   | billing_info | text      | ``-``     | Reserved columns                            |
   +--------------+-----------+-----------+---------------------------------------------+
