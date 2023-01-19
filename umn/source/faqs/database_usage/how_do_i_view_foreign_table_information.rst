:original_name: dws_03_2124.html

.. _dws_03_2124:

How Do I View Foreign Table Information?
========================================

To query information about OBS/GDS foreign tables such as OBS paths, run the following statement:

::

   select * from pg_get_tabledef('Foreign_table_name')

The following uses table **traffic_data.GCJL_OBS** as an example:

::

   select * from pg_get_tabledef('traffic_data.GCJL_OBS');

|image1|

.. |image1| image:: /_static/images/en-us_image_0000001165989868.png
