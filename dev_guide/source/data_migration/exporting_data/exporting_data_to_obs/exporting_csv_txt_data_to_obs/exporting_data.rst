:original_name: dws_04_0254.html

.. _dws_04_0254:

.. _en-us_topic_0000001717097360:

Exporting Data
==============

Syntax
------

**Run the following command to export data:**

::

   INSERT INTO [Foreign table name] SELECT * FROM [Source table name];

Examples
--------

-  **Example 1**: Export data from table **product_info_output** to a data file through the **product_info_output_ext** foreign table.

   ::

      INSERT INTO product_info_output_ext SELECT * FROM product_info_output;

   If information similar to the following is displayed, the data has been exported.

   ::

      INSERT 0 10

-  **Example 2**: Export part of the data to a data file by specifying the filter condition **WHERE** *product\_price>500*.

   ::

      INSERT INTO product_info_output_ext SELECT * FROM product_info_output WHERE product_price>500;

.. note::

   -  The directory to be used for data storage must be empty, or the export will fail.
   -  Data of a special type, such as RAW, is exported as a binary file, which cannot be recognized by the import tool. You need to use the RAWTOHEX() function to convert it to the hexadecimal format before export.
