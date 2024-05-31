:original_name: dws_06_0123.html

.. _dws_06_0123:

ALTER FOREIGN TABLE (GDS Import and Export)
===========================================

Function
--------

Modifies a foreign table.

Precautions
-----------

None

Syntax
------

-  Set the attributes of a foreign table.

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ]  table_name
          OPTIONS ( {[ ADD | SET | DROP ] option ['value']}[, ... ]);

-  Set a new owner.

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ] tablename
          OWNER TO new_owner;

Parameter Description
---------------------

-  **table_name**

   Specifies the name of an existing foreign table to be modified.

   Value range: an existing foreign table name.

-  **option**

   Name of the option to be modified.

   Value range: See :ref:`Parameter Description <en-us_topic_0000001188589008__s949bbfb7d67e4891ac3744b6ecf3bb2a>` in **CREATE FOREIGN TABLE**.

-  **value**

   Specifies the new value of **option**.

Examples
--------

Create a foreign table\ **customer_ft** to import data from GDS server 10.10.123.234 in TEXT format:

::

   DROP FOREIGN TABLE IF EXISTS customer_ft;
   CREATE FOREIGN TABLE customer_ft
   (
       c_customer_sk             integer               ,
       c_customer_id             char(16)              ,
       c_current_cdemo_sk        integer               ,
       c_current_hdemo_sk        integer               ,
       c_current_addr_sk         integer               ,
       c_first_shipto_date_sk    integer               ,
       c_first_sales_date_sk     integer               ,
       c_salutation              char(10)              ,
       c_first_name              char(20)              ,
       c_last_name               char(30)              ,
       c_preferred_cust_flag     char(1)               ,
       c_birth_day               integer               ,
       c_birth_month             integer               ,
       c_birth_year              integer                       ,
       c_birth_country           varchar(20)                   ,
       c_login                   char(13)                      ,
       c_email_address           char(50)                      ,
       c_last_review_date        char(10)
   )
       SERVER gsmpp_server
       OPTIONS
   (
       location 'gsfs://10.10.123.234:5000/customer1*.dat',
       FORMAT 'TEXT' ,
       DELIMITER '|',
       encoding 'utf8',
       mode 'Normal')READ ONLY;

Modify the **customer_ft** attribute of the foreign table. Delete the **mode** option.

::

   ALTER FOREIGN TABLE customer_ft options(drop mode);

Helpful Links
-------------

:ref:`CREATE FOREIGN TABLE (for GDS Import and Export) <dws_06_0159>`, :ref:`DROP FOREIGN TABLE <dws_06_0192>`
