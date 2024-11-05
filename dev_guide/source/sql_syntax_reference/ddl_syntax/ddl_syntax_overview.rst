:original_name: dws_06_0119.html

.. _dws_06_0119:

DDL Syntax Overview
===================

Data definition language (DDL) is used to define or modify an object in a database, such as a table, index, or view.

.. note::

   GaussDB(DWS) does not support DDL if its CN is unavailable. For example, if a CN in the cluster is faulty, creating a database or table will fail.

Defining a Database
-------------------

A database is the warehouse for organizing, storing, and managing data. Defining a database includes: creating a database, altering the database attributes, and dropping the database. The following table lists the related SQL statements.

.. table:: **Table 1** SQL statements for defining a database

   ========================= ====================================
   Function                  SQL Statement
   ========================= ====================================
   Create a database         :ref:`CREATE DATABASE <dws_06_0156>`
   Alter database attributes :ref:`ALTER DATABASE <dws_06_0120>`
   Delete a database         :ref:`DROP DATABASE <dws_06_0189>`
   ========================= ====================================

Defining a Schema
-----------------

A schema is the set of a group of database objects and is used to control the access to the database objects. The following table lists the related SQL statements.

.. table:: **Table 2** SQL statements for defining a schema

   ======================= ==================================
   Function                SQL Statement
   ======================= ==================================
   Create a schema         :ref:`CREATE SCHEMA <dws_06_0173>`
   Alter schema attributes :ref:`ALTER SCHEMA <dws_06_0136>`
   Delete a schema         :ref:`DROP SCHEMA <dws_06_0204>`
   ======================= ==================================

Defining a Table
----------------

A table is a special data structure in a database and is used to store data objects and the relationship between data objects. The following table lists the related SQL statements.

.. table:: **Table 3** SQL statements for defining a table

   ================================ =================================
   Function                         SQL Statement
   ================================ =================================
   Create a table                   :ref:`CREATE TABLE <dws_06_0177>`
   Alter table attributes           :ref:`ALTER TABLE <dws_06_0142>`
   Alter a table name               :ref:`RENAME TABLE <dws_06_0276>`
   Delete a table                   :ref:`DROP TABLE <dws_06_0208>`
   Delete all the data from a table :ref:`TRUNCATE <dws_06_0225>`
   ================================ =================================

Defining a Partitioned Table
----------------------------

A partitioned table is a special data structure in a database and is used to store data objects and the relationship between data objects. The following table lists the related SQL statements.

.. table:: **Table 4** SQL statements for defining a partitioned table

   +------------------------------------+---------------------------------------------+
   | Function                           | SQL Statement                               |
   +====================================+=============================================+
   | Create a partitioned table         | :ref:`CREATE TABLE PARTITION <dws_06_0179>` |
   +------------------------------------+---------------------------------------------+
   | Editing a partition                | :ref:`ALTER TABLE PARTITION <dws_06_0143>`  |
   +------------------------------------+---------------------------------------------+
   | Alter partitioned table attributes | :ref:`ALTER TABLE PARTITION <dws_06_0143>`  |
   +------------------------------------+---------------------------------------------+
   | Delete a partition                 | :ref:`ALTER TABLE PARTITION <dws_06_0143>`  |
   +------------------------------------+---------------------------------------------+
   | Delete a partitioned table         | :ref:`DROP TABLE <dws_06_0208>`             |
   +------------------------------------+---------------------------------------------+

Defining an Index
-----------------

An index indicates the sequence of values in one or more columns in the database table. The database index is a data structure that improves the speed of data access to specific information in a database table. The following table lists the related SQL statements.

.. table:: **Table 5** SQL statements for defining an index

   ====================== =================================
   Function               SQL Statement
   ====================== =================================
   Create an index        :ref:`CREATE INDEX <dws_06_0165>`
   Alter index attributes :ref:`ALTER INDEX <dws_06_0128>`
   Delete an index        :ref:`DROP INDEX <dws_06_0195>`
   Rebuild an index       :ref:`REINDEX <dws_06_0218>`
   ====================== =================================

Defining a Role
---------------

A role is used to manage rights. For database security, all management and operation rights can be assigned to different roles. The following table lists the related SQL statements.

.. table:: **Table 6** SQL statements for defining a role

   ===================== ================================
   Function              SQL Statement
   ===================== ================================
   Create a role         :ref:`CREATE ROLE <dws_06_0172>`
   Alter role attributes :ref:`ALTER ROLE <dws_06_0134>`
   Delete a role         :ref:`DROP ROLE <dws_06_0203>`
   ===================== ================================

