:original_name: dws_04_0186.html

.. _dws_04_0186:

Importing Data
==============

Context
-------

Before importing data, you are advised to optimize your design and deployment based on the following excellent practices, helping maximize system resource utilization and improving data import performance.

-  In most cases, OBS data import performance is limited by concurrent network access rate. Therefore, you are advised to deploy multiple buckets on the OBS server to import data in parallel from buckets, better utilizing DN data transfer.

-  Similar to the single table import, ensure that the I/O performance is greater than the maximum network throughput in the concurrent import.

-  Set GUC parameters :ref:`raise_errors_if_no_files <en-us_topic_0000001099134526__sd65ae1e8226f4611819e91ce8b6a35fb>`, :ref:`partition_mem_batch <en-us_topic_0000001099134526__s9d2ce4e6e9ea4f6a8a8df3e3a7ddadd8>`, and :ref:`partition_max_cache_size <en-us_topic_0000001099134526__s004b2931955e4e549caeb98b2f2723af>`. When importing data, specify whether to distinguish between the following two cases: no records exist in the data file or the data file does not exist. You also need to specify the number of caches and the size of data buffers.

-  If a table has an index, the index information is incrementally updated during the import, affecting data import performance. You are advised to delete the index from the target table before the import. You can create index again after the import is complete.

Procedure
---------

#. Create a table in the GaussDB(DWS) database to store the data imported from the OBS. For details about the syntax, see CREATE TABLE.

   The structure of the table must be consistent with that of the fields in the source data file. That is, the number of fields and field types must be the same. In addition, the structure of the target table must be the same as that of the foreign table. The field names can be different.

#. (Optional) If the target table has an index, the index information is incrementally updated during the import, affecting data import performance. You are advised to delete the index from the target table before the import. You can create index again after the import is complete.

#. Import data.

   ::

      INSERT INTO [Target table name] SELECT * FROM [Foreign table name]

   -  If information similar to the following is displayed, the data has been imported. Query the error information table to check whether any data format errors occurred. For details, see :ref:`Handling Import Errors <dws_04_0187>`.

      ::

         INSERT 0 20

   -  If data fails to be loaded, rectify the problem by following the instructions provided in :ref:`Handling Import Errors <dws_04_0187>` and try again.

Example
-------

For example, create a table named *product_info*.

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

Run the following statement to import data from the *product_info*\ **\_ext** foreign table to the *product_info* table:

::

   INSERT INTO product_info SELECT * FROM product_info_ext;
