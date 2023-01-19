:original_name: dws_04_0483.html

.. _dws_04_0483:

Case: Adjusting the Local Clustering Column
===========================================

Symptom
-------

During the test at a site, if the following execution plan is performed, the customer expects that the performance can be improved and the result can be returned within 3s.

|image1|

Optimization Analysis
---------------------

The analysis shows that the performance bottleneck of this plan is lfbank. f_ev_dp_kdpl_zhminx. The scan condition of this table is as follows:

|image2|

Try to modify the lfbank. f_ev_dp_kdpl_zhmin table to a column-store table, and then create the PCK (local clustering) in the **yezdminc** column, and set **PARTIAL_CLUSTER_ROWS** to **100000000**. The execution plan after optimization is as follows:

|image3|

.. note::

   -  This method actually sacrifices the performance during data import to improve the query performance.
   -  The number of local sorting tuples is increased, and you need to increase the value of **psort_work_mem** to improve the sorting efficiency.

.. |image1| image:: /_static/images/en-us_image_0000001098815218.png
.. |image2| image:: /_static/images/en-us_image_0000001145695149.png
.. |image3| image:: /_static/images/en-us_image_0000001145815079.png
