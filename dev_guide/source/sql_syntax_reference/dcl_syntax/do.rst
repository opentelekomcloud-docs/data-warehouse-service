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

-  **lang_name**

   Parses the programming language used by the code. If not specified, the default value **plpgsql** is used.

-  **code**

   Specifies executable programming language code. The language is specified as a string.

Examples
--------

Grant user **webuser** all the operation permissions on views in the **tpcds** schema:

::

   DO $$DECLARE r record;
   BEGIN
       FOR r IN SELECT c.relname,n.nspname FROM pg_class c,pg_namespace n
                WHERE c.relnamespace = n.oid AND n.nspname = 'tpcds' AND relkind IN ('r','v')
       LOOP
           EXECUTE 'GRANT ALL ON ' || quote_ident(r.table_schema) || '.' || quote_ident(r.table_name) || ' TO webuser';
       END LOOP;
   END$$;
