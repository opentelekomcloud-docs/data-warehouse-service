:original_name: dws_06_0281.html

.. _dws_06_0281:

CREATE EXCEPT RULE
==================

Function
--------

Creates an exception rule set. When creating an exception rule, you can specify the rule thresholds and operations following the triggering of an exception rule. Currently, only the **abort** operation is supported.

Precautions
-----------

None

Syntax
------

::

   CREATE EXCEPT RULE except_rule_name
          WITH (
               | BLOCKTIME = VALUE,
               | CPUTIME = VALUE,
               | ELAPSEDTIME = VALUE,
               | CPUSKEWPERCENT = VALUE,
               | SPILLSIZE = VALUE,
               | BROADCASTSIZE = VALUE,
               | MEMSIZE = VALUE,
               | CPUAVGPERCENT = VALUE,
               | BANDWIDTH = VALUE,
               | ACTION = ['abort' | 'penalty']
               );

Parameter Description
---------------------

-  **rule_name**

   Name of an exception rule set.

   Value range: a string of 1 to 64 characters. It must comply with the naming convention.

-  **blocktime**

   Maximum duration of job queue blocking, in seconds.

   Value range: -1, 1~INT64_MAX

-  **elapsedtime**

   Maximum job execution duration, in seconds.

   Value range: -1, 1~INT64_MAX

-  **allcputime**

   Maximum CPU time used during job running. The unit is second.

   Value range: -1, 1~INT64_MAX

-  **cpuskewpercent**

   Average CPU usage during job execution. The unit is percentage.

   Value range: -1, 1-100.

-  **cpuavgpercent**

   CPU usage skew during job execution. The unit is percentage.

   Value range: -1, 1-100.

-  **spillsize**

   Maximum size of data spilled to disks during job execution. The unit is MB.

   Value range: -1, 1~INT64_MAX

-  **broadcastsize**

   Maximum broadcast size of a job. The unit is MB.

   Value range: -1, 1~INT64_MAX

-  **memsize**

   Maximum memory size used for job execution. Unit: MB.

   Value range: -1, 1~INT64_MAX

-  bandwidth

   Maximum bandwidth that can be used for job execution. Unit: MB.

   Value range: -1, 1~INT64_MAX

Examples
--------

Create exception rule set **except_rule1** and set the **blocktime** threshold to **3000 seconds**, and spilling space to **4000 MB**.

::

   CREATE EXCEPT RULE except_rule1 WITH (blocktime=3000, spillsize=4000, action=abort);

Create an exception rule set **except_rule2** and set the **memsize** threshold to **5000 MB**. The default operation following an exception is **abort**.

::

   CREATE EXCEPT RULE except_rule2 WITH (memsize=3000);

Create a resource pool and bind it to exception rule set **except_rule3**.

::

   CREATE resource pool resource_pool_a1 WITH (except_rule='except_rule3');

Helpful Links
-------------

:ref:`ALTER EXCEPT RULE <dws_06_0280>`, :ref:`DROP EXCEPT RULE <dws_06_0282>`
