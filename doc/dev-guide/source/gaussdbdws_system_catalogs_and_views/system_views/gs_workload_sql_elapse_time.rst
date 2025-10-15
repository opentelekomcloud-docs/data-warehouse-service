:original_name: dws_04_0710.html

.. _dws_04_0710:

GS_WORKLOAD_SQL_ELAPSE_TIME
===========================

**GS_WORKLOAD_SQL_ELAPSE_TIME** displays statistics on the response time of SQL statements in workload Cgroups on the current node, including the maximum, minimum, average, and total response time of **SELECT**, **UPDATE**, **INSERT**, and **DELETE** statements. The unit is microsecond.

.. table:: **Table 1** **GS_WORKLOAD_SQL_ELAPSE_TIME** columns

   +---------------------+--------+------------------------------------------------+
   | Column              | Type   | Description                                    |
   +=====================+========+================================================+
   | workload            | Name   | Workload Cgroup name                           |
   +---------------------+--------+------------------------------------------------+
   | total_select_elapse | Bigint | Total response time of **SELECT** statements   |
   +---------------------+--------+------------------------------------------------+
   | max_select_elapse   | Bigint | Maximum response time of **SELECT** statements |
   +---------------------+--------+------------------------------------------------+
   | min_select_elapse   | Bigint | Minimum response time of **SELECT** statements |
   +---------------------+--------+------------------------------------------------+
   | avg_select_elapse   | Bigint | Average response time of **SELECT** statements |
   +---------------------+--------+------------------------------------------------+
   | total_update_elapse | Bigint | Total response time of **UPDATE** statements   |
   +---------------------+--------+------------------------------------------------+
   | max_update_elapse   | Bigint | Maximum response time of **UPDATE** statements |
   +---------------------+--------+------------------------------------------------+
   | min_update_elapse   | Bigint | Minimum response time of **UPDATE** statements |
   +---------------------+--------+------------------------------------------------+
   | avg_update_elapse   | Bigint | Average response time of **UPDATE** statements |
   +---------------------+--------+------------------------------------------------+
   | total_insert_elapse | Bigint | Total response time of **INSERT** statements   |
   +---------------------+--------+------------------------------------------------+
   | max_insert_elapse   | Bigint | Maximum response time of **INSERT** statements |
   +---------------------+--------+------------------------------------------------+
   | min_insert_elapse   | Bigint | Minimum response time of **INSERT** statements |
   +---------------------+--------+------------------------------------------------+
   | avg_insert_elapse   | Bigint | Average response time of **INSERT** statements |
   +---------------------+--------+------------------------------------------------+
   | total_delete_elapse | Bigint | Total response time of **DELETE** statements   |
   +---------------------+--------+------------------------------------------------+
   | max_delete_elapse   | Bigint | Maximum response time of **DELETE** statements |
   +---------------------+--------+------------------------------------------------+
   | min_delete_elapse   | Bigint | Minimum response time of **DELETE** statements |
   +---------------------+--------+------------------------------------------------+
   | avg_delete_elapse   | Bigint | Average response time of **DELETE** statements |
   +---------------------+--------+------------------------------------------------+
