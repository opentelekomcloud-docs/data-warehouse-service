:original_name: dws_04_0484.html

.. _dws_04_0484:

Case: Reconstructing Partition Tables
=====================================

Symptom
-------

In the following simple SQL statements, the performance bottlenecks exist in the scan operation of dwcjk.

|image1|

|image2|

Optimization Analysis
---------------------

Obviously, there are date features in the cjrq field of table data in the service layer, and this meet the features of a partitioned table. Replan the table definition of the dwcjk table. Set the cjrq field as a partition key, and day as an interval unit. Define the partitioned table dwcjk_part. The modified result is as follows, and the performance is nearly doubled.

|image3|

|image4|

.. |image1| image:: /_static/images/en-us_image_0000001098655366.png
.. |image2| image:: /_static/images/en-us_image_0000001099135172.png
.. |image3| image:: /_static/images/en-us_image_0000001145495195.png
.. |image4| image:: /_static/images/en-us_image_0000001098815188.png
