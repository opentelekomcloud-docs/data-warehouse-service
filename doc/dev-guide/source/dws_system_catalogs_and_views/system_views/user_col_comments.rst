:original_name: dws_04_0859.html

.. _dws_04_0859:

USER_COL_COMMENTS
=================

**USER_COL_COMMENTS** stores the column comments of the tables and views that the current user can access.

=========== ===================== ==========================
Column      Type                  Description
=========== ===================== ==========================
column_name character varying(64) Column name
table_name  character varying(64) Table or view name
owner       character varying(64) Owner of the table or view
comments    Text                  Comments
=========== ===================== ==========================
