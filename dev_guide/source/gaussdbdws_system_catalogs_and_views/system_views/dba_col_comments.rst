:original_name: dws_04_0660.html

.. _dws_04_0660:

DBA_COL_COMMENTS
================

**DBA_COL_COMMENTS** displays column comments in the tables and views of a database. Only users with system administrator permissions can access this view.

=========== ===================== ==========================
Column      Type                  Description
=========== ===================== ==========================
column_name character varying(64) Column name
table_name  character varying(64) Table or view name
owner       character varying(64) Owner of the table or view
comments    Text                  Comments
=========== ===================== ==========================
