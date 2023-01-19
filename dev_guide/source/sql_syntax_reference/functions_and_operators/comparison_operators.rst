:original_name: dws_06_0029.html

.. _dws_06_0029:

Comparison Operators
====================

Comparison operators are available for all data types and return Boolean values.

All comparison operators are binary operators. Only data types that are the same or can be implicitly converted can be compared using comparison operators.

:ref:`Table 1 <en-us_topic_0000001098830674__t6a435675959f484a9b0194cd232c098c>` describes comparison operators provided by GaussDB(DWS).

.. _en-us_topic_0000001098830674__t6a435675959f484a9b0194cd232c098c:

.. table:: **Table 1** Comparison operators

   ========= ========================
   Operators Description
   ========= ========================
   <         Less than
   >         Greater than
   <=        Less than or equal to
   >=        Greater than or equal to
   =         Equality
   <> or !=  Inequality
   ========= ========================

Comparison operators are available for all relevant data types. All comparison operators are binary operators that returned values of Boolean type. Expressions like **1 < 2 < 3** are invalid. (Because there is no comparison operator to compare a Boolean value with **3**.)
