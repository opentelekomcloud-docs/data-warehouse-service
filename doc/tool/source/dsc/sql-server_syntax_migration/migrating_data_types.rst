:original_name: dws_07_0189.html

.. _dws_07_0189:

Migrating Data Types
====================

Overview
--------

A data type is a basic data attribute. Occupied storage space and allowed operations vary according to data types. In a database, data is stored in tables, in which a data type is specified for each column. Data in the column must be of its allowed data type. Otherwise, errors may occur. The following table shows how SQL Server data types can be converted to GaussDB(DWS) data types.

Type Comparison
---------------

========= ===================================== ===================
Data Type SQL Server Type                       GaussDB(DWS) OUTPUT
========= ===================================== ===================
Time      datetimeoffset [ ( n ) ]              timestamptz(n)
\         datetime2 [ ( n ) ]                   timestamp(n)
\         datetime                              timestamp
\         smalldatetime                         timestamp
\         date                                  date
\         time [ ( n ) ]                        time(n)
Number    float [ ( n ) ]                       float(n)
\         real [ ( n ) ]                        float(n)
\         decimal [ ( precision [ , scale ] ) ] decimal
\         numeric [ ( precision [ , scale ] ) ] numeric
\         money                                 money
\         smallmoney                            money
\         bigint                                bigint
\         int                                   int
\         smallint                              smallint
\         tinyint                               tinyint
\         bit                                   bit
Character nvarchar [ ( n \| max ) ]             varchar
\         nchar [ ( n ) ]                       nchar(n)
\         varchar [ ( n \| max ) ]              varchar(n)
\         char [ ( n ) ]                        char(n)
Binary    varbinary [ ( n \| max ) ]            BYTEA
\         binary [ ( n ) ]                      BYTEA
Other     uniqueidentifier                      text
========= ===================================== ===================
