:original_name: dws_06_0293.html

.. _dws_06_0293:

DROP BLOCK RULE
===============

Function
--------

DROP BLOCK RULE deletes a query filtering rule. A query filtering rule includes the filtering rule name, bound client name, client IP address, user, and matching mode. During routine database operations, executing abnormal SQL statements may use too many resources, affecting database performance and stability. You can create query filtering rules to intercept such SQL statements.

Precautions
-----------

-  This syntax is supported only by clusters of 9.1.0.100 and later versions.

-  Only a user with the database owner permission or the **gs_role_block** role permission can run the **DROP BLOCK RULE** command. The system administrator has this permission by default.

-  Common users cannot run **CREATE BLOCK RULE**. To run it, they must be authorized by the system administrator. The authorization syntax is as follows:

   ::

      GRANT gs_role_block TO user;

   You are advised to narrow down the application scope when querying filtering rules to improve performance.

Syntax
------

::

   DROP BLOCK RULE [ IF EXISTS ] block_name;

Parameter Description
---------------------

.. table:: **Table 1** DROP BLOCK RULE parameters

   +------------+-------------------------------------------------------------------------------------------+-------------------------------------------------+
   | Parameter  | Description                                                                               | Value Range                                     |
   +============+===========================================================================================+=================================================+
   | IF EXISTS  | Sends a message instead of an error if the specified query filtering rule does not exist. | ``-``                                           |
   +------------+-------------------------------------------------------------------------------------------+-------------------------------------------------+
   | block_name | Name of the query filtering rule to be deleted.                                           | The value must be an existing filter rule name. |
   +------------+-------------------------------------------------------------------------------------------+-------------------------------------------------+

Examples
--------

Create a query filtering rule named **query_block**.

::

   DROP BLOCK RULE IF EXISTS query_block;
   CREATE BLOCK RULE query_block FILTER BY SQL('update table_table set a=1');

Delete a query filtering rule named **query_block**.

::

   DROP BLOCK RULE query_block;
   DROP BLOCK RULE IF EXISTS query_block;

Helpful Links
-------------

:ref:`CREATE BLOCK RULE <dws_06_0292>`, :ref:`ALTER BLOCK RULE <dws_06_0291>`
