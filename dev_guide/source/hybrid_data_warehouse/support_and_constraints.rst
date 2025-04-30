:original_name: dws_04_1025.html

.. _dws_04_1025:

Support and Constraints
=======================

A hybrid data warehouse is compatible with all column-store syntax.

.. table:: **Table 1** Supported syntax

   ============================== =========
   Syntax                         Supported
   ============================== =========
   CREATE TABLE                   Yes
   CREATE TABLE LIKE              Yes
   DROP TABLE                     Yes
   INSERT                         Yes
   COPY                           Yes
   SELECT                         Yes
   TRUNCATE                       Yes
   EXPLAIN                        Yes
   ANALYZE                        Yes
   VACUUM                         Yes
   ALTER TABLE DROP PARTITION     Yes
   ALTER TABLE ADD PARTITION      Yes
   ALTER TABLE SET WITH OPTION    Yes
   ALTER TABLE DROP COLUMN        Yes
   ALTER TABLE ADD COLUMN         Yes
   ALTER TABLE ADD NODELIST       Yes
   ALTER TABLE CHANGE OWNER       Yes
   ALTER TABLE RENAME COLUMN      Yes
   ALTER TABLE TRUNCATE PARTITION Yes
   CREATE INDEX                   Yes
   DROP INDEX                     Yes
   DELETE                         Yes
   Other ALTER TABLE syntax       Yes
   ALTER INDEX                    Yes
   MERGE                          Yes
   SELECT INTO                    Yes
   UPDATE                         Yes
   CREATE TABLE AS                Yes
   ============================== =========

Constraints
-----------

#. To use HStore tables, use the following parameter settings, or the performance of HStore tables will deteriorate significantly:

   **autovacuum_max_workers_hstore=3**, **autovacuum_max_workers=6**, and **autovacuum=true**

#. In version 8.2.1 and later, you can now clear the dirty data from column-store indexes. This is especially beneficial when dealing with frequent data updates and imports into the database. By efficiently managing the index space, it improves both the import and query performance.

#. When using HStore asynchronous sorting, pay attention to the following:

   -  DML operations on certain data may be blocked during asynchronous sorting. The maximum blocking granularity is the row threshold for asynchronous sorting. This function is not recommended for frequent DML operations.
   -  Automatic asynchronous sorting and column-store VACUUM cannot be used together. If the autovacuum process meets the conditions for column-store VACUUM, asynchronous sorting is skipped and will wait for the next trigger. In some cases, column-store VACUUM may be continuously triggered due to a high volume of DML operations, which means asynchronous sorting will never be triggered.
