:original_name: dws_04_0590.html

.. _dws_04_0590:

PG_EXTENSION_DATA_SOURCE
========================

**PG_EXTENSION_DATA_SOURCE** records information about external data source. An external data source contains information about an external database, such as its password encoding. It is mainy used with Extension Connector.

.. table:: **Table 1** PG_EXTENSION_DATA_SOURCE columns

   +------------+-----------+---------------+---------------------------------------------------------------------+
   | Name       | Type      | Reference     | Description                                                         |
   +============+===========+===============+=====================================================================+
   | oid        | oid       | ``-``         | Row identifier (hidden attribute; must be explicitly selected)      |
   +------------+-----------+---------------+---------------------------------------------------------------------+
   | srcname    | name      | ``-``         | Name of an external data source                                     |
   +------------+-----------+---------------+---------------------------------------------------------------------+
   | srcowner   | oid       | PG_AUTHID.oid | Owner of an external data source                                    |
   +------------+-----------+---------------+---------------------------------------------------------------------+
   | srctype    | text      | ``-``         | Type of an external data source. It is NULL by default.             |
   +------------+-----------+---------------+---------------------------------------------------------------------+
   | srcversion | text      | ``-``         | Type of an external data source. It is NULL by default.             |
   +------------+-----------+---------------+---------------------------------------------------------------------+
   | srcacl     | aclitem[] | ``-``         | Access permissions                                                  |
   +------------+-----------+---------------+---------------------------------------------------------------------+
   | srcoptions | text[]    | ``-``         | Option used for foreign data sources. It is a keyword=value string. |
   +------------+-----------+---------------+---------------------------------------------------------------------+
