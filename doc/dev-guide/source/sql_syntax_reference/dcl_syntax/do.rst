:original_name: dws_06_0247.html

.. _dws_06_0247:

DO
==

Function
--------

**DO** executes an anonymous code block.

A code block is a function body without parameters. Its return type is **void**. It is analyzed and executed at the same time.

Precautions
-----------

-  Before using a programming language, install it in the current database using **CREATE LANGUAGE**. If no language is specified, **plpgsql** is installed by default.
-  To use an untrusted language, you must be a system administrator or have the USAGE permission for programming languages.

Syntax
------

::

   DO [ LANGUAGE lang_name ] code;

Parameter Description
---------------------

.. table:: **Table 1** DO parameters

   +-----------+------------------------------------------------------+-------------------------------------------------------------------------------------------+
   | Parameter | Description                                          | Value Range                                                                               |
   +===========+======================================================+===========================================================================================+
   | lang_name | Name of the programming language used to parse code. | Program language that complies with the specifications. The default language is PL/pgSQL. |
   +-----------+------------------------------------------------------+-------------------------------------------------------------------------------------------+
   | code      | Programming language code that can be executed.      | A string.                                                                                 |
   +-----------+------------------------------------------------------+-------------------------------------------------------------------------------------------+

Examples
--------

Grant user **webuser** all the operation permissions on views in the **tpcds** schema:

::

   DO $$DECLARE r record;
   BEGIN
       FOR r IN SELECT c.relname  AS table_name ,n.nspname AS table_schema FROM pg_class c,pg_namespace n
                WHERE c.relnamespace = n.oid AND n.nspname = 'tpcds' AND relkind IN ('r','v')
       LOOP
           EXECUTE 'GRANT ALL ON ' || quote_ident(r.table_schema) || '.' || quote_ident(r.table_name) || ' TO webuser';
       END LOOP;
   END$$;
