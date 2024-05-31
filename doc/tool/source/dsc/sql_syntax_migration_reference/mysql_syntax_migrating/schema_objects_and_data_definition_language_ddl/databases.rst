:original_name: dws_16_0176.html

.. _dws_16_0176:

.. _en-us_topic_0000001819416265:

Databases
=========

In MySQL, **DATABASE** is a schema object, which is equivalent to the **SCHEMA** of Oracle and GaussDB(DWS). DSC supports the following two scenarios:

#. Database creation

   **Input**

   .. code-block::

      create database IF NOT EXISTS dbname1 CHARACTER SET=utf8 COLLATE=utf8_unicode_ci;
      create database IF NOT EXISTS dbname2;

      drop database if exists dbname1;
      drop database if exists dbname2;

   **Output**

   .. code-block::

      CREATE SCHEMA "dbname1";
      CREATE SCHEMA "dbname2";

      DROP SCHEMA IF EXISTS "dbname1";
      DROP SCHEMA IF EXISTS "dbname2";

#. Database use

   **Input**

   .. code-block::

      drop database if exists test;
      create database if not exists test;
      use test;

   **Output**

   .. code-block::

      DROP SCHEMA IF EXISTS "test";
      CREATE SCHEMA "test";
      SET CURRENT_SCHEMA = "test";
