:original_name: dws_03_2106.html

.. _dws_03_2106:

What Is a GaussDB(DWS) Schema Starting with pg_toast_temp\* or pg_temp*?
========================================================================

When you query the schema list, the query result may contain schemas starting with **pg_temp\*** or **pg_toast_temp\***, as shown in the following figure.

::

   SELECT * FROM pg_namespace;

|image1|

These schemas are created when temporary tables are created. Each session has an independent schema starting with **pg_temp** to ensure that the temporary tables are visible only to the current session. Therefore, you are not advised to manually delete schemas starting with **pg_temp** or **pg_toast_temp** during routine operations.

Temporary tables are visible only in the current session and are automatically deleted after the session ends. The corresponding schemas are also deleted.

.. |image1| image:: /_static/images/en-us_image_0000001414084876.png
