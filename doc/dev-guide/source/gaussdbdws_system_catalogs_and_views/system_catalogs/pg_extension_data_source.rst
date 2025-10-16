:original_name: dws_04_0590.html

.. _dws_04_0590:

PG_EXTENSION_DATA_SOURCE
========================

**PG_EXTENSION_DATA_SOURCE** records information about external data source. An external data source contains information about an external database, such as its password encoding. It is mainly used with Extension Connector.

.. table:: **Table 1** PG_EXTENSION_DATA_SOURCE columns

   +------------+-----------+---------------+---------------------------------------------------------------------+
   | Name       | Type      | Reference     | Description                                                         |
   +============+===========+===============+=====================================================================+
   | OID        | OID       | ``-``         | Row identifier (hidden attribute; must be explicitly selected)      |
   +------------+-----------+---------------+---------------------------------------------------------------------+
   | srcname    | Name      | ``-``         | Name of an external data source                                     |
   +------------+-----------+---------------+---------------------------------------------------------------------+
   | srcowner   | OID       | PG_AUTHID.oid | Owner of an external data source                                    |
   +------------+-----------+---------------+---------------------------------------------------------------------+
   | srctype    | Text      | ``-``         | Type of an external data source. It is NULL by default.             |
   +------------+-----------+---------------+---------------------------------------------------------------------+
   | srcversion | Text      | ``-``         | Type of an external data source. It is NULL by default.             |
   +------------+-----------+---------------+---------------------------------------------------------------------+
   | srcacl     | aclitem[] | ``-``         | Access permissions                                                  |
   +------------+-----------+---------------+---------------------------------------------------------------------+
   | srcoptions | Text[]    | ``-``         | Option used for foreign data sources. It is a keyword=value string. |
   +------------+-----------+---------------+---------------------------------------------------------------------+
