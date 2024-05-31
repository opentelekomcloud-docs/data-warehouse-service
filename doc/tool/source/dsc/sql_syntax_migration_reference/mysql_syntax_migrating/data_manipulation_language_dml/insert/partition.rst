:original_name: dws_16_0187.html

.. _dws_16_0187:

.. _en-us_topic_0000001772536556:

PARTITION
=========

When inserting into a partitioned table, you can control which partitions and subpartitions accept new rows.

**Input**

.. code-block::

   INSERT INTO employees PARTITION(p3) VALUES (19, 'Frank1', 'Williams', 1, 2);
   INSERT INTO employees PARTITION(p0) VALUES (4, 'Frank1', 'Williams', 1, 2);
   INSERT INTO employees PARTITION(p1) VALUES (9, 'Frank1', 'Williams', 1, 2);
   INSERT INTO employees PARTITION(p2) VALUES (10, 'Frank1', 'Williams', 1, 2);
   INSERT INTO employees PARTITION(p2) VALUES (11, 'Frank1', 'Williams', 1, 2);

**Output**

.. code-block::

   INSERT INTO "public"."employees"  VALUES (19,'Frank1','Williams',1,2);
   INSERT INTO "public"."employees"  VALUES (4,'Frank1','Williams',1,2);
   INSERT INTO "public"."employees"  VALUES (9,'Frank1','Williams',1,2);
   INSERT INTO "public"."employees"  VALUES (10,'Frank1','Williams',1,2);
   INSERT INTO "public"."employees"  VALUES (11,'Frank1','Williams',1,2);
