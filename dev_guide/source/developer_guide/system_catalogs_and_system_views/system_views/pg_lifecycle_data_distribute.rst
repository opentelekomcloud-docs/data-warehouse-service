:original_name: dws_04_0736.html

.. _dws_04_0736:

PG_LIFECYCLE_DATA_DISTRIBUTE
============================

**PG_LIFECYCLE_DATA_DISTRIBUTE** displays the distribution of cold and hot data in a multi-temperature table of OBS.

.. table:: **Table 1** PG_LIFECYCLE_DATA_DISTRIBUTE columns

   =================== ==== ===============================================
   Name                Type Description
   =================== ==== ===============================================
   schemaname          name Schema name
   tablename           name Current table name
   nodename            name Node name
   hotpartition        text Hot partition on the DN
   coldpartition       text Cold partition on the DN
   switchablepartition text Switchable partition on the DN
   hotdatasize         text Data size of the hot partition on the DN
   colddatasize        text Data size of the cold partition on the DN
   switchabledatasize  text Data size of the switchable partition on the DN
   =================== ==== ===============================================
