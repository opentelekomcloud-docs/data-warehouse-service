:original_name: dws_07_0208.html

.. _dws_07_0208:

Configuration of check_input.xlsx
=================================

The **check_input.xlsx** file includes user input details such as schema, original table name, destination table name, specified column name (all columns are checked by default), check scope, check level (**low** by default), and excluded columns. You can customize this information as needed.

Perform the following steps to configure the parameters:

#. Open the **check_input.xlsx** file in the folder.

#. Change the values of parameters in the **check_input.xlsx** file based on the site requirements.

   For the description of the parameters in the **check_input.xlsx** file, see :ref:`Table 1 <en-us_topic_0000001860318817__en-us_topic_0000001382367698_table60771938143352>`.

   .. note::

      -  Parameter values are case-insensitive.
      -  Do not modify any other parameter except the listed ones.

#. Save the configuration and exit.

.. _en-us_topic_0000001860318817__en-us_topic_0000001382367698_table60771938143352:

.. table:: **Table 1** Parameters in the check_input.xlsx file

   +----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+---------------+-----------------------------------------------------------+
   | Parameter            | Description                                                                                                                                                                                | Value Range | Default Value | Example                                                   |
   +======================+============================================================================================================================================================================================+=============+===============+===========================================================+
   | Source Database Name | Name of the database to which the source table to be verified belongs. This parameter is optional. If this parameter is not specified, **src.dbname** in **dbinfo.properties** is used.    | NA          | NA            | mydb                                                      |
   +----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+---------------+-----------------------------------------------------------+
   | Source Schema Name   | Name of the schema to which the source table to be verified belongs. If this parameter is not involved, leave it blank.                                                                    | N/A         | N/A           | myschema                                                  |
   +----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+---------------+-----------------------------------------------------------+
   | Source Table Name    | Name of the table to be verified in the source database. This parameter is mandatory.                                                                                                      | N/A         | N/A           | order_info                                                |
   +----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+---------------+-----------------------------------------------------------+
   | Target Database Name | Name of the database to which the destination GaussDB(DWS) table belongs. This parameter is optional. If this parameter is not specified, **dws.dbname** in **dbinfo.properties** is used. | NA          | NA            | dws_db                                                    |
   +----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+---------------+-----------------------------------------------------------+
   | Target Schema Name   | Name of the schema to which the destination GaussDB(DWS) table belongs. This parameter is mandatory.                                                                                       | NA          | NA            | dws_sch                                                   |
   +----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+---------------+-----------------------------------------------------------+
   | Target Table Name    | Name of the table to be verified in the destination GaussDB(DWS) database. This parameter is mandatory.                                                                                    | N/A         | N/A           | dws_info                                                  |
   +----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+---------------+-----------------------------------------------------------+
   | Row Range(Where sql) | Check scope of table records. By default, all records are checked.                                                                                                                         | N/A         | ALL           | where begin_time > '2020-1-1' and begin_time < '2021-1-1' |
   +----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+---------------+-----------------------------------------------------------+
   | Column Range         | Column check. By default, all supported fields are checked, including numeric, time, and character types. This parameter is optional.                                                      | N/A         | N/A           | col1,col10,col19                                          |
   +----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+---------------+-----------------------------------------------------------+
   | Check Strategy       | Data check level                                                                                                                                                                           | -  low      | low           | low                                                       |
   |                      |                                                                                                                                                                                            | -  middle   |               |                                                           |
   |                      |                                                                                                                                                                                            | -  high     |               |                                                           |
   +----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+---------------+-----------------------------------------------------------+
   | Column Exclude       | Function that allows for the exclusion of specified columns from the sum check.                                                                                                            | N/A         | N/A           | col1,col10,col19                                          |
   +----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+---------------+-----------------------------------------------------------+
