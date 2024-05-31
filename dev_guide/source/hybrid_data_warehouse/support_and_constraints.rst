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

#. Currently, HStore and column storage do not support the use of VACUUM to clear dirty index data, and frequent updates may cause index bloat. This function will be supported in later versions.
