:original_name: dws_06_0023.html

.. _dws_06_0023:

Pseudo-Types
============

GaussDB(DWS) has a number of special-purpose entries that are collectively called pseudo-types. A pseudo-type cannot be used as a column data type, but it can be used to declare a function's argument or result type.

Each of the available pseudo-types is useful in situations where a function's behavior does not correspond to simply taking or returning a value of a specific SQL data type. :ref:`Table 1 <en-us_topic_0000001145511011__t6c556c1e17d64480875dad682fb109b4>` lists all pseudo-types.

.. _en-us_topic_0000001145511011__t6c556c1e17d64480875dad682fb109b4:

.. table:: **Table 1** Pseudo-Types

   +------------------+-----------------------------------------------------------------------------------------------+
   | Name             | Description                                                                                   |
   +==================+===============================================================================================+
   | any              | Indicates that a function accepts any input data type.                                        |
   +------------------+-----------------------------------------------------------------------------------------------+
   | anyelement       | Indicates that a function accepts any data type.                                              |
   +------------------+-----------------------------------------------------------------------------------------------+
   | anyarray         | Indicates that a function accepts any array data type.                                        |
   +------------------+-----------------------------------------------------------------------------------------------+
   | anynonarray      | Indicates that a function accepts any non-array data type.                                    |
   +------------------+-----------------------------------------------------------------------------------------------+
   | anyenum          | Indicates that a function accepts any enum data type.                                         |
   +------------------+-----------------------------------------------------------------------------------------------+
   | anyrange         | Indicates that a function accepts any range data type.                                        |
   +------------------+-----------------------------------------------------------------------------------------------+
   | cstring          | Indicates that a function accepts or returns a null-terminated C string.                      |
   +------------------+-----------------------------------------------------------------------------------------------+
   | internal         | Indicates that a function accepts or returns a server-internal data type.                     |
   +------------------+-----------------------------------------------------------------------------------------------+
   | language_handler | Indicates that a procedural language call handler is declared to return **language_handler**. |
   +------------------+-----------------------------------------------------------------------------------------------+
   | fdw_handler      | Indicates that a foreign-data wrapper handler is declared to return **fdw_handler**.          |
   +------------------+-----------------------------------------------------------------------------------------------+
   | record           | Identifies a function returning an unspecified row type.                                      |
   +------------------+-----------------------------------------------------------------------------------------------+
   | trigger          | Indicates that a trigger function is declared to return **trigger**.                          |
   +------------------+-----------------------------------------------------------------------------------------------+
   | void             | Indicates that a function returns no value.                                                   |
   +------------------+-----------------------------------------------------------------------------------------------+
   | opaque           | Indicates an obsolete type name that formerly served all the above purposes.                  |
   +------------------+-----------------------------------------------------------------------------------------------+

Functions coded in C (whether built in or dynamically loaded) can be declared to accept or return any of these pseudo data types. It is up to the function author to ensure that the function will behave safely when a pseudo-type is used as an argument type.

Functions coded in procedural languages can use pseudo-types only as allowed by their implementation languages. At present the procedural languages all forbid use of a pseudo-type as argument type, and allow only **void** and **record** as a result type. Some also support polymorphic functions using the **anyelement**, **anyarray**, **anynonarray**, **anyenum**, and **anyrange** types.

The **internal** pseudo-type is used to declare functions that are meant only to be called internally by the database system, and not by direct call in an SQL query. If a function has at least one **internal**-type argument, it cannot be called from SQL. You are not advised to create any function that is declared to return **internal** unless the function has at least one **internal** argument.

For example:

::

   -- Create or replace the showall() function:
   CREATE OR REPLACE FUNCTION showall() RETURNS SETOF record
   AS $$ SELECT count(*) from tpcds.store_sales where ss_customer_sk = 9692; $$
   LANGUAGE SQL;

   -- Invoke the showall() function:
   SELECT showall();
    showall
   ---------
    (35)
   (1 row)

   -- Delete the function:
   DROP FUNCTION showall();
