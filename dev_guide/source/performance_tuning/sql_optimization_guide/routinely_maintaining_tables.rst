:original_name: dws_04_0466.html

.. _dws_04_0466:

Routinely Maintaining Tables
============================

To ensure proper database running, after INSERT and DELETE operations, you need to routinely do **VACUUM FULL** and **ANALYZE** as appropriate for customer scenarios and update statistics to obtain better performance.

Related Concepts
----------------

You need to routinely run **VACUUM**, **VACUUM FULL**, and **ANALYZE** to maintain tables, because:

-  **VACUUM FULL** reclaims disk space occupied by updated or deleted data and combines small-size data files.
-  **VACUUM** maintains a visualized mapping to track pages that contain arrays visible to other active transactions. A common index scan uses the mapping to obtain the corresponding array and check whether pages are visible to the current transaction. If the array cannot be obtained, the visibility is checked by fetching stack arrays. Therefore, updating the visible mapping of a table can accelerate unique index scans.
-  **VACUUM** can avoid old data loss caused by duplicate transaction IDs when the number of executed transactions exceeds the database threshold.
-  **ANALYZE** collects statistics on tables in databases. The statistics are stored in the PG_STATISTIC system catalog. Then, the query optimizer uses the statistics to work out the most efficient execution plan.

Procedure
---------

#. Run the **VACUUM** or **VACUUM FULL** command to reclaim disk space.

   -  **VACUUM**:

      Do **VACUUM** to the table:

      .. code-block::

         VACUUM customer;

      .. code-block::

         VACUUM

      This command can be concurrently executed with database operation commands, including **SELECT**, **INSERT**, **UPDATE**, and **DELETE**; excluding **ALTER TABLE**.

      Do **VACUUM** to the partitioned table:

      .. code-block::

         VACUUM customer_par PARTITION ( P1 );

      .. code-block::

         VACUUM

   -  VACUUM FULL:

      .. code-block::

         VACUUM FULL customer;

      .. code-block::

         VACUUM

      **VACUUM FULL** needs to add exclusive locks on tables it operates on and requires that all other database operations be suspended.

      When reclaiming disk space, you can query for the session corresponding to the earliest transactions in the cluster, and then end the earliest long transactions as needed to make full use of the disk space.

      a. Run the following command to query for oldestxmin on the GTM:

         .. code-block::

            select * from pgxc_gtm_snapshot_status();

      b. Run the following command to query for the PID of the corresponding session on the CN. *xmin* is the oldestxmin obtained in the previous step.

         .. code-block::

            select * from pgxc_running_xacts() where xmin=1400202010;

#. Do **ANALYZE** to update statistical information.

   .. code-block::

      ANALYZE customer;

   .. code-block::

      ANALYZE

   Do **ANALYZE VERBOSE** to update statistics and display table information.

   .. code-block::

      ANALYZE VERBOSE customer;

   .. code-block::

      ANALYZE

   You can use **VACUUM ANALYZE** at the same time to optimize the query.

   .. code-block::

      VACUUM ANALYZE customer;

   .. code-block::

      VACUUM

   .. note::

      **VACUUM** and **ANALYZE** cause a substantial increase in I/O traffic, which may cause poor performance of other active sessions. Therefore, you are advised to set by specifying the **vacuum_cost_delay** parameter.

#. Delete a table

   .. code-block::

      DROP TABLE customer;
      DROP TABLE customer_par;
      DROP TABLE part;

   If the following output is displayed, the index has been deleted.

   .. code-block::

      DROP TABLE

Maintenance Suggestion
----------------------

-  Routinely do **VACUUM FULL** to large tables. If the database performance deteriorates, do **VACUUM FULL** to the entire database. If the database performance is stable, you are advised to monthly do **VACUUM FULL**.
-  Routinely do **VACUUM FULL** to system catalogs, mainly **PG_ATTRIBUTE**.
-  The automatic vacuum process (**AUTOVACUUM**) in the system automatically runs the **VACUUM** and **ANALYZE** statements to reclaim the record space marked as the deleted state and to update statistics related to the table.
