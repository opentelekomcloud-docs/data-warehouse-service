:original_name: dws_03_2124.html

.. _dws_03_2124:

How Do I View GaussDB(DWS) Foreign Table Information?
=====================================================

To query information about OBS/GDS foreign tables such as OBS paths, run the following statement:

::

   SELECT * FROM pg_get_tabledef ('foreign_table_name')

The following uses table **traffic_data.GCJL_OBS** as an example:

::

   SELECT * FROM pg_get_tabledef('traffic_data.GCJL_OBS');

|image1|

.. |image1| image:: /_static/images/en-us_image_0000001381609461.png
