:original_name: dws_04_0672.html

.. _dws_04_0672:

DBA_SOURCE
==========

**DBA_SOURCE** displays all stored procedures or functions in the database, and it provides the columns defined by the stored procedures or functions. It is accessible only to users with system administrator rights.

+--------+-----------------------+----------------------------------------------------+
| Column | Type                  | Description                                        |
+========+=======================+====================================================+
| owner  | character varying(64) | Owner of the stored procedure or the function      |
+--------+-----------------------+----------------------------------------------------+
| Name   | character varying(64) | Name of the stored procedure or the function       |
+--------+-----------------------+----------------------------------------------------+
| Text   | Text                  | Definition of the stored procedure or the function |
+--------+-----------------------+----------------------------------------------------+
