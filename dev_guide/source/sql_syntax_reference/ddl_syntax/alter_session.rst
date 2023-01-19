:original_name: dws_06_0139.html

.. _dws_06_0139:

ALTER SESSION
=============

Function
--------

**ALTER SESSION** defines or modifies the conditions or parameters that affect the current session. Modified session parameters are kept until the current session is disconnected.

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

To modify the description of parameters related to the session, see :ref:`Parameter Description <en-us_topic_0000001145510673__se8530cae21fd4932a87b96aedaebc0a9>` of the **SET** syntax.

Examples
--------

Create the **ds** schema.

.. code-block::

   CREATE SCHEMA ds;

Set the search path of the schema.

.. code-block::

   SET SEARCH_PATH TO ds, public;

Set the time/date type to the traditional postgres format (date before month).

.. code-block::

   SET DATESTYLE TO postgres, dmy;

Set the character code of the current session to UTF8.

.. code-block::

   ALTER SESSION SET NAMES 'UTF8';

Set the time zone to Berkeley of California.

.. code-block::

   SET TIME ZONE 'PST8PDT';

Set the time zone to Italy.

.. code-block::

   SET TIME ZONE 'Europe/Rome';

Set the current schema.

.. code-block::

   ALTER SESSION SET CURRENT_SCHEMA TO tpcds;

Set **XML OPTION** to **DOCUMENT**.

.. code-block::

   ALTER SESSION SET XML OPTION DOCUMENT;

Create the role **joe**, and set the session role to **joe**.

.. code-block::

   CREATE ROLE joe WITH PASSWORD '{password}';
   ALTER SESSION SET SESSION AUTHORIZATION joe PASSWORD '{password}';

Switch to the default user.

.. code-block::

   ALTER SESSION SET SESSION AUTHORIZATION default;

Helpful Links
-------------

:ref:`SET <dws_06_0220>`
