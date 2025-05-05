:original_name: dws_03_2100.html

.. _dws_03_2100:

How Do I Check Whether a GaussDB(DWS) Table Is Row-Stored or Column-Stored?
===========================================================================

The storage mode of a table is controlled by the ORIENTATION parameter in the table creation statement. **row** indicates row storage, and **column** indicates column storage.

You can use the table definition function **PG_GET_TABLEDEF** to check whether the created table is row-store or column-store.

For example, **orientation=column** indicates a column-store table.

Currently, you cannot run the **ALTER TABLE** statement to modify the parameter **ORIENTATION**.

::

   SELECT * FROM PG_GET_TABLEDEF('customer_t1');
                                     pg_get_tabledef
   -----------------------------------------------------------------------------------
    SET search_path = tpchobs;                                                       +
    CREATE  TABLE customer_t1 (                                                      +
            c_customer_sk integer,                                                   +
            c_customer_id character(5),                                              +
            c_first_name character(6),                                               +
            c_last_name character(8)                                                 +
    )                                                                                +
    WITH (orientation=column, compression=middle, colversion=2.0, enable_delta=false)+
    DISTRIBUTE BY HASH(c_last_name)                                                  +
    TO GROUP group_version1;
   (1 row)
