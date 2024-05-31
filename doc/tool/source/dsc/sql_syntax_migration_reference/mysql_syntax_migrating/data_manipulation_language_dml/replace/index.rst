:original_name: dws_16_0198.html

.. _dws_16_0198:

.. _en-us_topic_0000001772696244:

REPLACE
=======

In MySQL, **REPLACE** allows the following keywords: **LOW_PRIORITY**, **PARTITION**, **DELAYED**, **VALUES**, and **SET**. The following examples are temporary migration solutions only.

.. note::

   **REPLACE** works exactly like **INSERT**, except that if an old row in the table has the same value as a new row for a primary key or unique index, the old row is deleted before the new row is inserted.

-  :ref:`LOW_PRIORITY <dws_16_0199>`
-  :ref:`PARTITION <dws_16_0200>`
-  :ref:`DELAYED <dws_16_0201>`
-  :ref:`VALUES <dws_16_0202>`
-  :ref:`SET <dws_16_0203>`

.. toctree::
   :maxdepth: 1
   :hidden: 

   low_priority
   partition
   delayed
   values
   set
