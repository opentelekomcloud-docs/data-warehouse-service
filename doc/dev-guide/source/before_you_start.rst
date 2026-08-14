:original_name: dws_04_0001.html

.. _dws_04_0001:

Before You Start
================

Target Readers
--------------

This document is intended for database designers, application developers, and database administrators, and provides information required for designing, building, querying and maintaining data warehouses.

As a database administrator or application developer, you need to be familiar with:

-  Knowledge about OSs, which is the basis for everything.
-  SQL syntax, which is the necessary skill for database operation.

Prerequisites
-------------

Complete the following tasks before you perform operations described in this document:

-  Create a DWS cluster.
-  Use the SQL editor or SQL client to connect to the DWS database.

For details about these tasks, see "Creating a DWS Cluster" and "Connecting to a DWS Cluster" in the *Data Warehouse Service (DWS) User Guide*.

Reading Guide
-------------

If you are a new DWS user, you are advised to read the following contents first:

-  Sections describing the features, functions, and application scenarios of DWS.
-  "Getting Started": guides you through creating a data warehouse cluster, creating a database table, uploading data, and testing queries.

If you intend to or are migrating applications from other data warehouses to DWS, you might want to know how DWS differs from them.

You can find useful information from the following table for DWS database application development.

+------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Operation                                                        | Query Suggestion                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
+==================================================================+===============================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
| Quickly getting started with DWS                                 | Deploy a cluster, connect to the database, and perform some queries by referring to "Creating a Cluster" and "Connecting to a Cluster" in *Data Warehouse Service (DWS) User Guide*.                                                                                                                                                                                                                                                                                          |
|                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|                                                                  | When you are ready to construct a database, load data to tables and compile the query content to operate the data in the data warehouse. Then, you can return to the *Data Warehouse Service Database Developer Guide*.                                                                                                                                                                                                                                                       |
+------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Understand the internal architecture of a DWS data warehouse.    | To know more about DWS, go to the DWS homepage.                                                                                                                                                                                                                                                                                                                                                                                                                               |
+------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Learn how to design tables to achieve the excellent performance. | :ref:`DWS Development Design Proposal <dws_04_0074>` introduces the design specifications that should be complied with during the development of database applications. Modeling compliant with these specifications fits the distributed processing architecture of DWS and provides efficient SQL code.                                                                                                                                                                     |
|                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|                                                                  | To accelerate service execution through optimization, refer to :ref:`DWS Performance Tuning <dws_04_0400>`. Database administrators' experience and judgment play a more significant role in achieving successful performance optimization than instructions and explanations. However, the :ref:`DWS Performance Tuning <dws_04_0400>` section is expected to systematically describe the performance tuning methods for application development personnel and new DWS DBAs. |
+------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Loading data                                                     | :ref:`Importing Data <dws_04_0179>` describes how to import data to DWS.                                                                                                                                                                                                                                                                                                                                                                                                      |
+------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Managing users, groups, and database security                    | :ref:`DWS Database Security Management <dws_04_0043>` covers database security topics.                                                                                                                                                                                                                                                                                                                                                                                        |
+------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Monitoring and optimizing system performance                     | :ref:`DWS System Catalogs and Views <dws_04_0559>` describes the system catalogs where you can query the database status and monitor the query content and process.                                                                                                                                                                                                                                                                                                           |
|                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|                                                                  | To learn how to use the DWS console to check the system running status and monitoring metrics, see "Monitoring Clusters" in the *Data Warehouse Service (DWS) User Guide*.                                                                                                                                                                                                                                                                                                    |
+------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

SQL Syntax Text Conventions
---------------------------

To better understand how to use the syntax, you can refer to the following description of SQL syntax text conventions.

+----------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| Format                     | Description                                                                                                                          |
+============================+======================================================================================================================================+
| Uppercase characters       | Keywords must be in uppercase.                                                                                                       |
+----------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| Lowercase characters       | Parameters must be in lowercase.                                                                                                     |
+----------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| [ ]                        | Items in brackets [] are optional.                                                                                                   |
+----------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| ...                        | Preceding elements can appear repeatedly.                                                                                            |
+----------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| [ x \| y \| ... ]          | One item is selected from two or more options or no item is selected.                                                                |
+----------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| { x \| y \| ... }          | One item is selected from two or more options.                                                                                       |
+----------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| [x \| y \| ... ] [ ... ]   | You can choose either multiple parameters or no parameters. If you choose multiple parameters, simply separate them with spaces.     |
+----------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| [ x \| y \| ... ] [ ,... ] | You can choose either multiple parameters or no parameters. If you choose multiple parameters, simply separate them with commas (,). |
+----------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| { x \| y \| ... } [ ... ]  | You must select at least one parameter. If you select multiple parameters, separate them with spaces.                                |
+----------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| { x \| y \| ... } [ ,... ] | You must select at least one parameter. If you select multiple parameters, separate them with commas (,).                            |
+----------------------------+--------------------------------------------------------------------------------------------------------------------------------------+

Statement
---------

When writing documents, the writers of DWS try their best to provide guidance from the perspective of commercial use, application scenarios, and task completion. Even so, references to PostgreSQL content may still exist in the document. For this type of content, the following PostgreSQL Copyright is applicable:

Postgres-XC is Copyright © 1996-2013 by the PostgreSQL Global Development Group.

PostgreSQL is Copyright © 1996-2013 by the PostgreSQL Global Development Group.

Postgres95 is Copyright © 1994-5 by the Regents of the University of California.

IN NO EVENT SHALL THE UNIVERSITY OF CALIFORNIA BE LIABLE TO ANY PARTY FOR DIRECT, INDIRECT, SPECIAL, INCIDENTAL, OR CONSEQUENTIAL DAMAGES, INCLUDING LOST PROFITS, ARISING OUT OF THE USE OF THIS SOFTWARE AND ITS DOCUMENTATION, EVEN IF THE UNIVERSITY OF CALIFORNIA HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

THE UNIVERSITY OF CALIFORNIA SPECIFICALLY DISCLAIMS ANY WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE. THE SOFTWARE PROVIDED HEREUNDER IS ON AN "AS-IS" BASIS, AND THE UNIVERSITY OF CALIFORNIA HAS NO OBLIGATIONS TO PROVIDE MAINTENANCE, SUPPORT, UPDATES, ENHANCEMENTS, OR MODIFICATIONS.
