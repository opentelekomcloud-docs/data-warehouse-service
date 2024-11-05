:original_name: dws_04_0787.html

.. _dws_04_0787:

PG_TIMEZONE_NAMES
=================

**PG_TIMEZONE_NAMES** displays all time zone names that can be recognized by **SET TIMEZONE**, along with their associated abbreviations, UTC offsets, and daylight saving time statuses.

.. table:: **Table 1** PG_TIMEZONE_NAMES columns

   +------------+----------+------------------------------------------------------------------------------------------+
   | Name       | Type     | Description                                                                              |
   +============+==========+==========================================================================================+
   | name       | text     | Name of the time zone                                                                    |
   +------------+----------+------------------------------------------------------------------------------------------+
   | abbrev     | text     | Time zone name abbreviation                                                              |
   +------------+----------+------------------------------------------------------------------------------------------+
   | utc_offset | interval | Offset from UTC                                                                          |
   +------------+----------+------------------------------------------------------------------------------------------+
   | is_dst     | boolean  | Whether DST is used. If it is, its value is **true**. Otherwise, its value is **false**. |
   +------------+----------+------------------------------------------------------------------------------------------+
