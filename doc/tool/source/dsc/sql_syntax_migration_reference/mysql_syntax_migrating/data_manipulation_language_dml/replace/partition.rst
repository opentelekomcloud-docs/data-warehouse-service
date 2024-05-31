:original_name: dws_16_0200.html

.. _dws_16_0200:

.. _en-us_topic_0000001819416289:

PARTITION
=========

MySQL REPLACE supports explicit partitioning selection using the PARTITION keyword and a comma-separated name list for partitions, subpartitions, or both.

**Input**

.. code-block::

   replace INTO employees PARTITION(p3) VALUES (19, 'Frank1', 'Williams', 1, 2);
   replace INTO employees PARTITION(p0) VALUES (4, 'Frank1', 'Williams', 1, 2);
   replace INTO employees PARTITION(p1) VALUES (9, 'Frank1', 'Williams', 1, 2);
   replace INTO employees PARTITION(p2) VALUES (10, 'Frank1', 'Williams', 1, 2);
   replace INTO employees PARTITION(p2) VALUES (11, 'Frank1', 'Williams', 1, 2);

**Output**

.. code-block::

   INSERT INTO "public"."employees"  VALUES (19,'Frank1','Williams',1,2);
   INSERT INTO "public"."employees"  VALUES (4,'Frank1','Williams',1,2);
   INSERT INTO "public"."employees"  VALUES (9,'Frank1','Williams',1,2);
   INSERT INTO "public"."employees"  VALUES (10,'Frank1','Williams',1,2);
   INSERT INTO "public"."employees"  VALUES (11,'Frank1','Williams',1,2);
