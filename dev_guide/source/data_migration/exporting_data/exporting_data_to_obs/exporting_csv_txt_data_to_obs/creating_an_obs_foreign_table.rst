:original_name: dws_04_0253.html

.. _dws_04_0253:

Creating an OBS Foreign Table
=============================

Procedure
---------

#. Based on the path planned in :ref:`Planning Data Export <dws_04_0252>`, determine the value of the **location** parameter used for creating a foreign table.

#. Obtain the access keys (AK and SK) to access OBS.

   To obtain access keys, log in to the management console, click the username in the upper right corner, and select **My Credential** from the menu. Then choose **Access Keys** in the navigation tree on the left. On the **Access Keys** page, you can view the existing AKs or click **Add Access Key** to create and download access keys.

#. Examine the formats of data to be exported and determine the values of data format parameters used for creating a foreign table. For details, see data format parameters.

#. Create an OBS table based on the parameter settings in the preceding steps. For details about how to create a foreign table, see CREATE FOREIGN TABLE (for GDS Import and Export).

Example 1
---------

For example, in the GaussDB(DWS) database, create a write-only foreign table with the **format** parameter as **text** to export text files. Set parameters as follows:

-  **location**

   The OBS path of the source data file has been obtained in :ref:`step 2 <en-us_topic_0000001188482188__en-us_topic_0000001145410931_en-us_topic_0102810712_li123314509351>` in :ref:`Planning Data Export <dws_04_0252>`.

   For example, set **location** as follows:

   .. code-block::

      location 'obs://mybucket/output_data/',

-  **Access keys (AK and SK)**

   -  Set **access_key** to the AK you have obtained.
   -  Set **secret_access_key** to the SK you have obtained.

   .. note::

      **access_key** and **secret_access_key** have been obtained during user creation. Replace the italic part with the actual keys.

-  **Data format parameters**

   -  Set **format** to **TEXT**.
   -  Set **encoding** to **UTF-8**.
   -  Configure **encrypt**. Its default value is **off**.
   -  Set **delimiter** to **\|**.

Based on the preceding settings, the foreign table is created using the following statements:

.. important::

   // Hard-coded or plaintext AK and SK are risky. For security purposes, encrypt your AK and SK and store them in the configuration file or environment variables.

::

   DROP FOREIGN TABLE IF EXISTS product_info_output_ext1;
   CREATE FOREIGN TABLE product_info_output_ext1
   (
    c_bigint bigint,
    c_char char(30),
    c_varchar varchar(30),
    c_nvarchar2 nvarchar2(30) ,
    c_data date,
    c_time time ,
    c_test varchar(30))
    server gsmpp_server
    options (
    LOCATION 'obs://mybucket/output_data/',
    ACCESS_KEY 'access_key_value_to_be_replaced',
    SECRET_ACCESS_KEY 'secret_access_key_value_to_be_replaced'
    format 'text',
    delimiter '|',
    encoding 'utf-8',
    encrypt 'on'
     )
    WRITE ONLY;

If the following information is displayed, the foreign table has been created:

.. code-block::

   CREATE FOREIGN TABLE

Example 2:
----------

For example, in the GaussDB(DWS) database, create a write-only foreign table with the **format** parameter as **CSV** to export CSV files. Set parameters as follows:

-  **location**

   The OBS path of the source data file has been obtained in :ref:`step 2 <en-us_topic_0000001188482188__en-us_topic_0000001145410931_en-us_topic_0102810712_li123314509351>` in :ref:`Planning Data Export <dws_04_0252>`.

   For example, set **location** as follows:

   ::

      location 'obs://mybucket/output_data/',

-  **Access keys (AK and SK)**

   -  Set **access_key** to the AK you have obtained.
   -  Set **secret_access_key** to the SK you have obtained.

   .. note::

      **access_key** and **secret_access_key** have been obtained during user creation. Replace the italic part with the actual keys.

-  **Data format parameters**

   -  Set **format** to **CSV**.

   -  Set **encoding** to **UTF-8**.

   -  Configure **encrypt**. Its default value is **off**.

   -  Set **delimiter** to **,**.

   -  Set **header** (whether the exported data file contains the header row).

      Specifies whether a file contains a header with the names of each column in the file.

      When exporting data from OBS, this parameter cannot be set to **true**. Use the default value **false**, indicating that the first row of the exported data file is not the header.

Based on the preceding settings, the foreign table is created using the following statements:

.. important::

   // Hard-coded or plaintext AK and SK are risky. For security purposes, encrypt your AK and SK and store them in the configuration file or environment variables.

::

   DROP FOREIGN TABLE IF EXISTS product_info_output_ext2;
   CREATE FOREIGN TABLE product_info_output_ext2
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
   SERVER gsmpp_server
   OPTIONS(
   location 'obs://mybucket/output_data/',
   FORMAT 'CSV' ,
   DELIMITER ',',
   encoding 'utf8',
   header 'false',
   ACCESS_KEY 'access_key_value_to_be_replaced',
   SECRET_ACCESS_KEY 'secret_access_key_value_to_be_replaced'
   )
   WRITE ONLY ;

If the following information is displayed, the foreign table has been created:

::

   CREATE FOREIGN TABLE
