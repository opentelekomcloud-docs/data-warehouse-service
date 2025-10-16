:original_name: dws_06_0291.html

.. _dws_06_0291:

ALTER BLOCK RULE
================

Function
--------

This function modifies the attributes of a filtering rule, including the filtering rule name, bound client name, client IP address, user, and matching mode.

This syntax is supported only by clusters of 9.1.0.100 and later versions.

Precautions
-----------

-  Only a user with the database owner permission or the **gs_role_block** role permission can run the **ALTER BLOCK RULE** statement. A system administrator has this permission by default.
-  The **DEFAULT** syntax is used to reset an attribute, but it applies only to optional options.

Syntax
------

::

   ALTER BLOCK RULE block_name
       [ [ TO user_name@'host' ] | [ TO user_name ] | [ TO 'host' ] | [ TO DEFAULT ] ] |
       [ FOR UPDATE | SELECT | INSERT | DELETE | MERGE | DEFAULT ] |
       FILTER BY
       { SQL ( 'text' ) | TEMPLATE ( template_parameter = value ) }
       [ WITH ( { with_parameter = value }, [, ... ] ) ];
   ALTER BLOCK RULE block_name RENAME to new_block_name;

Parameter Description
---------------------

-  **block_name**

   Name of the query filtering rule whose attributes need to be modified.

   Value range: a string. It must comply with the naming convention.

-  **user_name**

   Queries the users to which the filtering rule applies.

   Value range: A string. It must be a valid username.

-  **host**

   Queries the client IP address to which the filtering rule applies.

   Value range: a string of valid IP addresses.

-  **SQL**

   Queries the regular expression matching statement of the filtering rule.

   Value range: a string or a regular expression.

-  **template_parameter**

   Queries a filtering rule matching template.

   Value range: **unique_sql_id/sql_hash**. The value is a character string, where **unique_sql_id** must be all digits.

-  **with_parameter**

   Queries filtering rule options.

   Valid value:

   -  **application_name**: client name.
   -  query_band
   -  **table_num**: number of tables scanned by the statement.
   -  **partition_num**: maximum number of partitions to be scanned by the operator.
   -  **estimate_row**: estimated maximum number of table rows scanned by the operator.
   -  **resource_pool**: name of the resource pool to be switched to.
   -  **max_active_num**: maximum number of concurrent statements corresponding to the rule.
   -  **is_warning**: whether an alarm should be generated or an error should be reported when a statement is intercepted.

Examples
--------

Create a query rule **query_block**.

::

   CREATE BLOCK RULE query_block FILTER BY SQL('update table_table set a=1');

Modify the attributes of the query rule **query_block**.

::

   ALTER BLOCK RULE query_block TO user1@'192.168.x.x' FOR SELECT FILTER BY SQL('select * from table_name')WITH(application_name='gsql',query_band='test1',table_num='2',partition_num='3',estimate_row='1000',resource_pool='rsp1',max_active_num='3',is_warning='off');

Rename the query rule **query_block**.

::

   ALTER BLOCK RULE query_block RENAME TO query_block_new;
