:original_name: dws_04_0481.html

.. _dws_04_0481:

Case: Adjusting the Partial Clustering Key
==========================================

Partial Cluster Key (PCK) is an index technology that uses min/max indexes to quickly scan base tables in column storage. Partial cluster key can specify multiple columns, but you are advised to specify no more than two columns. It can be used to accelerated queries on large column-store tables.

Before Optimization
-------------------

Create a column-store table **orders_no_pck** without partial clustering (PCK). The table is defined as follows:

|image1|

Run the following SQL statement to query the execution plan of a point query:

::

   EXPLAIN PERFORMANCE
   SELECT * FROM orders_no_pck
   WHERE o_orderkey = '13095143'
   ORDER BY o_orderdate;

As shown in the following figure, the execution time is 48 ms. Check **Datanode Information**. It is found that the filter time is 19 ms and the CUNone ratio is 0.

|image2|

|image3|

After Optimization
------------------

The created column-store table **orders_pck** is defined as follows:

|image4|

Use **ALTER TABLE** to set the **o_orderkey** field to **PCK**:

|image5|

Run the following SQL statement to query the execution plan of the same point query SQL statement again:

::

   EXPLAIN PERFORMANCE
   SELECT * FROM orders_pck
   WHERE o_orderkey = '13095143'
   ORDER BY o_orderdate;

As shown in the following figure, the execution time is 5 ms. Check **Datanode Information**. It is found that the filter time is 0.5 ms and the CUNone ratio is 82. The higher the CUNone ratio, the higher performance that the PCK will bring.

|image6|

|image7|

.. |image1| image:: /_static/images/en-us_image_0000001598442429.png
.. |image2| image:: /_static/images/en-us_image_0000001547757780.png
.. |image3| image:: /_static/images/en-us_image_0000001598557569.png
.. |image4| image:: /_static/images/en-us_image_0000001598641405.png
.. |image5| image:: /_static/images/en-us_image_0000001547777474.png
.. |image6| image:: /_static/images/en-us_image_0000001598622253.png
.. |image7| image:: /_static/images/en-us_image_0000001547437736.png
