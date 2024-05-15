:original_name: dws_04_0931.html

.. _dws_04_0931:

Platform and Client Compatibility
=================================

Many platforms use the database system. External compatibility of the database system provides a lot of conveniences for platforms.

transform_null_equals
---------------------

**Parameter description**: Determines whether expressions of the form expr = NULL (or NULL = expr) are treated as expr IS NULL. They return true if expr evaluates to **NULL**, and false otherwise.

-  The correct SQL-standard-compliant behavior of expr = NULL is to always return null (unknown).
-  Filtered forms in MS Access generate queries that appear to use expr = NULL to test for null values. If you turn this option on, you can use this interface to access the database.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates expressions of the form expr = NULL (or NULL = expr) are treated as expr IS NULL.
-  **off** indicates expr = NULL always returns NULL.

**Default value**: **off**

.. note::

   New users are always confused about the semantics of expressions involving **NULL** values. Therefore, **off** is used as the default value.

td_compatible_truncation
------------------------

**Parameter description**: Determines whether to enable features compatible with a Teradata database. You can set this parameter to **on** when connecting to a database compatible with the Teradata database, so that when you perform the INSERT operation, overlong strings are truncated based on the allowed maximum length before being inserted into char- and varchar-type columns in the target table. This ensures all data is inserted into the target table without errors reported.

.. note::

   -  The string truncation function cannot be used if the **INSERT** statement includes a foreign table.
   -  If inserting multi-byte character data (such as Chinese characters) to database with the character set byte encoding (SQL_ASCII, LATIN1), and the character data crosses the truncation position, the string is truncated based on its bytes instead of characters. Unexpected result will occur in tail after the truncation. If you want correct truncation result, you are advised to adopt encoding set such as UTF8, which has no character data crossing the truncation position.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates overlong strings are truncated.
-  **off** indicates overlong strings are not truncated.

**Default value**: **off**
