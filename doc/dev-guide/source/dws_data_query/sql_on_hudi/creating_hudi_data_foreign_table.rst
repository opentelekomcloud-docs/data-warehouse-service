:original_name: dws_04_1073.html

.. _dws_04_1073:

Creating Hudi Data (Foreign Table)
==================================

DWS accesses Hudi data on OBS through foreign tables. Compared with common foreign tables, Hudi foreign tables do not require additional parameter settings. You only need to set **format** to **hudi** to specify the format of the data source file. For details, see "CREATE FOREIGN TABLE (SQL on OBS or Hadoop)" in *Data Warehouse Service (DWS) SQL Syntax Reference*.

Obtaining the Definitions of Tables on MRS.
-------------------------------------------

DWS Hudi foreign tables are read-only. You need to specify the columns and column types of the target data before creating a foreign table.

For example, for a Hudi table on MRS, you can use spark-sql to query the original table definitions:

::

   SHOW create table rtd_mfdt_int_currency_t;

Compiling DWS Table Definitions
-------------------------------

Copy the definitions of all columns in the MRS table, create a Hudi foreign table, and perform appropriate type conversion to be compatible with DWS syntax.

::

   CREATE FOREIGN TABLE rtd_mfdt_int_currency_ft(
   _hoodie_commit_time text,
   _hoodie_commit_seqno text,
   _hoodie_record_key text,
   _hoodie_partition_path text,
   _hoodie_file_name text,
   ...
   )SERVER hudi_server OPTIONS (
   foldername '/erpgc-test-01/s000/u1/rtd_mfdt_int_currency_t/',
   format 'hudi',
   encoding 'utf-8'
   )distribute by roundrobin;

**foldername** indicates the storage path of the Hudi data on OBS, which corresponds to **LOCATION** in the Spark-sql table definitions of MRS. The path must end with a slash (/).
