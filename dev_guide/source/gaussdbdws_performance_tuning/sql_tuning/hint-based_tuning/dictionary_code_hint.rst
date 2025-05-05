:original_name: dws_04_1208.html

.. _dws_04_1208:

Dictionary Code Hint
====================

Function
--------

Specifies a column to construct a dictionary code. The comparison of character strings in the dictionary code is converted into the comparison of numbers, which accelerates the query speed such as **group by** and **filter**. This hint is supported only by clusters of version 8.3.0 or later.

Precautions
-----------

-  Currently, only the new version hstore tables are supported (the table-level parameter **enable_hstore_opt** is set to **on**).

Syntax
------

.. code-block::

    /* + ( no ) dict(table (column)) */

Parameter description
---------------------

-  dict(table (column))

   Column with the dictionary encoding table enabled.

-  no dict(table (column))

   Column with the dictionary encoding table disabled.

Example
-------

::

   SELECT /*+ dict (bitmaptbl_high (server_ip)) */ distinct(server_ip) FROM bitmaptbl_high WHERE scope_name='saetataetaeta' ORDER BY server_ip;

The generated plan is as follows. server_ip uses the dictionary encoding:

|image1|

You can use no **dict to** disable **server_ip** from using dictionary encoding.

::

   SELECT /*+ no dict (bitmaptbl_high (server_ip)) */ distinct(server_ip) FROM bitmaptbl_high WHERE scope_name='saetataetaeta' ORDER BY server_ip;

.. |image1| image:: /_static/images/en-us_image_0000001764651288.png
