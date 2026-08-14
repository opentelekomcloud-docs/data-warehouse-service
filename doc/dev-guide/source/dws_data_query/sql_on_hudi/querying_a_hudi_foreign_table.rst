:original_name: dws_04_1075.html

.. _dws_04_1075:

Querying a Hudi Foreign Table
=============================

You can query data in a Hudi foreign table. By default, it gives you a real-time view. You can set parameters to query the incremental data.

.. note::

   In DWS, a Hudi foreign table that uses the schema evolution feature cannot be read. Schema evolution enables you to modify a Hudi table's schema throughout its lifecycle, ensuring the data remains adaptable to changing requirements. This includes adding, deleting, or modifying fields within the table.

Querying Incremental Data in a Hudi Foreign Table
-------------------------------------------------

You can set incremental query parameters to query incremental data in a Hudi foreign table.

::

   SET hoodie.SCHEMA.FOREIGN_TABLE.consume.mode=incremental;
   SET hoodie.SCHEMA.FOREIGN_TABLE.consume.start.timestamp=start_timestamp;
   SET hoodie.SCHEMA.FOREIGN_TABLE.consume.ending.timestamp=end_timestamp;
   SELECT * FROM SCHEMA.FOREIGN_TABLE;

Example:

Query the incremental data of the MOR hudi foreign table **public.rtd_mfdt_int_currency_ft** from **20221207164617** to **20221207170234**.

::

   SET hoodie.public.rtd_mfdt_int_currency_ft.consume.mode=incremental;
   SET hoodie.public.rtd_mfdt_int_currency_ft.consume.start.timestamp=20221207164617;
   SET hoodie.public.rtd_mfdt_int_currency_ft.consume.ending.timestamp=20221207170234;
   SELECT * FROM public.rtd_mfdt_int_currency_ft where _hoodie_commit_time>20221207164617 and _hoodie_commit_time<=20221207170234;

Querying the Configured Incremental Parameters in a Hudi Foreign Table
----------------------------------------------------------------------

You can use the **pg_show_custom_settings()** function to view the configured incremental parameters in a Hudi foreign table and check whether the parameters are correctly set.

::

   SELECT * FROM pg_show_custom_settings();

Querying the Properties of a Hudi Foreign Table (hoodie.properties)
-------------------------------------------------------------------

Query the hoodie.properties properties of a Hudi foreign table on OBS.

Syntax:

::

   SELECT * FROM hudi_get_options('SCHEMA.FOREIGN_TABLE');

Example: Query the hudi properties of the OBS foreign table **rtd_mfdt_int_currency_ft** in the current schema.

::

   SELECT * FROM hudi_get_options('rtd_mfdt_int_currency_ft');

Querying the Maximum Timeline of a Hudi Foreign Table
-----------------------------------------------------

Query the maximum timeline of Hudi data on OBS, that is, the latest commit record.

Syntax:

::

   SELECT * FROM hudi_get_max_commit('SCHEMA.FOREIGN_TABLE');

Example: Query the maximum timeline of the OBS foreign table **rtd_mfdt_int_currency_ft** in the current schema.

::

   SELECT * FROM hudi_get_max_commit('rtd_mfdt_int_currency_ft');
