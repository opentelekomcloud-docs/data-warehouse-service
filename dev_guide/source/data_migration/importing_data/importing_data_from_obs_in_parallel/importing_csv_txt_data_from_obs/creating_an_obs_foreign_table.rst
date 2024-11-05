:original_name: dws_04_0185.html

.. _dws_04_0185:

.. _en-us_topic_0000001717256720:

Creating an OBS Foreign Table
=============================

Procedure
---------

#. Set **location** of the foreign table based on the path planned in :ref:`Uploading Data to OBS <en-us_topic_0000001764896577>`.
#. Obtain the access keys (AK and SK) to access OBS. To obtain access keys, log in to the management console, move the cursor over the username in the upper right corner, and click **My Credentials**. Then choose **Access Keys** in the navigation tree on the left. On the **Access Keys** page, you can view the existing AKs or click **Add Access Key** to create an AK/SK pair.
#. Set data format parameters for the foreign table based on the formats of data to be imported. You need to collect the following source data information:

   -  **format**: format of the source data file in the foreign table. OBS foreign tables support CSV and TEXT formats. The default value is **TEXT**.
   -  **header**: Whether the data file contains a table header. Only CSV files can have headers.
   -  **delimiter**: Delimiter specified to separate data fields in a file. If no delimiter is specified, the default one will be used.
   -  For more parameters used for foreign tables, see data format parameters.

#. Plan the error tolerance of parallel import to specify how errors are handled during the import.

   -  **fill_missing_fields**: When the last column in a row of the source data file is empty, this parameter specifies whether to report an error or set this field in the row to **NULL**.

      .. note::

         Valid value: **true**, **on**, **false**, and **off**.

         -  If this parameter is set to **true** or **on** and the last column of a data row in a source data file is lost, the column will be replaced with **null** and no error message will be generated.

         -  If this parameter is set to **false** or **off** and the last column of a data row in a source data file is lost, the following error information will be displayed:

            .. code-block::

               missing data for column "tt"

         Default value: **false** or **off**

   -  **ignore_extra_data**: When the number of columns in the source data file is greater than that specified in the foreign table, this parameter specifies whether to report an error or ignore the extra columns.

      .. note::

         Value range: true/on, false/off.

         -  When this parameter is **true** or **on** and the number of data source files exceeds the number of foreign table columns, excessive columns will be ignored.

         -  If the parameter is set to **false** or **off**, and the number of data source files exceeds the number of foreign table columns, the following error information will be displayed:

            ::

               extra data after last expected column

         Default value: **false** or **off**

   -  **per node reject limit**: This parameter specifies the number of data format errors allowed on each DN. If the number of errors recorded in the error table on a DN exceeds the specified value, the import fails and an error message will be reported. This parameter is optional.

   -  **compatible_illegal_chars**: When an illegal character is encountered, this parameter specifies whether to import an error, or convert it and proceed with the import.

      .. note::

         Valid value: **true**, **on**, **false**, and **off**.

         -  When the parameter is **true** or **on**, invalid characters are tolerated and imported to the database after conversion.
         -  If the parameter is **false** or **off**, and an error occurs when there are invalid characters, the import will be interrupted.

         Default value: **false** or **off**

      The following describes the rules for converting an invalid character:

      -  **\\0** is converted to a space.
      -  Other invalid characters are converted to question marks (?).
      -  If **NULL**, **DELIMITER**, **QUOTE**, or **ESCAPE** is also set to a space or question mark, an error message such as "illegal chars conversion may confuse COPY escape 0x20" is displayed, prompting you to adjust parameter settings to avoid import errors.

   -  **error_table_name**: This parameter specifies the name of the table that records data format errors. After the parallel import, you can query this table for error details.

   -  For details about the parameters, see error tolerance parameters.

#. Create an OBS table based on the parameter settings in the preceding steps. For details about how to create a foreign table, see CREATE FOREIGN TABLE (for GDS Import and Export).

Example
-------

Create a foreign table in the GaussDB(DWS) database. Parameters are described as follows:

-  **Data format parameter access keys (AK and SK)**

   -  Set **access_key** to the AK you have obtained.
   -  Set **secret_access_key** to the SK you have obtained.

   .. note::

      The values of **access_key** and **secret_access_key** are examples only.

-  **Set data format parameters.**

   -  Set **format** to **CSV**.
   -  Set **encoding** to **UTF-8**.
   -  Configure **encrypt**. Its default value is **off**.
   -  Set **delimiter** to **,**.
   -  Retain the default value (double quotation marks) of **quote**.
   -  Set **null** (null value in a source data file) to a null string without quotation marks.
   -  Set **header** (whether the exported data file contains the header row) to the default value **false**. If the first row of the data file is not a header, retain the default value.

      .. note::

         When exporting data from OBS, this parameter cannot be set to **true**. Use the default value **false**.

-  **Set fault-tolerant parameters for data import.**

   -  To allow all data format errors detected during data import, set the **PER NODE REJECT LIMIT** to **'unlimited'**.
   -  To record data format errors detected during data import, set **LOG INTO** to **product_info_err**, which will store the errors in the **product_info_err** table.
   -  When **fill_missing_fields** is set to **true** and the last column of a data row in a source data file is missing, it will be replaced with **NULL** and no error message will be displayed.
   -  When **ignore_extra_data** is set to **true** and the number of columns in the source data file exceeds the number defined for the foreign table, any extra columns at the end of the row will be ignored and no error message will be displayed.

Based on the preceding settings, the foreign table is created using the following statements:

.. important::

   // Hard-coded or plaintext AK and SK are risky. For security purposes, encrypt your AK and SK and store them in the configuration file or environment variables.

::

   DROP FOREIGN TABLE product_info_ext;

   CREATE FOREIGN TABLE product_info_ext
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

   LOCATION 'obs://mybucket/input_data/product_info | obs://mybucket02/input_data/product_info',
   FORMAT 'CSV' ,
   DELIMITER ',',
   encoding 'utf8',
   header 'false',
   ACCESS_KEY 'access_key_value_to_be_replaced',
   SECRET_ACCESS_KEY 'secret_access_key_value_to_be_replaced',
   fill_missing_fields 'true',
   ignore_extra_data 'true'
   )
   READ ONLY
   LOG INTO product_info_err
   PER NODE REJECT LIMIT 'unlimited';

If the following information is displayed, the foreign table has been created:

::

   CREATE FOREIGN TABLE
