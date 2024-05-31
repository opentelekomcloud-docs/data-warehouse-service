:original_name: dws_16_0194.html

.. _dws_16_0194:

.. _en-us_topic_0000001772696240:

LOW_PRIORITY
============

With the **LOW_PRIORITY** modifier, execution of **UPDATE** is delayed.

**Input**

.. code-block::

   # LOW_PRIORITY
   UPDATE LOW_PRIORITY employees SET department_id=2;

**Output**

.. code-block::

   -- LOW_PRIORITY
   UPDATE "public"."employees" SET "department_id" = 2;
