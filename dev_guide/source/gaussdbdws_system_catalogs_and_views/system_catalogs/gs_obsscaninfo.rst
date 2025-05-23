:original_name: dws_04_0562.html

.. _dws_04_0562:

GS_OBSSCANINFO
==============

**GS_OBSSCANINFO** defines the OBS runtime information scanned in cluster acceleration scenarios. Each record corresponds to a piece of runtime information of a foreign table on OBS in a query.

.. table:: **Table 1** GS_OBSSCANINFO columns

   +--------------+-----------+-----------+------------------------------------------------------------+
   | Name         | Type      | Reference | Description                                                |
   +==============+===========+===========+============================================================+
   | query_id     | Bigint    | ``-``     | Specifies a query ID.                                      |
   +--------------+-----------+-----------+------------------------------------------------------------+
   | user_id      | Text      | ``-``     | Specifies a database user who performs queries.            |
   +--------------+-----------+-----------+------------------------------------------------------------+
   | table_name   | Text      | ``-``     | Specifies the name of a foreign table on OBS.              |
   +--------------+-----------+-----------+------------------------------------------------------------+
   | file_type    | Text      | ``-``     | Specifies the format of files storing the underlying data. |
   +--------------+-----------+-----------+------------------------------------------------------------+
   | time_stamp   | time_stam | ``-``     | Specifies the scanning start time.                         |
   +--------------+-----------+-----------+------------------------------------------------------------+
   | actual_time  | double    | ``-``     | Specifies the scanning execution time in seconds.          |
   +--------------+-----------+-----------+------------------------------------------------------------+
   | file_scanned | Bigint    | ``-``     | Specifies the number of files scanned.                     |
   +--------------+-----------+-----------+------------------------------------------------------------+
   | data_size    | double    | ``-``     | Specifies the size of data scanned in bytes.               |
   +--------------+-----------+-----------+------------------------------------------------------------+
   | billing_info | Text      | ``-``     | Specifies the reserved fields.                             |
   +--------------+-----------+-----------+------------------------------------------------------------+
