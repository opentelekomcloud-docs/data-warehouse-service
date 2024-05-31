:original_name: dws_16_0197.html

.. _dws_16_0197:

.. _en-us_topic_0000001819336253:

IGNORE
======

With the **IGNORE** modifier, the **UPDATE** statement does not abort even if errors occur during execution.

**Input**

.. code-block::

   # IGNORE
   UPDATE IGNORE employees SET department_id=3;

**Output**

.. code-block::

   -- IGNORE
   UPDATE "public"."employees" SET "department_id" = 3;
