:original_name: dws_04_0695.html

.. _dws_04_0695:

GS_SQL_COUNT
============

**GS_SQL_COUNT** displays statistics about the five types of statements (**SELECT**, **INSERT**, **UPDATE**, **DELETE**, and **MERGE INTO**) executed on the current node of the database, including the number of execution times, response time (the maximum, minimum, average, and total response time of the other four types of statements except the **MERGE INTO** statement, in microseconds), and the number of execution times of **DDL**, **DML**, and **DCL statements**.

The classification of **DDL**, **DML**, and **DCL** statements in the **GS_SQL_COUNT** view is slightly different from that of the SQL syntaxt. The details are as follows:

-  User-related statements, such as **CREATE**/**ALTER**/**DROP USER** and **CREATE**/**ALTER**/**DROP ROLE**, are of the DCL type.
-  Transaction-related statements such as **BEGIN**/**COMMIT**/**SET CONSTRAINTS**/**ROLLBACK**/**SAVEPOINT**/**START** are of the DCL type.
-  **ALTER SYSTEM KILL SESSION** is equivalent to the **SELECT pg_terminate_backend()** statement and is of the DML type.

The classification of other statements is similar to the definition in the SQL syntax.

When a common user queries the **GS_SQL_COUNT** view, only the statistics of this user in the current node can be viewed. When a user with the administrator permissions queries the **GS_SQL_COUNT** view, the statistics of all users in the current node can be viewed. When the cluster or the node is restarted, the statistics are cleared and the counting restarts. The counting is based on the number of queries received by the node, including the queries performed inside the cluster. Statistics about the **GS_SQL_COUNT** view are collected only on CNs, and SQL statements sent from other CNs are not collected. No result is returned when you query the view on a DN.

.. _en-us_topic_0000001145814311__t8f0334486f934453827d563b90c86711:

.. table:: **Table 1** GS_SQL_COUNT columns

   +---------------------+--------+------------------------------------------------+
   | Name                | Type   | Description                                    |
   +=====================+========+================================================+
   | node_name           | name   | Node name                                      |
   +---------------------+--------+------------------------------------------------+
   | user_name           | name   | User name                                      |
   +---------------------+--------+------------------------------------------------+
   | select_count        | bigint | Number of **SELECT** statements                |
   +---------------------+--------+------------------------------------------------+
   | update_count        | bigint | Number of **UPDATE** statements                |
   +---------------------+--------+------------------------------------------------+
   | insert_count        | bigint | Number of **INSERT** statements                |
   +---------------------+--------+------------------------------------------------+
   | delete_count        | bigint | Number of **DELETE** statements                |
   +---------------------+--------+------------------------------------------------+
   | mergeinto_count     | bigint | Number of **MERGE INTO** statements            |
   +---------------------+--------+------------------------------------------------+
   | ddl_count           | bigint | Number of **DDL** statements                   |
   +---------------------+--------+------------------------------------------------+
   | dml_count           | bigint | Number of **DML** statements                   |
   +---------------------+--------+------------------------------------------------+
   | dcl_count           | bigint | Number of **DCL** statements                   |
   +---------------------+--------+------------------------------------------------+
   | total_select_elapse | bigint | Total response time of **SELECT** statements   |
   +---------------------+--------+------------------------------------------------+
   | avg_select_elapse   | bigint | Average response time of **SELECT** statements |
   +---------------------+--------+------------------------------------------------+
   | max_select_elapse   | bigint | Maximum response time of **SELECT** statements |
   +---------------------+--------+------------------------------------------------+
   | min_select_elapse   | bigint | Minimum response time of **SELECT** statements |
   +---------------------+--------+------------------------------------------------+
   | total_update_elapse | bigint | Total response time of **UPDATE** statements   |
   +---------------------+--------+------------------------------------------------+
   | avg_update_elapse   | bigint | Average response time of **UPDATE** statements |
   +---------------------+--------+------------------------------------------------+
   | max_update_elapse   | bigint | Maximum response time of **UPDATE** statements |
   +---------------------+--------+------------------------------------------------+
   | min_update_elapse   | bigint | Minimum response time of **UPDATE** statements |
   +---------------------+--------+------------------------------------------------+
   | total_delete_elapse | bigint | Total response time of **DELETE** statements   |
   +---------------------+--------+------------------------------------------------+
   | avg_delete_elapse   | bigint | Average response time of **DELETE** statements |
   +---------------------+--------+------------------------------------------------+
   | max_delete_elapse   | bigint | Maximum response time of **DELETE** statements |
   +---------------------+--------+------------------------------------------------+
   | min_delete_elapse   | bigint | Minimum response time of **DELETE** statements |
   +---------------------+--------+------------------------------------------------+
   | total_insert_elapse | bigint | Total response time of **INSERT** statements   |
   +---------------------+--------+------------------------------------------------+
   | avg_insert_elapse   | bigint | Average response time of **INSERT** statements |
   +---------------------+--------+------------------------------------------------+
   | max_insert_elapse   | bigint | Maximum response time of **INSERT** statements |
   +---------------------+--------+------------------------------------------------+
   | min_insert_elapse   | bigint | Minimum response time of **INSERT** statements |
   +---------------------+--------+------------------------------------------------+