Defining a User
---------------

A user is used to log in to a database. Different rights can be assigned to users for managing data accesses and operations of users. The following table lists the related SQL statements.

.. table:: **Table 7** SQL statements for defining a user

   ===================== ================================
   Function              SQL Statement
   ===================== ================================
   Create a user         :ref:`CREATE USER <dws_06_0186>`
   Alter user attributes :ref:`ALTER USER <dws_06_0149>`
   Delete a user         :ref:`DROP USER <dws_06_0214>`
   ===================== ================================

Defining a Redaction Policy
---------------------------

Data redaction is to protect sensitive data by masking or changing data. You can create a data redaction policy for a specific table object and specify the effective scope of the policy. You can also add, modify, and delete redaction columns. The following table lists the related SQL statements.

.. table:: **Table 8** SQL statements for managing redaction policies

   +-------------------------------------------------------------+----------------------------------------------+
   | Function                                                    | SQL Statement                                |
   +=============================================================+==============================================+
   | Create a data redaction policy                              | :ref:`CREATE REDACTION POLICY <dws_06_0168>` |
   +-------------------------------------------------------------+----------------------------------------------+
   | Modify a data redaction policy applied to a specified table | :ref:`ALTER REDACTION POLICY <dws_06_0132>`  |
   +-------------------------------------------------------------+----------------------------------------------+
   | Delete a data redaction policy applied to a specified table | :ref:`DROP REDACTION POLICY <dws_06_0199>`   |
   +-------------------------------------------------------------+----------------------------------------------+

Defining Row-Level Access Control
---------------------------------

Row-level access control policies control the visibility of rows in database tables. In this way, the same SQL query may return different results for different users. The following table lists the related SQL statements.

.. table:: **Table 9** SQL statements for row-level access control

   +-------------------------------------------------------+-------------------------------------------------------+
   | Function                                              | SQL Statement                                         |
   +=======================================================+=======================================================+
   | Create a row-level access control policy              | :ref:`CREATE ROW LEVEL SECURITY POLICY <dws_06_0169>` |
   +-------------------------------------------------------+-------------------------------------------------------+
   | Modify an existing row-level access control policy    | :ref:`ALTER ROW LEVEL SECURITY POLICY <dws_06_0135>`  |
   +-------------------------------------------------------+-------------------------------------------------------+
   | Delete a row-level access control policy from a table | :ref:`DROP ROW LEVEL SECURITY POLICY <dws_06_0200>`   |
   +-------------------------------------------------------+-------------------------------------------------------+

Defining a Stored Procedure
---------------------------

A stored procedure is a set of SQL statements for achieving specific functions and is stored in the database after compiling. Users can specify a name and provide parameters (if necessary) to execute the stored procedure. The following table lists the related SQL statements.

.. table:: **Table 10** SQL statements for defining a stored procedure

   ========================= =====================================
   Function                  SQL Statement
   ========================= =====================================
   Create a stored procedure :ref:`CREATE PROCEDURE <dws_06_0170>`
   Delete a stored procedure :ref:`DROP PROCEDURE <dws_06_0201>`
   ========================= =====================================

Defining a Function
-------------------

In GaussDB(DWS), a function is similar to a stored procedure, which is a set of SQL statements. The function and stored procedure are used the same. The following table lists the related SQL statements.

.. table:: **Table 11** SQL statements for defining a function

   ========================= ====================================
   Function                  SQL Statement
   ========================= ====================================
   Create a function         :ref:`CREATE FUNCTION <dws_06_0163>`
   Alter function attributes :ref:`ALTER FUNCTION <dws_06_0126>`
   Delete a function         :ref:`DROP FUNCTION <dws_06_0193>`
   ========================= ====================================

Defining a View
---------------

A view is a virtual table exported from one or several basic tables. The view is used to control data accesses for users. The following table lists the related SQL statements.

.. table:: **Table 12** SQL statements for defining a view

   ============= ================================
   Function      SQL Statement
   ============= ================================
   Create a view :ref:`CREATE VIEW <dws_06_0187>`
   Delete a view :ref:`DROP VIEW <dws_06_0215>`
   ============= ================================

Defining a Cursor
-----------------

To process SQL statements, the stored procedure process assigns a memory segment to store context association. Cursors are handles or pointers to context regions. With a cursor, the stored procedure can control alterations in context areas.

