:original_name: dws_04_0481.html

.. _dws_04_0481:

Case: Adjusting the Partial Clustering Key
==========================================

Symptom
-------

Information on the EXPLAIN PERFORMANCE at a site is as follows: As shown in the red boxes, two performance bottlenecks are scan operations in a table.

|image1|

Optimization Analysis
---------------------

After further analysis, we found that the filter condition acct_id ='A012709548':: bpchar exists in the two tables.

|image2|

Try to add the partial clustering key in the acct_id column of the two tables, and run the **VACUUM FULL** statement to make the local clustering take effect. The table performance is improved.

.. |image1| image:: /_static/images/en-us_image_0000001145495125.png
.. |image2| image:: /_static/images/en-us_image_0000001099135098.png
