:original_name: dws_04_0160.html

.. _dws_04_0160:

Overview
========

GaussDB(DWS) allows you to export ORC and Parquet data to MRS using an HDFS foreign table. You can specify the export mode and export data format in the foreign table. Data is exported from GaussDB(DWS) in parallel using multiple DNs and stored in HDFS. In this way, the overall export performance is improved.

-  The CN only plans data export tasks and delivers the tasks to DNs for execution. In this case, the CN is released to process external requests.
-  Every DN is involved in data export, and the computing capabilities and bandwidths of all the DNs are fully leveraged to export data.

-  Multiple HDFS servers can export data concurrently. The export path can be empty. The naming rule of the path must be the same as that of the exported file.
-  MRS connects to GaussDB(DWS) cluster nodes. The export rate is affected by the network bandwidth.
-  Data files in the ORC or Parquet format are supported.

.. note::

   This section uses the ORC format as an example to describe how to export data. The method for exporting Parquet data is similar. Parquet data can be exported from clusters of version 9.1.0 or later.

Naming Rules of Exported Files
------------------------------

The rules for naming ORC and Parquet data files exported from GaussDB(DWS) are as follows:

#. Data exported to MRS (HDFS): When data is exported from a DN, the data is stored in HDFS in the segment format. The file is named in the format of **mpp\_**\ *Database name*\ **\_**\ *Schema name*\ **\_**\ *Table name*\ **\_**\ *Node name*\ **\_**\ *n*\ **\_**\ *UUID*\ **.**\ *Data format*. *n* is a natural number starting from 0 in ascending order, for example, 0, 1, 2, 3. The UUID should be a standard one, comprising of 32 hexadecimal characters and divided into five segments by hyphens (-). The format should be 8-4-4-4-12.
#. You are advised to export data from different clusters or databases to different paths. The maximum size of a single file in ORC or Parquet format is about 256 MB. (This is a soft constraint and may exceed a little in actual services.)
#. After the export is complete, the **\_SUCCESS** file is generated.
