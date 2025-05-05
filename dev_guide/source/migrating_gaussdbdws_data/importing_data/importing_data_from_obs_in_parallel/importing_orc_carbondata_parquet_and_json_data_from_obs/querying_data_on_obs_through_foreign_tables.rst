:original_name: dws_04_0246.html

.. _dws_04_0246:

Querying Data on OBS Through Foreign Tables
===========================================

Viewing Data on OBS by Directly Querying the Foreign Table
----------------------------------------------------------

If the data amount is small, you can directly run **SELECT** to query the foreign table and view the data on OBS.

#. Run the following command to query data from the foreign table:

   ::

      SELECT * FROM product_info_ext_obs;

   If the query result is the same as the data in :ref:`Original Data <en-us_topic_0000001811609589__en-us_topic_0000001188482188_en-us_topic_0000001145410931_en-us_topic_0102810712_en-us_topic_0093837401_section1888720568453>`, the import is successful. The following information is displayed at the end of the query result:

   ::

      (10 rows)

   After data is queried, you can insert the data to common tables in the database.

.. _en-us_topic_0000001764491660__en-us_topic_0000001188642102_en-us_topic_0000001099130958_en-us_topic_0102810710_section152121815193012:

Querying Data After Importing It
--------------------------------

#. Create a table in GaussDB(DWS) to store imported data.

   The target table structure must be the same as the structure of the foreign table created in :ref:`Creating a Foreign Table <dws_04_0245>`. That is, both tables must have the same number of columns and column types.

   For example, create a table named *product_info*. The table example is as follows:

   ::

      DROP TABLE IF EXISTS product_info;

      CREATE TABLE product_info
      (
          product_price                integer        not null,
          product_id                   char(30)       not null,
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

#. Run the **INSERT INTO.. SELECT ..** command to import data from the foreign table to the target table.

   Example:

   ::

      INSERT INTO product_info SELECT * FROM product_info_ext_obs;

   If information similar to the following is displayed, the data has been imported.

   ::

      INSERT 0 10

#. Run the following **SELECT** command to view data imported from OBS to GaussDB(DWS):

   ::

      SELECT * FROM product_info;

   If the query result is the same as the data in :ref:`Original Data <en-us_topic_0000001811609589__en-us_topic_0000001188482188_en-us_topic_0000001145410931_en-us_topic_0102810712_en-us_topic_0093837401_section1888720568453>`, the import is successful. The following information is displayed at the end of the query result:

   ::

      (10 rows)
