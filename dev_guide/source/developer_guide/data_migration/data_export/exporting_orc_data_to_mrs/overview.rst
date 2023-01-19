:original_name: dws_04_0160.html

.. _dws_04_0160:

Overview
========

GaussDB(DWS) allows you to export ORC data to MRS using an HDFS foreign table. You can specify the export mode and export data format in the foreign table. Data is exported from GaussDB(DWS) in parallel using multiple DNs and stored in HDFS. In this way, the overall export performance is improved.

-  The CN only plans data export tasks and delivers the tasks to DNs. In this case, the CN is released to process external requests.
-  The computing capability and bandwidth of all the DNs are fully leveraged to export data.
-  Multiple HDFS servers can export data concurrently. The export path can be empty. The naming rule of the path must be the same as that of the exported file.
-  MRS connects to GaussDB(DWS) cluster nodes. The export rate is affected by the network bandwidth.
-  Data files in the ORC format are supported.

Naming Rules of Exported Files
------------------------------

The rules for naming ORC data files exported from GaussDB(DWS) are as follows:

#. Data exported to MRS (HDFS): When data is exported from a DN, the data is stored in HDFS in the segment format. The file is named in the format of **mpp\_**\ *Database name*\ **\_**\ *Schema name*\ **\_**\ *Table name*\ **\_**\ *Node name*\ **\_**\ *n*\ **.orc**. *n* is a natural number starting from 0 in ascending order, for example, 0, 1, 2, 3.
#. You are advised to export data from different clusters or databases to different paths. The maximum size of an ORC file is 128 MB, and that of a stripe file is 64 MB.
#. After the export is complete, the **\_SUCCESS** file is generated.
