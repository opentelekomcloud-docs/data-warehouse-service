:original_name: dws_mt_0111.html

.. _dws_mt_0111:

Views
=====

A view is a logical table based on one or more tables or views. A view itself contains no data.

In the source file, if the table names are not qualified with the schema name, then the target file is modified such that the table is also qualified with the same schema name as that of the view.

The following is an example of the syntax before and after view migration.

Table Name (Without Schema Name)
--------------------------------


.. figure:: /_static/images/en-us_image_0000001658025090.png
   :alt: **Figure 1** Input: tab1 and tab2

   **Figure 1** Input: tab1 and tab2

Table Name (With Schema Name)
-----------------------------


.. figure:: /_static/images/en-us_image_0000001706224445.png
   :alt: **Figure 2** Output: view schema names

   **Figure 2** Output: view schema names
