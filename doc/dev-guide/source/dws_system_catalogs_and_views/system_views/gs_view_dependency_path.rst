:original_name: dws_04_0948.html

.. _dws_04_0948:

GS_VIEW_DEPENDENCY_PATH
=======================

**GS_VIEW_DEPENDENCY_PATH** allows you to query the direct dependencies of all views visible to the current user. If the base table on which the view depends exists and the dependency between views at different levels is normal, you can use this view to query the dependency between views at different levels starting from the base table.

.. table:: **Table 1** GS_VIEW_DEPENDENCY_PATH columns

   ============ ==== ====================================================
   Column       Type Description
   ============ ==== ====================================================
   objschema    Name View space name
   objname      Name View name
   refobjschema Name Name of the space where the dependent object resides
   refobjname   Name Name of a dependent object
   path         Text Dependency path
   ============ ==== ====================================================
