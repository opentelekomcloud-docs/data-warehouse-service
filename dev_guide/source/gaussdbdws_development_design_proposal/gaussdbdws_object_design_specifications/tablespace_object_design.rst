:original_name: dws_04_0111.html

.. _dws_04_0111:

TABLESPACE Object Design
========================

.. _en-us_topic_0000002136185105__en-us_topic_0000002100047734_section17496104317416:

Rule 2.8 Avoiding Tablespace Customization
------------------------------------------

.. note::

   **Impact of rule violation:**

   -  In a distributed scenario, using a custom tablespace to create a table can result in the table data not being stored in a distributed manner by DN, leading to storage skew.

   **Solution:**

   -  Use the built-in default tablespace when creating a table object.
