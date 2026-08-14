:original_name: dws_04_0139.html

.. _dws_04_0139:

Case: Table in TargetList Can Not Be Shipped
============================================

Possible Cause
--------------

The table name or alias is in the targetlist (between SELECT and FROM).

**Solution**: Rewrite the SQL statement to eliminate the direct reference to the table name or alias.

Case 1: Using the Alias t of the Test Table in the targetlist
-------------------------------------------------------------

Original statement

::

   SELECT t, 1 FROM test t;

|image1|

**Rewritten statement**: Convert the table name or alias to the text type, or optimize the SQL statement to remove the table name or alias from the output column.

::

   SELECT t::text, 1 FROM test t;

|image2|

.. |image1| image:: /_static/images/en-us_image_0000002493971718.png
.. |image2| image:: /_static/images/en-us_image_0000002526131815.png
