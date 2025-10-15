:original_name: dws_04_1073.html

.. _dws_04_1073:

Creating a Hudi Data Description (Foreign Table)
================================================

A foreign table maps data on OBS. GaussDB(DWS) accesses Hudi data on OBS through foreign tables. For details, see section "CREATE FOREIGN TABLE (SQL on OBS or Hadoop)" in the *Data Warehouse Service SQL Syntax Reference*.

Compared with OBS foreign tables, you only need to set **format** to **hudi** for Hudi foreign tables. For Hudi bucket tables, you need to set **distribute by** to **hash(bk_col1,bk_col2...)**. Only 9.1.0.100 and later versions support Hudi bucket tables.

Obtaining the Definitions of Tables on MRS.
-------------------------------------------

Hudi foreign tables on GaussDB(DWS) are read-only. Before creating a foreign table, you need to specify the number of fields defined in the target data and the type of each field. A Hudi foreign table supports a maximum of 5000 columns.

For example, for a Hudi table on MRS, you can use spark-sql to query the original table definitions:

::

   SHOW create table rtd_mfdt_int_currency_t;

Compiling GaussDB(DWS) Table Definitions
----------------------------------------

-  Non-bucket table

   Copy the definitions of all columns in the MRS table, perform proper type conversion to adapt to the GaussDB(DWS) syntax, and create an OBS foreign table.

   ::

      CREATE FOREIGN TABLE rtd_mfdt_int_currency_ft(
      _hoodie_commit_time text,
      _hoodie_commit_seqno text,
      _hoodie_record_key text,
      _hoodie_partition_path text,
      _hoodie_file_name text,
      ...
      )SERVER obs_server OPTIONS (
      foldername '/erpgc-obs-test-01/s000/sbi_fnd/rtd_mfdt_int_currency_t/',
      format 'hudi',
      encoding 'utf-8'
      )distribute by roundrobin;

   **foldername** indicates the storage path of the Hudi data on OBS, which corresponds to **LOCATION** in the Spark-sql table definitions of MRS. The path must end with a slash (/).

-  Bucket table

   Copy the definitions of all columns in the MRS table, perform proper type conversion to adapt to the GaussDB(DWS) syntax, create an OBS foreign table, and specify the hash distribution mode.

   ::

      CREATE FOREIGN TABLE rtd_mfdt_int_currency_ft(
      _hoodie_commit_time text,
      _hoodie_commit_seqno text,
      _hoodie_record_key text,
      _hoodie_partition_path text,
      _hoodie_file_name text,
      ...
      )SERVER obs_server OPTIONS (
      foldername '/erpgc-obs-test-01/s000/sbi_fnd/rtd_mfdt_int_currency_t/',
      format 'hudi',
      encoding 'utf-8'
      )distribute by hash(bk_col1,bk_col2...);

   **foldername** indicates the storage path of the Hudi data on OBS, which corresponds to **LOCATION** in the Spark-sql table definitions of MRS. The path must end with a slash (/).

   **distribute by** indicates the distribution column of the bucket table. The value must be the same as that of **hoodie.bucket.index.hash.field** in the **foldername/.hoodie/hoodie.index.properties** file.
