:original_name: dws_04_0076.html

.. _dws_04_0076:

Database Object Naming Conventions
==================================

The name of a database object must contain 1 to 63 characters, start with a letter or underscore (_), and can contain letters, digits, underscores (_), dollar signs ($), and number signs (#).

-  [Proposal] Do not use reserved or non-reserved keywords to name database objects.

   .. note::

      To query the keywords of GaussDB(DWS), run **select \* from pg_get_keywords()** or refer to section "Keyword."

-  [Proposal] Do not use a string enclosed in double quotation marks ("") to define the database object name, unless you need to specify its capitalization. Case sensitivity of database object names makes problem location difficult.
-  [Proposal] Use the same naming format for database objects.

   -  In a system undergoing incremental development or service migration, you are advised to comply with its historical naming conventions.
   -  A database object name consists of letters, digits, and underscores (_); and cannot start with a digit. You are advised to use multiple words separated with hyphens (-).
   -  You are advised to use intelligible names and common acronyms or abbreviations for database objects. Acronyms or abbreviations that are generally understood are recommended. For example, you can use English words indicating actual business terms. The naming format should be consistent within a cluster.
   -  A variable name must be descriptive and meaningful. It must have a prefix indicating its type.

-  [Proposal] The name of a table object should indicate its main characteristics, for example, whether it is an ordinary, temporary, or unlogged table.

   -  An ordinary table name should indicate the business relevant to a data set.
   -  Temporary tables are named in the format of **tmp\_**\ *Suffix*.
   -  Unlogged tables are named in the format of **ul\_**\ *Suffix*.
   -  Foreign tables are named in the format of **f\_**\ *Suffix*.
