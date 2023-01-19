:original_name: dws_06_0006.html

.. _dws_06_0006:

PostgreSQL Features Unsupported by GaussDB(DWS)
===============================================

-  Table inheritance
-  Table creation features:

   -  Use **REFERENCES reftable [ (refcolumn) ] [ MATCH FULL \| MATCH PARTIAL \| MATCH SIMPLE ] [ ON DELETE action ] [ ON UPDATE action ]** to create a foreign key constraint for a table.
   -  Use **EXCLUDE [ USING index_method ] ( exclude_element WITH operator [, ... ] )** to create exclusion constraints for a table.

-  Define or change the security tag of an object.
-  User-defined C functions
-  Create, modify, and delete operators.
-  Create, modify, and delete operator classes.
-  Create, modify, and delete operator families.
-  Create, modify, and delete text search parsers.
-  Create, modify, and delete text search templates.
-  Create, modify, and delete collations.
-  Create and delete rules.
-  Register, modify, and delete languages.
-  Create, modify, and delete domains.
-  Define, modify, and delete the conversion of character set encoding.
-  Define and delete casts.
-  Define, modify, and delete user mapping.
-  Generate a notification.
-  Listen to a notification.
-  Stop listening to a notification.
-  Load or reload a shared library file.
-  Release the session resources of a database.
-  Move a cursor backward.

The following features are disabled in GaussDB(DWS) for separation of rights:

-  **TO PUBLIC** of **GRANT**
-  **COPY FROM FILE** and **COPY TO FILE** of **COPY**
