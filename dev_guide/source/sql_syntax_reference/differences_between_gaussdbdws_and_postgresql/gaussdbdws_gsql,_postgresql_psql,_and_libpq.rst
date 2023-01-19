:original_name: dws_06_0003.html

.. _dws_06_0003:

GaussDB(DWS) gsql, PostgreSQL psql, and libpq
=============================================

GaussDB(DWS) gsql and PostgreSQL psql
-------------------------------------

GaussDB(DWS) gsql differs from PostgreSQL psql in that the former has made the following changes to enhance security:

-  User passwords cannot be set by running the **\\password** meta-command.
-  The **\\i+**, **\\ir+**, and **\\include_relative+** meta-commands and the input and output parameter **-k** are added to encrypt imported and exported files.
-  Historical command lines cannot be printed to files using the **\\s** meta-command.
-  SQL statements related to sensitive operations, such as those containing passwords, are not recorded. Users cannot see such records when they turn pages or press up or down arrow keys to view the SQL history.
-  After a connection is set up, a message is displayed to inform users of password expiration and to show version information.

gsql provides the following additional functions based on psql:

-  The output format parameter **-r** is added to allow you to adjust the focus by pressing the **Tab** key or arrow keys when entering commands.
-  The **\\parallel** meta-command is added to improve execution performance.
-  The **\\set RETRY** meta-command is added to support retry upon statement errors.
-  Slashes (/) are used as the default terminator at the end of PL/SQL statements CREATE OR REPLACE FUNCTION/PROCEDURE.

libpq
-----

During the development of certain GaussDB(DWS) functions such as the gsql client connection tool, PostgreSQL libpq is greatly modified. However, the libpq interface is not verified in application development. You are not advised to use this set of APIs for application development, because underlying risks probably exist. You can use the ODBC or JDBC APIs instead.
