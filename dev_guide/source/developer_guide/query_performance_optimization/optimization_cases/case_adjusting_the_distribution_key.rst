:original_name: dws_04_0480.html

.. _dws_04_0480:

Case: Adjusting the Distribution Key
====================================

Symptom
-------

During a site test, the information is displayed after **EXPLAIN ANALYZE** is executed:

|image1|

According to the execution information, HashJoin becomes the performance bottleneck of the whole plan. Based on the execution time of HashJoin **[2657.406, 93339.924]**, it can be seen that severe skew occurs on different DNs during the HashJoin operation.

In the memory information (as shown in the following figure), it can be seen that the data skew occurs in the memory usage of each node.

|image2|

Optimization Analysis
---------------------

The preceding two symptoms indicate that this SQL statement has serious computing skew. The further lower-layer analysis on the HashJoin operator shows that serious computing skew **[38.885,2940.983]** occurs in **Seq Scan on s_riskrate_setting**. Based on the description of the Scan, we can infer that the performance problems of this plan lie in data skew occurred in the **s_riskrate_setting** table. Later, it is proved that serious data skew occurred in the **s_riskrate_setting** table. After performance optimization, the execution time is reduced from 94s to 50s.

.. |image1| image:: /_static/images/en-us_image_0000001099135194.png
.. |image2| image:: /_static/images/en-us_image_0000001145895187.png
