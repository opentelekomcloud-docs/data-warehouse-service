:original_name: dws_04_0716.html

.. _dws_04_0716:

GS_VIEW_DEPENDENCY
==================

**GS_VIEW_DEPENDENCY** allows you to query the direct dependencies of all views visible to the current user.

.. table:: **Table 1** GS_VIEW_DEPENDENCY columns

   +-----------------------+-----------------------+------------------------------------------------------+
   | Column                | Type                  | Description                                          |
   +=======================+=======================+======================================================+
   | objschema             | Name                  | View space name                                      |
   +-----------------------+-----------------------+------------------------------------------------------+
   | objname               | Name                  | View name                                            |
   +-----------------------+-----------------------+------------------------------------------------------+
   | refobjschema          | Name                  | Name of the space where the dependent object resides |
   +-----------------------+-----------------------+------------------------------------------------------+
   | refobjname            | Name                  | Name of a dependent object                           |
   +-----------------------+-----------------------+------------------------------------------------------+
   | relobjkind            | Char                  | Type of a dependent object                           |
   |                       |                       |                                                      |
   |                       |                       | -  **r**: table                                      |
   |                       |                       | -  **v**: view                                       |
   +-----------------------+-----------------------+------------------------------------------------------+
