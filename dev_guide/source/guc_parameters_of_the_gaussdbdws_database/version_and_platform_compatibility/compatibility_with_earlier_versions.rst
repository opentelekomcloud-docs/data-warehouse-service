:original_name: dws_04_0930.html

.. _dws_04_0930:

Compatibility with Earlier Versions
===================================

GaussDB(DWS) provides parameter controls for the downward compatibility and external compatibility features of the database. The backward compatibility of the database system can provide support for old versions of database applications. The parameters introduced in this section mainly control the backward compatibility of the database.

array_nulls
-----------

**Parameter description**: Determines whether the array input parser recognizes unquoted NULL as a null array element.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that null values can be entered in arrays.
-  **off** indicates backward compatibility with the old behavior. Arrays containing **NULL** values can still be created when this parameter is set to **off**.

**Default value**: **on**

backslash_quote
---------------

**Parameter description**: Determines whether a single quotation mark can be represented by \\' in a string text.

**Type**: USERSET

.. important::

   When the string text meets the SQL standards, \\ has no other meanings. This parameter only affects the handling of non-standard-conforming string texts, including escape string syntax (E'...').

**Value range**: enumerated values

-  **on** indicates that the use of \\' is always allowed.
-  **off** indicates that the use of \\' is rejected.
-  **safe_encoding** indicates that the use of \\' is allowed only when client encoding does not allow ASCII \\ within a multibyte character.

**Default value**: **safe_encoding**

default_with_oids
-----------------

**Parameter description**: Determines whether **CREATE TABLE** and **CREATE TABLE AS** include an **OID** field in newly-created tables if neither **WITH OIDS** nor **WITHOUT OIDS** is specified. It also determines whether OIDs will be included in tables created by **SELECT INTO**.

It is not recommended that OIDs be used in user tables. Therefore, this parameter is set to **off** by default. When OIDs are required for a particular table, **WITH OIDS** needs to be specified during the table creation.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates **CREATE TABLE** and **CREATE TABLE AS** can include an **OID** field in newly-created tables.
-  **off** indicates **CREATE TABLE** and **CREATE TABLE AS** cannot include any **OID** field in newly-created tables.

**Default value**: **off**

escape_string_warning
---------------------

**Parameter description**: Specifies a warning on directly using a backslash (\\) as an escape in an ordinary character string.

-  Applications that wish to use a backslash (\\) as an escape need to be modified to use escape string syntax (E'...'). This is because the default behavior of ordinary character strings is now to treat the backslash as an ordinary character in each SQL standard.
-  This variable can be enabled to help locate codes that need to be changed.

**Type**: USERSET

**Value range**: Boolean

**Default value**: **on**

lo_compat_privileges
--------------------

**Parameter description**: Determines whether to enable backward compatibility for the privilege check of large objects.

**Type**: SUSET

**Value range**: Boolean

**on** indicates that the privilege check is disabled when users read or modify large objects. This setting is compatible with versions earlier than PostgreSQL 9.0.

**Default value**: **off**

quote_all_identifiers
---------------------

**Parameter description**: When the database generates SQL, this parameter forcibly quotes all identifiers even if they are not keywords. This will affect the output of EXPLAIN as well as the results of functions, such as pg_get_viewdef. For details, see the **--quote-all-identifiers** parameter of **gs_dump**.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates the forcible quotation function is enabled.
-  **off** indicates the forcible quotation function is disabled.

**Default value**: **off**

sql_inheritance
---------------

**Parameter description**: Determines whether to inherit semantics.

**Type**: USERSET

**Value range**: Boolean

**off** indicates that child tables cannot be accessed by various commands. That is, an ONLY keyword is used by default. This setting is compatible with versions earlier than PostgreSQL 7.1.

**Default value**: **on**

standard_conforming_strings
---------------------------

**Parameter description**: Determines whether ordinary string texts ('...') treat backslashes as ordinary texts as specified in the SQL standard.

-  Applications can check this parameter to determine how string texts will be processed.
-  It is recommended that characters be escaped by using the escape string syntax (E'...').

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the function is enabled.
-  **off** indicates that the function is disabled.

**Default value**: **on**

synchronize_seqscans
--------------------

**Parameter description**: Controls sequential scans of tables to synchronize with each other. Concurrent scans read the same data block about at the same time and share the I/O workload.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that a scan may start in the middle of the table and then "wrap around" the end to cover all rows to synchronize with the activity of scans already in progress. This may result in unpredictable changes in the row ordering returned by queries that have no ORDER BY clause.
-  **off** indicates that the scan always starts from the table heading.

**Default value**: **on**

enable_beta_features
--------------------

**Parameter description**: Controls whether certain limited features, such as GDS table join, are available. These features are not explicitly prohibited in earlier versions, but are not recommended due to their limitations in certain scenarios.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the features are enabled and forward compatible, but may incur errors in certain scenarios.
-  **off** indicates that the features are disabled.

**Default value**: **off**
