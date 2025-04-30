:original_name: dws_04_0736.html

.. _dws_04_0736:

PG_LIFECYCLE_DATA_DISTRIBUTE
============================

**PG_LIFECYCLE_DATA_DISTRIBUTE** displays the distribution of cold and hot data in a multi-temperature table of OBS.

.. table:: **Table 1** PG_LIFECYCLE_DATA_DISTRIBUTE columns

   =================== ==== ===============================================
   Column              Type Description
   =================== ==== ===============================================
   schemaname          Name Schema name
   tablename           Name Current table name
   nodename            Name Node name
   hotpartition        Text Hot partition on the DN
   coldpartition       Text Cold partition on the DN
   switchablepartition Text Switchable partition on the DN
   hotdatasize         Text Data size of the hot partition on the DN
   colddatasize        Text Data size of the cold partition on the DN
   switchabledatasize  Text Data size of the switchable partition on the DN
   =================== ==== ===============================================
