:original_name: dws_04_0602.html

.. _dws_04_0602:

PG_OBSSCANINFO
==============

**PG_OBSSCANINFO** defines the OBS runtime information scanned in cluster acceleration scenarios. Each record corresponds to a piece of runtime information of a foreign table on OBS in a query.

.. table:: **Table 1** PG_OBSSCANINFO columns

   +--------------+-----------+-----------+---------------------------------------------+
   | Name         | Type      | Reference | Description                                 |
   +==============+===========+===========+=============================================+
   | query_id     | Bigint    | N/A       | Query ID                                    |
   +--------------+-----------+-----------+---------------------------------------------+
   | user_id      | Text      | N/A       | Database user who performs queries          |
   +--------------+-----------+-----------+---------------------------------------------+
   | table_name   | Text      | N/A       | Name of a foreign table on OBS              |
   +--------------+-----------+-----------+---------------------------------------------+
   | file_type    | Text      | N/A       | Format of files storing the underlying data |
   +--------------+-----------+-----------+---------------------------------------------+
   | time_stamp   | time_stam | N/A       | Scanning start time                         |
   +--------------+-----------+-----------+---------------------------------------------+
   | actual_time  | Double    | N/A       | Scanning execution time, in seconds         |
   +--------------+-----------+-----------+---------------------------------------------+
   | file_scanned | Bigint    | N/A       | Number of files scanned                     |
   +--------------+-----------+-----------+---------------------------------------------+
   | data_size    | Double    | N/A       | Size of data scanned, in bytes              |
   +--------------+-----------+-----------+---------------------------------------------+
   | billing_info | Text      | N/A       | Reserved column                             |
   +--------------+-----------+-----------+---------------------------------------------+
