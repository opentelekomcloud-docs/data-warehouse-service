:original_name: dws_04_0670.html

.. _dws_04_0670:

DBA_PROCEDURES
==============

**DBA_PROCEDURES** displays information about all stored procedures and functions in the database. This view is accessible only to users with system administrator rights.

+-----------------+-----------------------+--------------------------------------------------------+
| Column          | Type                  | Description                                            |
+=================+=======================+========================================================+
| owner           | character varying(64) | Owner of the stored procedure or the function          |
+-----------------+-----------------------+--------------------------------------------------------+
| object_name     | character varying(64) | Name of the stored procedure or the function           |
+-----------------+-----------------------+--------------------------------------------------------+
| argument_number | Smallint              | Number of the input parameters in the stored procedure |
+-----------------+-----------------------+--------------------------------------------------------+
