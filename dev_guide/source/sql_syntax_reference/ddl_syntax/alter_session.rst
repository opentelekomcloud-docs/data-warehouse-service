:original_name: dws_06_0139.html

.. _dws_06_0139:

ALTER SESSION
=============

Function
--------

Defines or modifies the conditions or parameters that affect the current session. Modified session parameters are kept until the current session is disconnected.

Precautions
-----------

-  If the **START TRANSACTION** command is not executed before the **SET TRANSACTION** command, the transaction is ended instantly and the command does not take effect.
-  You can use the transaction_mode(s) method declared in the **START TRANSACTION** command to avoid using the **SET TRANSACTION** command.

Syntax
------

-  Set transaction parameters of a session.

   ::

      ALTER SESSION SET [ SESSION CHARACTERISTICS AS ] TRANSACTION
          { ISOLATION LEVEL { READ COMMITTED | READ UNCOMMITTED } | { READ ONLY  | READ WRITE } } [, ...] ;

-  Set other running parameters of a session.

   ::

      ALTER SESSION SET
          {{config_parameter { { TO  | =  }  { value | DEFAULT }
            | FROM CURRENT }} | CURRENT_SCHEMA [ TO | = ] { schema | DEFAULT }
            | TIME ZONE time_zone
            | SCHEMA schema
            | NAMES encoding_name
            | ROLE role_name PASSWORD 'password'
            | SESSION AUTHORIZATION { role_name PASSWORD 'password' | DEFAULT }
            | XML OPTION { DOCUMENT | CONTENT }
          } ;

Parameter Description
---------------------

-  **SESSION**

   Indicates that the specified parameters take effect for the current session. This is the default value if neither **SESSION** nor **LOCAL** appears.

   If **SET** or **SET SESSION** is executed within a transaction that is later aborted, the effects of the **SET** command disappear when the transaction is rolled back. Once the surrounding transaction is committed, the effects will persist until the end of the session, unless overridden by another **SET**.

-  **config_parameter**

   Indicates the configurable run-time parameters. You can use **SHOW ALL** to view available run-time parameters.

   .. note::

      Some parameters that are viewed by **SHOW ALL** cannot be set by **SET**. For example, **max_datanodes**.

-  **value**

   Indicates the new value of the **config_parameter** parameter. This parameter can be specified as string constants, identifiers, numbers, or comma-separated lists of these. **DEFAULT** can be written to indicate resetting the parameter to its default value.

-  **TIME ZONE timezone**

   Indicates the local time zone for the current session.

   Value range: A valid local time zone. The corresponding run-time parameter is **TimeZone**. The default value is **PRC**.

-  **CURRENT_SCHEMA**

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

Examples
--------

Create the **ds** schema.

::

   CREATE SCHEMA ds;

Set the search path of the schema.

::

   SET SEARCH_PATH TO ds, public;

Set the time/date type to the traditional postgres format (date before month).

::

   SET DATESTYLE TO postgres, dmy;

Set the character code of the current session to UTF8.

::

   ALTER SESSION SET NAMES 'UTF8';

Set the time zone to Berkeley of California.

::

   SET TIME ZONE 'PST8PDT';

Set the time zone to Italy.

::

   SET TIME ZONE 'Europe/Rome';

Set the current schema.

::

   ALTER SESSION SET CURRENT_SCHEMA TO tpcds;

Set **XML OPTION** to **DOCUMENT**.

::

   ALTER SESSION SET XML OPTION DOCUMENT;

Create the role **joe**, and set the session role to **joe**.

::

   CREATE ROLE joe WITH PASSWORD '{password}';
   ALTER SESSION SET SESSION AUTHORIZATION joe PASSWORD '{password}';

Switch to the default user.

::

   ALTER SESSION SET SESSION AUTHORIZATION default;

Helpful Links
-------------

:ref:`SET <dws_06_0220>`
