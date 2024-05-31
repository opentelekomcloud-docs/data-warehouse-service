:original_name: dws_16_0195.html

.. _dws_16_0195:

.. _en-us_topic_0000001772536564:

ORDER BY
========

In MySQL, if an **UPDATE** statement includes an **ORDER BY** clause, the rows will be updated in the order specified by the clause.

**Input**

.. code-block::

   # ORDER BY
   UPDATE  employees SET department_id=department_id+1  ORDER BY id;

**Output**

.. code-block::

   -- ORDER BY
   UPDATE "public"."employees" SET "department_id" = department_id+1;
