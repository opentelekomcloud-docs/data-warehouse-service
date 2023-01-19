:original_name: dws_04_0215.html

.. _dws_04_0215:

Importing Data
==============

Viewing Data in the MRS Data Source by Directly Querying the Foreign Table
--------------------------------------------------------------------------

If the data amount is small, you can directly run SELECT to query the foreign table and view the data in the MRS data source.

#. Run the following command to query data from the foreign table:

   ::

      SELECT * FROM foreign_product_info;

   If the query result is the same as the data in :ref:`Data File <en-us_topic_0000001146360931__en-us_topic_0000001082830951_en-us_topic_0109259515_en-us_topic_0101477888_section55166005141018>`, the import is successful. The following information is displayed at the end of the query result:

   .. code-block::

      (20 rows)

   After data is queried, you can insert the data to common tables in the database.

.. _en-us_topic_0000001146121075__en-us_topic_0000001083024575_en-us_topic_0109259518_en-us_topic_0101477887_section1375535445410:

Querying Data After Importing It
--------------------------------

You can query the MRS data after importing it to GaussDB(DWS).

#. Create a table in GaussDB(DWS) to store imported data.

   The target table structure must be the same as the structure of the foreign table created in :ref:`Creating a Foreign Table <dws_04_0214>`. That is, both tables must have the same number of columns and column types.

   For example, create a table named **product_info**. The table example is as follows:

   ::

      DROP TABLE IF EXISTS product_info;
      CREATE TABLE product_info
      (
          product_price                integer        ,
          product_id                   char(30)       ,
          product_time                 date           ,
          product_level                char(10)       ,
          product_name                 varchar(200)   ,
          product_type1                varchar(20)    ,
          product_type2                char(10)       ,
          product_monthly_sales_cnt    integer        ,
          product_comment_time         date           ,
          product_comment_num          integer        ,
          product_comment_content      varchar(200)
      )
      with (
      orientation = column,
      compression=middle
      )
      DISTRIBUTE BY HASH (product_id);

#. Run the **INSERT INTO .. SELECT ..** command to import data from the foreign table to the target table.

   Example:

   ::

      INSERT INTO product_info SELECT * FROM foreign_product_info;

   If information similar to the following is displayed, the data has been imported.

   .. code-block::

      INSERT 0 20

#. Run the following **SELECT** command to view data imported from MRS to GaussDB(DWS):

   ::

      SELECT * FROM product_info;

   If the query result is the same as the data in :ref:`Data File <en-us_topic_0000001146360931__en-us_topic_0000001082830951_en-us_topic_0109259515_en-us_topic_0101477888_section55166005141018>`, the import is successful. The following information is displayed at the end of the query result:

   .. code-block::

      (20 rows)
