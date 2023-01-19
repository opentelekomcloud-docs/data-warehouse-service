:original_name: dws_04_0872.html

.. _dws_04_0872:

USER_SOURCE
===========

**USER_SOURCE** displays information about stored procedures or functions in this mode, and provides the columns defined by the stored procedures or the functions.

.. table:: **Table 1** USER_SOURCE columns

   +-------+-----------------------+----------------------------------------------------+
   | Name  | Type                  | Description                                        |
   +=======+=======================+====================================================+
   | owner | character varying(64) | Owner of the stored procedure or the function      |
   +-------+-----------------------+----------------------------------------------------+
   | name  | character varying(64) | Name of the stored procedure or the function       |
   +-------+-----------------------+----------------------------------------------------+
   | text  | text                  | Definition of the stored procedure or the function |
   +-------+-----------------------+----------------------------------------------------+
