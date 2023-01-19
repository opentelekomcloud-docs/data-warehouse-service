:original_name: dws_04_0679.html

.. _dws_04_0679:

DBA_TRIGGERS
============

**DBA_TRIGGERS** displays information about triggers in the database. It is accessible only to users with system administrator rights.

.. table:: **Table 1** DBA_TRIGGERS columns

   +--------------+-----------------------+---------------------------------------------+
   | Name         | Type                  | Description                                 |
   +==============+=======================+=============================================+
   | trigger_name | character varying(64) | Trigger name                                |
   +--------------+-----------------------+---------------------------------------------+
   | table_name   | character varying(64) | Name of the table that defines the trigger  |
   +--------------+-----------------------+---------------------------------------------+
   | table_owner  | character varying(64) | Owner of the table that defines the trigger |
   +--------------+-----------------------+---------------------------------------------+
