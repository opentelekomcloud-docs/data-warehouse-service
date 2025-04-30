:original_name: dws_04_0025.html

.. _dws_04_0025:

Creating and Managing GaussDB(DWS) Databases
============================================

A database is a collection of objects such as tables, indexes, views, stored procedures, and operators. GaussDB (DWS) supports the creation of multiple databases. However, a client program can connect to and access only one database at a time, and cross-database query is not supported.

Template and Default Databases
------------------------------

-  GaussDB (DWS) provides two template databases **template0** and **template1** and a default database gaussdb.
-  By default, each newly created database is based on a template database. The GaussDB(DWS) database uses **template1** as the template by default. The encoding format is SQL_ASCII, and user-defined character encoding is not allowed. If you need to specify the character encoding when creating a database, use **template0** to create the database.
-  Do not use a client or any other tools to connect to or to perform operations on both the two template databases.

   .. note::

      You can run the **show server_encoding** command to view the current database encoding.

Creating a Database.
--------------------

Run the **CREATE DATABASE** statement to create a database.

::

   CREATE DATABASE mydatabase;

.. note::

   -  When you create a database, if the length of the database name exceeds 63 bytes, the server truncates the database name and retains the first 63 bytes. Therefore, you are advised to set the length of the database name to a value less than or equal to 63 bytes. Do not use multi-byte characters as object names. If an object whose name is truncated mistakenly cannot be deleted, delete the object using the name before the truncation, or manually delete it from the corresponding system catalog on each node.
   -  Database names must comply with the naming convention of SQL identifiers. The current user automatically becomes the owner of this new database.
   -  If a database system is used to support independent users and projects, store them in different databases.
   -  If the projects or users are associated with each other and share resources, store them in different schemas in the same database.
   -  A maximum of 128 databases can be created in GaussDB(DWS).
   -  You must have the permission to create a database or the permission that the system administrator owns.

Viewing Databases
-----------------

To view databases, perform the following steps:

-  Run the **\\l** meta-command to view the database list of the database system.

   ::

      \l

-  Querying the database list using the **pg_database** system catalog

   ::

      SELECT datname FROM pg_database;

Modifying a Database
--------------------

You can use the **ALTER DATABASE** statement modify database configuration such as the database owner, name, and default settings.

-  Run the following command to set the default search path for the database:

   ::

      ALTER DATABASE mydatabase SET search_path TO pa_catalog,public;

-  Rename the database.

   ::

      ALTER DATABASE mydatabase RENAME TO newdatabase;

Deleting a Database
-------------------

You can run **DROP DATABASE** statement to delete a database. This statement deletes the system catalog of the database and the database directory on the disk. Only the database owner or system administrator can delete a database. A database being accessed by users cannot be deleted, You need to connect to another database before deleting this database.

Run the **DROP DATABASE** statement to delete a database:

::

   DROP DATABASE newdatabase;
