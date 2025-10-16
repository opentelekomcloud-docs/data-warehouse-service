:original_name: dws_04_0971.html

.. _dws_04_0971:

Rules for Using Custom GaussDB(DWS) External Functions (pgSQL/Java)
===================================================================

-  [Notice] Java UDFs can perform some Java logic calculation. Do not encapsulate services in Java UDFs.
-  [Notice] Do not connect to a database in any way (for example, by using JDBC) in Java functions.
-  [Notice] Only the data types listed in the following table can be used. User-defined types and complex data types (Java Array and derived classes) are not supported.
-  [Notice] User-defined aggregation functions (UDAFs) and user-defined table-generating functions (UDTFs) are not supported.

.. table:: **Table 1** PL/Java mapping for default data types

   ============ ==================================================
   GaussDB(DWS) Java
   ============ ==================================================
   BOOLEAN      boolean
   "char"       byte
   bytea        byte[]
   SMALLINT     short
   INTEGER      int
   BIGINT       long
   FLOAT4       float
   FLOAT8       double
   CHAR         java.lang.String
   VARCHAR      java.lang.String
   TEXT         java.lang.String
   name         java.lang.String
   DATE         java.sql.Timestamp
   TIME         java.sql.Time (stored value treated as local time)
   TIMETZ       java.sql.Time
   TIMESTAMP    java.sql.Timestamp
   TIMESTAMPTZ  java.sql.Timestamp
   ============ ==================================================