.. table:: **Table 13** SQL statements for defining a cursor

   ========================== ===========================
   Function                   SQL Statement
   ========================== ===========================
   Create a cursor            :ref:`CURSOR <dws_06_0188>`
   Move a cursor              :ref:`MOVE <dws_06_0217>`
   Extract data from a cursor :ref:`FETCH <dws_06_0216>`
   Close a cursor             :ref:`CLOSE <dws_06_0152>`
   ========================== ===========================

Altering or Ending a Session
----------------------------

A session is a connection established between the user and the database. The following table lists the related SQL statements.

.. table:: **Table 14** SQL statements related to sessions

   =============== ==============================================
   Function        SQL Statement
   =============== ==============================================
   Alter a session :ref:`ALTER SESSION <dws_06_0139>`
   End a session   :ref:`ALTER SYSTEM KILL SESSION <dws_06_0141>`
   =============== ==============================================

Defining a Resource Pool
------------------------

A resource pool is a system catalog used by the resource load management module to specify attributes related to resource management, such as Cgroups. The following table lists the related SQL statements.

.. table:: **Table 15** SQL statements for defining a resource pool

   ========================== =========================================
   Function                   SQL Statement
   ========================== =========================================
   Create a resource pool     :ref:`CREATE RESOURCE POOL <dws_06_0171>`
   Change resource attributes :ref:`ALTER RESOURCE POOL <dws_06_0133>`
   Delete a resource pool     :ref:`DROP RESOURCE POOL <dws_06_0202>`
   ========================== =========================================

Defining Synonyms
-----------------

A synonym is a special database object compatible with Oracle. It is used to store the mapping between a database object and another. Currently, only synonyms can be used to associate the following database objects: tables, views, functions, and stored procedures. The following table lists the related SQL statements.

.. table:: **Table 16** SQL statements for managing synonyms

   =================== ===================================
   Function            SQL Statement
   =================== ===================================
   Creating a synonym  :ref:`CREATE SYNONYM <dws_06_0176>`
   Modifying a synonym :ref:`ALTER SYNONYM <dws_06_0140>`
   Deleting a synonym  :ref:`DROP SYNONYM <dws_06_0207>`
   =================== ===================================

Defining Text Search Configuration
----------------------------------

A text search configuration specifies a text search parser that can divide a string into tokens, plus dictionaries that can be used to determine which tokens are of interest for searching. The following table lists the related SQL statements.

.. table:: **Table 17** SQL statements for configuring text search

   +------------------------------------+-------------------------------------------------------+
   | Function                           | SQL Statement                                         |
   +====================================+=======================================================+
   | Create a text search configuration | :ref:`CREATE TEXT SEARCH CONFIGURATION <dws_06_0182>` |
   +------------------------------------+-------------------------------------------------------+
   | Modify a text search configuration | :ref:`ALTER TEXT SEARCH CONFIGURATION <dws_06_0145>`  |
   +------------------------------------+-------------------------------------------------------+
   | Delete a text search configuration | :ref:`DROP TEXT SEARCH CONFIGURATION <dws_06_0210>`   |
   +------------------------------------+-------------------------------------------------------+

Defining a Full-text Retrieval Dictionary
-----------------------------------------

A dictionary is used to identify and process specific words during full-text retrieval. Dictionaries are created by using predefined templates (defined in the **PG_TS_TEMPLATE** system catalog). Five types of dictionaries can be created, **Simple**, **Ispell**, **Synonym**, **Thesaurus**, and **Snowball**. Each type of dictionaries is used to handle different tasks. The following table lists the related SQL statements.

.. table:: **Table 18** SQL statements for a full-text search dictionary

   +-----------------------------------------+----------------------------------------------------+
   | Function                                | SQL Statement                                      |
   +=========================================+====================================================+
   | Create a full-text retrieval dictionary | :ref:`CREATE TEXT SEARCH DICTIONARY <dws_06_0183>` |
   +-----------------------------------------+----------------------------------------------------+
   | Modify a full-text retrieval dictionary | :ref:`ALTER TEXT SEARCH DICTIONARY <dws_06_0146>`  |
   +-----------------------------------------+----------------------------------------------------+
   | Delete a full-text retrieval dictionary | :ref:`DROP TEXT SEARCH DICTIONARY <dws_06_0211>`   |
   +-----------------------------------------+----------------------------------------------------+
