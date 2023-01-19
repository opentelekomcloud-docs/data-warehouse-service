:original_name: dws_04_0925.html

.. _dws_04_0925:

Statement Behavior
==================

This section describes related default parameters involved in the execution of SQL statements.

.. _en-us_topic_0000001145894759__sc0cb61109de045049d5802f668be2392:

search_path
-----------

**Parameter description**: Specifies the order in which schemas are searched when an object is referenced with no schema specified. The value of this parameter consists of one or more schema names. Different schema names are separated by commas (,).

**Type**: USERSET

-  If the schema of a temporary table exists in the current session, the scheme can be listed in **search_path** by using the alias **pg_temp**, for example, **'pg_temp,public'**. The schema of a temporary table has the highest search priority and is always searched before all the schemas specified in **pg_catalog** and **search_path**. Therefore, do not explicitly specify **pg_temp** to be searched after other schemas in **search_path**. This setting will not take effect and an error message will be displayed. If the alias **pg_temp** is used, the temporary schema will be only searched for database objects, including tables, views, and data types. Functions or operator names will not be searched for.
-  The schema of a system catalog, **pg_catalog**, has the second highest search priority and is the first to be searched among all the schemas, excluding **pg_temp**, specified in **search_path**. Therefore, do not explicitly specify **pg_catalog** to be searched after other schemas in **search_path**. This setting will not take effect and an error message will be displayed.
-  When an object is created without specifying a particular schema, the object will be placed in the first valid schema listed in **search_path**. An error will be reported if the search path is empty.
-  The current effective value of the search path can be examined through the SQL function current_schema. This is different from examining the value of **search_path**, because the current_schema function displays the first valid schema name in **search_path**.

**Value range**: a string

.. note::

   -  When this parameter is set to **"$user", public**, a database can be shared (where no users have private schemas, and all share use of public), and private per-user schemas and combinations of them are supported. Other effects can be obtained by modifying the default search path setting, either globally or per-user.
   -  When this parameter is set to a null string (''), the system automatically converts it into a pair of double quotation marks ("").
   -  If the content contains double quotation marks, the system considers them as insecure characters and converts each double quotation mark into a pair of double quotation marks.

**Default value**: **"$user",public**

.. note::

   **$user** indicates the name of the schema with the same name as the current session user. If the schema does not exist, **$user** will be ignored.

current_schema
--------------

**Parameter description**: Specifies the current schema.

**Type**: USERSET

**Value range**: a string

**Default value**: **"$user",public**

.. note::

   **$user** indicates the name of the schema with the same name as the current session user. If the schema does not exist, **$user** will be ignored.

.. _en-us_topic_0000001145894759__s2bc15c6041414a058ad5e1738739a120:

default_tablespace
------------------

**Parameter description**: Specifies the default tablespace of the created objects (tables and indexes) when a **CREATE** command does not explicitly specify a tablespace.

-  The value of this parameter is either the name of a tablespace, or an empty string that specifies the use of the default tablespace of the current database. If a non-default tablespace is specified, users must have CREATE privilege for it. Otherwise, creation attempts will fail.
-  This parameter is not used for temporary tables. For them, the :ref:`temp_tablespaces <en-us_topic_0000001145894759__sf6153b3838a045159743f7f32e69ffa3>` is consulted instead.
-  This parameter is not used when users create databases. By default, a new database inherits its tablespace setting from the template database.

**Type**: USERSET

**Value range**: a string. An empty string indicates that the default tablespace is used.

**Default value**: empty

default_storage_nodegroup
-------------------------

**Parameter description**: Specifies the Node Group where a table is created by default. This parameter takes effect only for ordinary tables.

-  **installation** indicates that tables will be created in the Node Group created during database installation.
-  A value other than **installation** indicates that tables will be created in the Node Group specified by this parameter.

**Type**: USERSET

**Value range**: a string

**Default value**: **installation**

default_colversion
------------------

**Parameter description**: Sets the storage format version of the column-store table that is created by default.

**Type**: SIGHUP

**Value range**: enumerated values

-  **1.0**: Each column in a column-store table is stored in a separate file. The file name is **relfilenode.C1.0**, **relfilenode.C2.0**, **relfilenode.C3.0**, or similar.
-  **2.0**: All columns of a column-store table are combined and stored in a file. The file is named **relfilenode.C1.0**.

**Default value:** **2.0**

.. _en-us_topic_0000001145894759__sf6153b3838a045159743f7f32e69ffa3:

temp_tablespaces
----------------

**Parameter description**: Specifies tablespaces to which temporary objects will be created (temporary tables and their indexes) when a **CREATE** command does not explicitly specify a tablespace. Temporary files for sorting large data are created in these tablespaces.

The value of this parameter is a list of names of tablespaces. When there is more than one name in the list, GaussDB(DWS) chooses a random tablespace from the list upon the creation of a temporary object each time. Except that within a transaction, successively created temporary objects are placed in successive tablespaces in the list. If the element selected from the list is an empty string, GaussDB(DWS) will automatically use the default tablespace of the current database instead.

**Type**: USERSET

**Value range**: a string An empty string indicates that all temporary objects are created only in the default tablespace of the current database. For details, see :ref:`default_tablespace <en-us_topic_0000001145894759__s2bc15c6041414a058ad5e1738739a120>`.

**Default value**: empty

check_function_bodies
---------------------

