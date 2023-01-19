:original_name: dws_04_0644.html

.. _dws_04_0644:

ALL_DEPENDENCIES
================

**ALL_DEPENDENCIES** displays dependencies between functions and advanced packages accessible to the current user.

.. important::

   Currently in GaussDB(DWS), this table is empty without any record due to information constraints.

.. table:: **Table 1** ALL_DEPENDENCIES columns

   +----------------------+------------------------+-------------------------------------------+
   | Name                 | Type                   | Description                               |
   +======================+========================+===========================================+
   | owner                | character varying(30)  | Owner of the object                       |
   +----------------------+------------------------+-------------------------------------------+
   | name                 | character varying(30)  | Object name                               |
   +----------------------+------------------------+-------------------------------------------+
   | type                 | character varying(17)  | Type of the object                        |
   +----------------------+------------------------+-------------------------------------------+
   | referenced_owner     | character varying(30)  | Owner of the referenced object            |
   +----------------------+------------------------+-------------------------------------------+
   | referenced_name      | character varying(64)  | Name of the referenced object             |
   +----------------------+------------------------+-------------------------------------------+
   | referenced_type      | character varying(17)  | Type of the referenced object             |
   +----------------------+------------------------+-------------------------------------------+
   | referenced_link_name | character varying(128) | Name of the link to the referenced object |
   +----------------------+------------------------+-------------------------------------------+
   | schemaid             | numeric                | ID of the current schema                  |
   +----------------------+------------------------+-------------------------------------------+
   | dependency_type      | character varying(4)   | Dependency type (REF or HARD)             |
   +----------------------+------------------------+-------------------------------------------+
