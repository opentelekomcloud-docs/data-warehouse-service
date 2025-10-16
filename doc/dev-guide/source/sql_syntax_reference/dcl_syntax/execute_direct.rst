:original_name: dws_06_0249.html

.. _dws_06_0249:

EXECUTE DIRECT
==============

Function
--------

**EXECUTE DIRECT** executes an SQL statement on a specified node. Generally, the cluster automatically allocates an SQL statement to proper nodes. **EXECUTE DIRECT** is mainly used for database maintenance and testing.

Precautions
-----------

-  Only a system administrator can run the **EXECUTE DIRECT** statement.
-  To ensure data consistency across nodes, only the **SELECT** statement can be used. Transaction statements, DDL, and DML cannot be used.
-  When the AVG aggregation calculation is performed on the specified DN using such statements, the result set is returned in array, for example, {4,2}. The result of sum is 4, and that of count is 2.
-  Do not run the **SELECT** statement on nodes where CNs reside because user table data is not stored there.
-  **EXECUTE DIRECT** cannot be nested. If the inner SQL statement to be executed is also **EXECUTE DIRECT**, run only the bottom-layer **EXECUTE DIRECT** statement.

Syntax
------

::

   EXECUTE DIRECT ON ( nodename [, ... ] ) query ;

Parameter Description
---------------------

-  **nodename**

   Specifies the node name.

   Value range: An existing node.

-  **query**

   Specifies the query SQL statement that you want to execute.

Examples
--------

Query records in table **tpcds.customer_address** on the dn_6001_6002 node:

::

   EXECUTE DIRECT ON(dn_6001_6002) 'select count(*) from tpcds.customer_address';
    count
   -------
    16922
   (1 row)
