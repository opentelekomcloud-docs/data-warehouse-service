:original_name: dws_06_0418.html

.. _dws_06_0418:

Comparing Strings
=================

STRCMP(text, text)
------------------

Description: Compares two strings. If all strings are the same, **0** is returned. If the first string is less than the second string according to the current classification order, **-1** is returned. Otherwise, **1** is returned. If the parameter contains NULL, NULL will be returned.

Return type: text

Example:

::

   SELECT STRCMP('AA', 'AA'), STRCMP('AA', 'AB'), STRCMP('AA', 'A');
   STRCMP  |  STRCMP  |  STRCMP
   ------------------------------
       0   |    -1    |     1
   (1 row)
