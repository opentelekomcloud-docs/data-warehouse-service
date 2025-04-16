:original_name: dws_07_0188.html

.. _dws_07_0188:

Migrating Tables
================

Table Name
----------

GaussDB(DWS) does not support the *Database name*\ **.**\ *Schema name*\ **.**\ *Table name* format. You need to convert it to the *Schema name*\ **.**\ *Table name* format.

+--------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------+
| SQL Server Syntax                                      | Syntax After Migration                                                                                                     |
+========================================================+============================================================================================================================+
| CREATE TABLE \`analytics-di-dev.abase.buyer_location\` | CREATE TABLE "abase"."buyer_location"                                                                                      |
|                                                        |                                                                                                                            |
| (                                                      | ("id_buyer" INT, "id_location" INT) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("id_buyer"); |
|                                                        |                                                                                                                            |
| id_buyer INT,                                          |                                                                                                                            |
|                                                        |                                                                                                                            |
| id_location INT                                        |                                                                                                                            |
|                                                        |                                                                                                                            |
| );                                                     |                                                                                                                            |
+--------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------+

Migration of Table-Level Parameters
-----------------------------------

SQL Server supports the creation of row-compressed tables, while GaussDB(DWS) does not. The tables are deleted during migration.

+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
| SQL Server Syntax                 | Syntax After Migration                                                                                                                 |
+===================================+========================================================================================================================================+
| CREATE TABLE dbo.T1               | CREATE TABLE "dbo"."t1" ("c1" INT, "c2" VARCHAR(200)) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("c1"); |
|                                   |                                                                                                                                        |
| (                                 |                                                                                                                                        |
|                                   |                                                                                                                                        |
| c1 INT,                           |                                                                                                                                        |
|                                   |                                                                                                                                        |
| c2 NVARCHAR(200)                  |                                                                                                                                        |
|                                   |                                                                                                                                        |
| )                                 |                                                                                                                                        |
|                                   |                                                                                                                                        |
| WITH (DATA_COMPRESSION = ROW);    |                                                                                                                                        |
+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+

SQL Server supports the creation of compressed XML tables, while GaussDB(DWS) does not. The tables are deleted during migration.

+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------+
| SQL Server Syntax                 | Syntax After Migration                                                                                                         |
+===================================+================================================================================================================================+
| CREATE TABLE dbo.T1               | CREATE TABLE "dbo"."t1" ("c1" INT, "c2" TEXT) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("c1"); |
|                                   |                                                                                                                                |
| (                                 |                                                                                                                                |
|                                   |                                                                                                                                |
| c1 INT,                           |                                                                                                                                |
|                                   |                                                                                                                                |
| c2 XML                            |                                                                                                                                |
|                                   |                                                                                                                                |
| )                                 |                                                                                                                                |
|                                   |                                                                                                                                |
| WITH (XML_COMPRESSION = ON);      |                                                                                                                                |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------+

SQL Server supports the **TEXTIMAGE_ON** parameter, which indicates that some types of data are stored in a specified file group. GaussDB(DWS) does not support this parameter and deletes it during migration.

+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------+
| SQL Server Syntax                 | Syntax After Migration                                                                                                         |
+===================================+================================================================================================================================+
| CREATE TABLE dbo.T1               | CREATE TABLE "dbo"."t1" ("c1" INT, "c2" TEXT) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("c1"); |
|                                   |                                                                                                                                |
| (                                 |                                                                                                                                |
|                                   |                                                                                                                                |
| c1 INT,                           |                                                                                                                                |
|                                   |                                                                                                                                |
| c2 text                           |                                                                                                                                |
|                                   |                                                                                                                                |
| ) TEXTIMAGE_ON "default";         |                                                                                                                                |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------+

SQL Server supports the **SYSTEM_VERSIONING** parameter, which is used to create a system versioning table. GaussDB(DWS) does not support this parameter and deletes it during migration.

+-------------------------------------------------+--------------------------------------------------------------------------------------------------+
| SQL Server Syntax                               | Syntax After Migration                                                                           |
+=================================================+==================================================================================================+
| CREATE TABLE Department                         | CREATE TABLE "department" (                                                                      |
|                                                 |                                                                                                  |
| (                                               | "departmentnumber" CHAR(10) NOT NULL PRIMARY KEY,                                                |
|                                                 |                                                                                                  |
| DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY, | "departmentname" VARCHAR(50) NOT NULL,                                                           |
|                                                 |                                                                                                  |
| DepartmentName VARCHAR(50) NOT NULL,            | "managerid" INT                                                                                  |
|                                                 |                                                                                                  |
| ManagerID INT NULL                              | ) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("departmentnumber"); |
|                                                 |                                                                                                  |
| )                                               |                                                                                                  |
|                                                 |                                                                                                  |
| WITH (SYSTEM_VERSIONING = ON);                  |                                                                                                  |
+-------------------------------------------------+--------------------------------------------------------------------------------------------------+

Migration of Column-Level Parameters
------------------------------------

SQL Server supports the creation of tables with sparse columns, but GaussDB(DWS) does not. The tables are deleted during migration.

+-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| SQL Server Syntax                 | Syntax After Migration                                                                                                                            |
+===================================+===================================================================================================================================================+
| CREATE TABLE dbo.T1               | CREATE TABLE "dbo"."t1" ("c1" INT PRIMARY KEY, "c2" VARCHAR(50)) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("c1"); |
|                                   |                                                                                                                                                   |
| (                                 |                                                                                                                                                   |
|                                   |                                                                                                                                                   |
| c1 INT PRIMARY KEY,               |                                                                                                                                                   |
|                                   |                                                                                                                                                   |
| c2 VARCHAR(50) SPARSE NULL        |                                                                                                                                                   |
|                                   |                                                                                                                                                   |
| );                                |                                                                                                                                                   |
+-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+

SQL Server supports the **FILESTREAM** keyword, which is used to specify the **FILESTREAM** data location of a table. GaussDB(DWS) does not support this keyword and is deleted during migration.

+-----------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------+
| SQL Server Syntax                                                           | Syntax After Migration                                                                        |
+=============================================================================+===============================================================================================+
| CREATE TABLE dbo.EmployeePhoto                                              | CREATE TABLE "dbo"."employeephoto" (                                                          |
|                                                                             |                                                                                               |
| (                                                                           | "employeeid" INT NOT NULL PRIMARY KEY,                                                        |
|                                                                             |                                                                                               |
| EmployeeId INT NOT NULL PRIMARY KEY,                                        | "photo" BYTEA,                                                                                |
|                                                                             |                                                                                               |
| Photo VARBINARY(MAX) FILESTREAM NULL,                                       | "myrowguidcolumn" TEXT NOT NULL                                                               |
|                                                                             |                                                                                               |
| MyRowGuidColumn UNIQUEIDENTIFIER NOT NULL ROWGUIDCOL UNIQUE DEFAULT NEWID() | ) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("employeeid");    |
|                                                                             |                                                                                               |
| );                                                                          | CREATE INDEX "idx_employeephoto_myrowguidcolumn" ON "dbo"."employeephoto"("myrowguidcolumn"); |
+-----------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------+

SQL Server supports clustered and nonclustered indexes, but GaussDB(DWS) does not. The indexes are deleted during migration.

Primary key clustered indexes

+-----------------------------------------------------------+--------------------------------------------------------------------------------------------------+
| SQL Server Syntax                                         | Syntax After Migration                                                                           |
+===========================================================+==================================================================================================+
| CREATE TABLE Department                                   | CREATE TABLE "department" (                                                                      |
|                                                           |                                                                                                  |
| (                                                         | "departmentnumber" CHAR(10) NOT NULL PRIMARY KEY,                                                |
|                                                           |                                                                                                  |
| DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY CLUSTERED, | "departmentname" VARCHAR(50) NOT NULL,                                                           |
|                                                           |                                                                                                  |
| DepartmentName VARCHAR(50) NOT NULL,                      | "managerid" INT                                                                                  |
|                                                           |                                                                                                  |
| ManagerID INT NULL                                        | ) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("departmentnumber"); |
|                                                           |                                                                                                  |
| );                                                        |                                                                                                  |
+-----------------------------------------------------------+--------------------------------------------------------------------------------------------------+

Unique index and nonclustered indexes

+---------------------------------------------------------+--------------------------------------------------------------------------------------------------+
| SQL Server Syntax                                       | Syntax After Migration                                                                           |
+=========================================================+==================================================================================================+
| CREATE TABLE Department                                 | CREATE TABLE "department" (                                                                      |
|                                                         |                                                                                                  |
| (                                                       | "departmentnumber" CHAR(10) NOT NULL UNIQUE,                                                     |
|                                                         |                                                                                                  |
| DepartmentNumber CHAR(10) NOT NULL UNIQUE NONCLUSTERED, | "departmentname" VARCHAR(50) NOT NULL,                                                           |
|                                                         |                                                                                                  |
| DepartmentName VARCHAR(50) NOT NULL,                    | "managerid" INT                                                                                  |
|                                                         |                                                                                                  |
| ManagerID INT NULL                                      | ) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("departmentnumber"); |
|                                                         |                                                                                                  |
| );                                                      |                                                                                                  |
+---------------------------------------------------------+--------------------------------------------------------------------------------------------------+