**Parameter description**: Specifies whether to enable validation of the function body string during the execution of **CREATE FUNCTION**. Verification is occasionally disabled to avoid problems, such as forward references when you restore function definitions from a dump.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that validation of the function body string is enabled during the execution of **CREATE FUNCTION**.
-  **off** indicates that validation of the function body string is disabled during the execution of **CREATE FUNCTION**.

**Default value**: **on**

default_transaction_isolation
-----------------------------

**Parameter description**: Specifies the default isolation level of each transaction.

**Type**: USERSET

**Value range**: enumerated values

-  **READ COMMITTED**: Only committed data is read. This is the default.
-  **READ UNCOMMITTED**: GaussDB(DWS) does not support **READ UNCOMMITTED**. If **READ UNCOMMITTED** is set, **READ COMMITTED** is executed instead.
-  **REPEATABLE READ**: Only the data committed before transaction start is read. Uncommitted data or data committed in other concurrent transactions cannot be read.
-  **SERIALIZABLE**: GaussDB(DWS) does not support **SERIALIZABLE**. If **SERIALIZABLE** is set, **REPEATABLE READ** is executed instead.

**Default value**: **READ COMMITTED**

default_transaction_read_only
-----------------------------

**Parameter description**: Specifies whether each new transaction is in read-only state.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates the transaction is in read-only state.
-  **off** indicates the transaction is in read/write state.

**Default value**: **off**

.. _en-us_topic_0000001145894759__s05ef9312d74143928830d7d459cdc63a:

default_transaction_deferrable
------------------------------

**Parameter description**: Specifies the default delaying state of each new transaction. It currently has no effect on read-only transactions or those running at isolation levels lower than serializable.

GaussDB(DWS) does not support the serializable isolation level of each transaction. The parameter is insignificant.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates a transaction is delayed by default.
-  **off** indicates a transaction is not delayed by default.

**Default value**: **off**

session_replication_role
------------------------

**Parameter description**: Specifies the behavior of replication-related triggers and rules for the current session.

**Type**: USERSET

.. important::

   Setting this parameter will discard all the cached execution plans.

**Value range**: enumerated values

-  **origin** indicates that the system copies operations such as insert, delete, and update from the current session.
-  **replica** indicates that the system copies operations such as insert, delete, and update from other places to the current session.
-  **local** indicates that the system will detect the role that has logged in to the database when using the function to copy operations and will perform related operations.

**Default value**: **origin**

statement_timeout
-----------------

**Parameter description**: If the statement execution time (starting when the server receives the command) is longer than the duration specified by the parameter, error information is displayed when you attempt to execute the statement and the statement then exits.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 2147483647. The unit is ms.

**Default value**: **0**

vacuum_freeze_min_age
---------------------

**Parameter description**: Specifies the minimum cutoff age (in the same transaction), based on which **VACUUM** decides whether to replace transaction IDs with FrozenXID while scanning a table.

**Type**: USERSET

**Value range**: an integer from 0 to 576460752303423487.

.. note::

   Although you can set this parameter to a value ranging from **0** to **1000000000** anytime, **VACUUM** will limit the effective value to half the value of autovacuum_freeze_max_age by default.

**Default value**: **5000000000**

vacuum_freeze_table_age
-----------------------

**Parameter description**: Specifies the time that VACUUM freezes tuples while scanning the whole table. **VACUUM** performs a whole-table scan if the value of the :ref:`pg_class <dws_04_0578>`\ **.relfrozenxid** column of the table has reached the specified time.

**Type**: USERSET

**Value range**: an integer from 0 to 576460752303423487.

.. note::

   Although users can set this parameter to a value ranging from **0** to **2000000000** anytime, **VACUUM** will limit the effective value to 95% of autovacuum_freeze_max_age by default. Therefore, a periodic manual VACUUM has a chance to run before an anti-wraparound autovacuum is launched for the table.

**Default value**: **15000000000**

bytea_output
------------

**Parameter description**: Specifies the output format for values of the bytea type.

**Type**: USERSET

**Value range**: enumerated values

-  **hex** indicates the binary data is converted to the two-byte hexadecimal digit.
-  **escape** indicates the traditional PostgreSQL format is used. It takes the approach of representing a binary string as a sequence of ASCII characters, while converting those bytes that cannot be represented as an ASCII character into special escape sequences.

**Default value**: **hex**

xmlbinary
---------

**Parameter description**: Specifies how binary values are to be encoded in XML.

**Type**: USERSET

**Value range**: enumerated values

-  base64
-  hex

**Default value**: **base64**

xmloption
---------

**Parameter description**: Specifies whether DOCUMENT or CONTENT is implicit when converting between XML and string values.

**Type**: USERSET

**Value range**: enumerated values

-  **document** indicates an HTML document.
-  **content** indicates a common string.

**Default value**: **content**

max_compile_functions
---------------------

**Parameter description**: Specifies the maximum number of function compilation results stored in the server. Excessive functions and compilation results generated during the storage may occupy large memory space. Setting this parameter to a proper value can reduce the memory usage and improve system performance.

**Type**: POSTMASTER

**Value range**: an integer ranging from 1 to INT_MAX

**Default value**: **1000**

gin_pending_list_limit
----------------------

**Parameter description**: Specifies the maximum size of the GIN pending list which is used when **fastupdate** is enabled. If the list grows larger than this maximum size, it is cleaned up by moving the entries in it to the main GIN data structure in batches. This setting can be overridden for individual GIN indexes by modifying index storage parameters.

**Type**: USERSET

**Value range**: an integer ranging from 64 to INT_MAX. The unit is KB.

**Default value**: **4 MB**
