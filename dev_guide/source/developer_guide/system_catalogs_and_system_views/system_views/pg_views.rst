:original_name: dws_04_0793.html

.. _dws_04_0793:

PG_VIEWS
========

**PG_VIEWS** displays basic information about each view in the database.

.. table:: **Table 1** PG_VIEWS columns

   +------------+------+-------------------------------------------+-------------------------------------------+
   | Name       | Type | Reference                                 | Description                               |
   +============+======+===========================================+===========================================+
   | schemaname | name | :ref:`PG_NAMESPACE <dws_04_0600>`.nspname | Name of the schema that contains the view |
   +------------+------+-------------------------------------------+-------------------------------------------+
   | viewname   | name | :ref:`PG_CLASS <dws_04_0578>`.relname     | View name                                 |
   +------------+------+-------------------------------------------+-------------------------------------------+
   | viewowner  | name | :ref:`PG_AUTHID <dws_04_0574>`.Erolname   | Owner of the view                         |
   +------------+------+-------------------------------------------+-------------------------------------------+
   | definition | text | ``-``                                     | Definition of the view                    |
   +------------+------+-------------------------------------------+-------------------------------------------+
