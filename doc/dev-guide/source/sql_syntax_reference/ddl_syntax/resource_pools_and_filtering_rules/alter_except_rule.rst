:original_name: dws_06_0280.html

.. _dws_06_0280:

ALTER EXCEPT RULE
=================

Function
--------

This syntax modifies an exception rule set. You can modify one or more specific rule thresholds in a rule set.

Precautions
-----------

When **ALTER** is used to add a new rule threshold, the original rules in the **WITH** parameter are retained and are not cleared. For example, if a rule set contains only one rule, that is, the maximum duration of job queue blocking (**blocktime**) is 100 seconds, and you want to add another rule, that is, the maximum job execution time (**elapsedtime**) is 1000 seconds, the rule set will contain two rules. For details, see the following example.

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

.. table:: **Table 1** ALTER EXCEPT RULE parameters

   +----------------+----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | Parameter      | Description                                                          | Value Range                                                                                                      |
   +================+======================================================================+==================================================================================================================+
   | rule_name      | Name of an exception rule set.                                       | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`. |
   +----------------+----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | blocktime      | Maximum duration of job queue blocking, in seconds.                  | Numeric type. The value can be **-1** or ranges from **1** to **INT64_MAX**.                                     |
   +----------------+----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | elapsedtime    | Maximum job execution duration, in seconds.                          | Numeric type. The value can be **-1** or ranges from **1** to **INT64_MAX**.                                     |
   +----------------+----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | allcputime     | Maximum CPU time used during job running, in seconds.                | Numeric type. The value can be **-1** or ranges from **1** to **INT64_MAX**.                                     |
   +----------------+----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | cpuskewpercent | CPU usage skew during job execution, in percentage.                  | Numeric type. The value can be **-1** or ranges from **1** to **100**.                                           |
   +----------------+----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | cpuavgpercent  | Average CPU usage during job execution, in percentage.               | Numeric type. The value can be **-1** or ranges from **1** to **100**.                                           |
   +----------------+----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | spillsize      | Maximum size of data spilled to disks during job execution, in MB.   | Numeric type. The value can be **-1** or ranges from **1** to **INT64_MAX**.                                     |
   +----------------+----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | broadcastsize  | Maximum broadcast size of a job, in MB.                              | Numeric type. The value can be **-1** or ranges from **1** to **INT64_MAX**.                                     |
   +----------------+----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | memsize        | Maximum memory size used for job execution, in MB.                   | Numeric type. The value can be **-1** or ranges from **1** to **INT64_MAX**.                                     |
   +----------------+----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | bandwidth      | Maximum network bandwidth that can be used for job execution, in MB. | Numeric type. The value can be **-1** or ranges from **1** to **INT64_MAX**.                                     |
   +----------------+----------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+

Examples
--------

Create an exception rule **except_rule1**.

::

   CREATE EXCEPT RULE except_rule_1 WITH (blocktime=2000, spillsize=3000, action=abort);

Change the blocktime threshold of exception rule set **except_rule1** to **3000s** and the space for spilled data to **4000 MB**.

::

   ALTER EXCEPT RULE except_rule_1 WITH (blocktime=3000, spillsize=4000);

Check whether the exception rule is modified successfully by querying the **PG_EXCEPT_RULE** system catalog.

::

   SELECT * FROM PG_EXCEPT_RULE WHERE name = 'except_rule_1';

|image1|

Add a new rule, that is, the maximum job execution time (**elapsedtime**) is 2000 seconds.

::

   ALTER EXCEPT RULE except_rule_1 WITH (elapsedtime=2000);

Check the exception rule again. The rule is modified successfully. **Note that the original rule is retained instead of being cleared.**

::

   SELECT * FROM PG_EXCEPT_RULE WHERE name = 'except_rule_1';

|image2|

Create a resource pool and bind it to exception rule set **except_rule_1**.

::

   DROP resource pool IF EXISTS resource_pool_a1;
   CREATE resource pool resource_pool_a1 WITH (except_rule='except_rule_1');

Unbind the exception rule set from the resource pool **resource_pool_a1**.

::

   ALTER resource pool resource_pool_a1 WITH (except_rule='None');

Helpful Links
-------------

:ref:`CREATE EXCEPT RULE <dws_06_0281>` and :ref:`DROP EXCEPT RULE <dws_06_0282>`

.. |image1| image:: /_static/images/en-us_image_0000002471319732.png
.. |image2| image:: /_static/images/en-us_image_0000002471320058.png
