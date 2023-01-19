:original_name: dws_04_0525.html

.. _dws_04_0525:

Assignment Statement
====================

Syntax
------

:ref:`Figure 1 <en-us_topic_0000001145894577__f62f6427de47d451aad5b1d15cc4af96a>` shows the syntax diagram for assigning a value to a variable.

.. _en-us_topic_0000001145894577__f62f6427de47d451aad5b1d15cc4af96a:

.. figure:: /_static/images/en-us_image_0000001145814991.png
   :alt: **Figure 1** assignment_value::=

   **Figure 1** assignment_value::=

The above syntax diagram is explained as follows:

-  **variable_name** indicates the name of a variable.
-  **value** can be a value or an expression. The type of **value** must be compatible with the type of **variable_name**.

Examples
--------

::

   DECLARE
       emp_id  INTEGER := 7788; --Assignment
   BEGIN
       emp_id := 5; --Assignment
       emp_id := 5*7784;
   END;
   /
