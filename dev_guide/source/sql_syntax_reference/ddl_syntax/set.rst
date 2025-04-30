:original_name: dws_06_0220.html

.. _dws_06_0220:

SET
===

Function
--------

**SET** modifies a run-time parameter.

Precautions
-----------

Most run-time parameters can be modified by executing **SET**. Some parameters cannot be modified after a server or session starts.

Syntax
------

-  Set the system time zone.

   ::

      SET [ SESSION | LOCAL ] TIME ZONE { timezone | LOCAL | DEFAULT };

-  Set the schema of the table.

   ::

      SET [ SESSION | LOCAL ]
          {CURRENT_SCHEMA { TO | = } { schema | DEFAULT }
          | SCHEMA 'schema'};

-  Set client encoding.

   ::

      SET [ SESSION | LOCAL ] NAMES encoding_name;

-  Set XML parsing mode.

   ::

      SET [ SESSION | LOCAL ] XML OPTION { DOCUMENT | CONTENT };

-  Set other running parameters.

   ::

      SET [ LOCAL | SESSION ]
          { {config_parameter { { TO | = } { value | DEFAULT }
                              | FROM CURRENT }}};

Parameter Description
---------------------

-  **SESSION**

   Indicates that the specified parameters take effect for the current session. This is the default value if neither **SESSION** nor **LOCAL** appears.

   If **SET** or **SET SESSION** is executed within a transaction that is later aborted, the effects of the **SET** command disappear when the transaction is rolled back. Once the surrounding transaction is committed, the effects will persist until the end of the session, unless overridden by another **SET**.

-  **LOCAL**

   Indicates that the specified parameters take effect for the current transaction. After **COMMIT** or **ROLLBACK**, the session-level setting takes effect again.

   The effects of **SET LOCAL** last only till the end of the current transaction, whether committed or not. A special case is **SET** followed by **SET LOCAL** within a single transaction: the **SET LOCAL** value will be seen until the end of the transaction, but afterwards (if the transaction is committed) the **SET** value will take effect.

-  **TIME ZONE timezone**

   Indicates the local time zone for the current session.

   Value range: A valid local time zone. The corresponding run-time parameter is **TimeZone**. The default value is **PRC**.

-  **CURRENT_SCHEMA**

   **schema**

   Indicates the current schema.

   Value range: An existing schema name.

-  **SCHEMA schema**

   Indicates the current schema. Here the schema is a string.

   Example: set schema 'public';

-  **NAMES encoding_name**

   Indicates the client character encoding name. This command is equivalent to **set client_encoding to encoding_name**.

   Value range: A valid character encoding name. The run-time parameter corresponding to this option is **client_encoding**. The default encoding is **UTF8**.

-  **XML OPTION option**

   Indicates the XML resolution mode.

   Value range: **CONTENT** (default), **DOCUMENT**

-  **config_parameter**

   Indicates the configurable run-time parameters. You can use **SHOW ALL** to view available run-time parameters.

   .. note::

      Some parameters that viewed by **SHOW ALL** cannot be set by **SET**. For example, **max_datanodes**.

-  **value**

   Indicates the new value of the **config_parameter** parameter. This parameter can be specified as string constants, identifiers, numbers, or comma-separated lists of these. **DEFAULT** can be written to indicate resetting the parameter to its default value.

Examples
--------

Configure the search path of the **tpcds** schema:

::

   SET search_path TO tpcds, public;

Set the date style to the traditional POSTGRES style (date placed before month).

::

   SET datestyle TO postgres;

Helpful Links
-------------

:ref:`RESET <dws_06_0219>`, :ref:`SHOW <dws_06_0224>`
