:original_name: dws_06_0304.html

.. _dws_06_0304:

Partition Management Function
=============================

.. _en-us_topic_0000001444998754__section9462151915274:

proc_add_partition (relname regclass, boundaries_interval interval)
-------------------------------------------------------------------

Description: Adds partitions to a table with the automatic partition creation function enabled.

Return type: void

Note: When the function is executed, multiple partitions with time range as **boundaries_interval** are created based on the existing partition boundary until **new_part_boundary - now_time >= 29 \* boundaries_interval**. Then, an extra partition is created to ensure that at least a new partition is created when the function is executed.

Example:

::

   call proc_add_partition('my_schema.my_table', interval '1 day');
    proc_add_partition
   --------------------

   (1 row)

.. _en-us_topic_0000001444998754__section9128833152714:

proc_drop_partition (relname regclass, older_than interval)
-----------------------------------------------------------

Description: Deletes partitions from a table with the automatic partition deletion function enabled.

Return type: void

Note: When this function is executed, all partitions in the table are traversed and the partitions whose boundary is smaller than **now_time - older_than** are deleted. If all partitions meet the deletion condition, the table is truncated with one partition kept.

Example:

::

   call proc_drop_partition('my_schema.my_table', interval '7 day');
    proc_drop_partition
   --------------------

   (1 row)
