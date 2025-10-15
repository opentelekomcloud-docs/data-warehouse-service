:original_name: dws_06_0280.html

.. _dws_06_0280:

ALTER EXCEPT RULE
=================

Function
--------

This syntax is used to modify an exception rule set. You can modify one or more specific rule thresholds in a rule set.

Precautions
-----------

None

Syntax
------

::

   ALTER EXCEPT RULE except_rule_name
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

   Value range: a string. It must comply with the naming convention.

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

   CPU usage skew during job execution. The unit is percentage.

   Value range: -1, 1-100.

-  **cpuavgpercent**

   Average CPU usage during job execution. The unit is percentage.

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

-  **bandwidth**

   Maximum network bandwidth that can be used for job execution. Unit: MB.

   Value range: -1, 1~INT64_MAX

Examples
--------

Change the blocktime threshold of exception rule set **except_rule1** to **3000s** and the space for spilled data to **4000 MB**.

::

   ALTER EXCEPT RULE except_rule1 WITH (blocktime=3000, spillsize=4000);

Change the spilled data size **spillsize** to **5000 MB** for the exception rule set **except_rule2**.

::

   ALTER EXCEPT RULE except_rule2 WITH (spillsize=5000);

Change the exception rule set bound to resource pool **resource_pool_a1** to **except_rule3**.

::

   ALTER resource pool resource_pool_a1 WITH (except_rule='except_rule3');

Unbind the exception rule set from the resource pool **resource_pool_a1**.

::

   ALTER resource pool resource_pool_a1 WITH (except_rule='None');

Helpful Links
-------------

:ref:`CREATE EXCEPT RULE <dws_06_0281>`, :ref:`DROP EXCEPT RULE <dws_06_0282>`
