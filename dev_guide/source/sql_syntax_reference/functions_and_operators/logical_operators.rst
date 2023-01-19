:original_name: dws_06_0028.html

.. _dws_06_0028:

Logical Operators
=================

The usual logical operators include AND, OR, and NOT. SQL uses a three-valued logical system with true, false, and null, which represents "unknown". Their priorities are NOT > AND > OR.

:ref:`Table 1 <en-us_topic_0000001145830677__tddad89fc8a334725a6d9b2dca1969309>` lists operation rules, where a and b represent logical expressions.

.. _en-us_topic_0000001145830677__tddad89fc8a334725a6d9b2dca1969309:

.. table:: **Table 1** Operation rules

   ===== ===== ============== ============== ============
   a     b     a AND b Result a OR b Result  NOT a Result
   ===== ===== ============== ============== ============
   TRUE  TRUE  TRUE           TRUE           FALSE
   TRUE  FALSE FALSE          TRUE           FALSE
   TRUE  NULL  NULL           TRUE           FALSE
   FALSE FALSE FALSE          FALSE          TRUE
   FALSE NULL  FALSE          NULL           TRUE
   NULL  NULL  NULL           NULL           NULL
   ===== ===== ============== ============== ============

.. note::

   The operators AND and OR are commutative, that is, you can switch the left and right operand without affecting the result.
