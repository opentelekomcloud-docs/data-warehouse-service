:original_name: dws_04_0515.html

.. _dws_04_0515:

Converting Data Types in GaussDB(DWS) Stored Procedures
=======================================================

Certain data types in the database support implicit data type conversions, such as assignments and parameters invoked by functions. For other data types, you can use the type conversion functions provided by GaussDB(DWS), such as the CAST function, to forcibly convert them.

:ref:`Table 1 <en-us_topic_0000001764650140__t766dc94ed85840d28e897ff107682ca2>` lists common implicit data type conversions in GaussDB(DWS).

.. important::

   The valid value range of DATE supported by GaussDB(DWS) is from 4713 B.C. to 294276 A.D.

.. _en-us_topic_0000001764650140__t766dc94ed85840d28e897ff107682ca2:

.. table:: **Table 1** Implicit data type conversions

   +---------------+------------------+----------------------------------------------+
   | Raw Data Type | Target Data Type | Remarks                                      |
   +===============+==================+==============================================+
   | CHAR          | VARCHAR2         | N/A                                          |
   +---------------+------------------+----------------------------------------------+
   | CHAR          | NUMBER           | Raw data must consist of digits.             |
   +---------------+------------------+----------------------------------------------+
   | CHAR          | DATE             | Raw data cannot exceed the valid date range. |
   +---------------+------------------+----------------------------------------------+
   | CHAR          | RAW              | N/A                                          |
   +---------------+------------------+----------------------------------------------+
   | CHAR          | CLOB             | N/A                                          |
   +---------------+------------------+----------------------------------------------+
   | VARCHAR2      | CHAR             | N/A                                          |
   +---------------+------------------+----------------------------------------------+
   | VARCHAR2      | NUMBER           | Raw data must consist of digits.             |
   +---------------+------------------+----------------------------------------------+
   | VARCHAR2      | DATE             | Raw data cannot exceed the valid date range. |
   +---------------+------------------+----------------------------------------------+
   | VARCHAR2      | CLOB             | N/A                                          |
   +---------------+------------------+----------------------------------------------+
   | NUMBER        | CHAR             | N/A                                          |
   +---------------+------------------+----------------------------------------------+
   | NUMBER        | VARCHAR2         | N/A                                          |
   +---------------+------------------+----------------------------------------------+
   | DATE          | CHAR             | N/A                                          |
   +---------------+------------------+----------------------------------------------+
   | DATE          | VARCHAR2         | N/A                                          |
   +---------------+------------------+----------------------------------------------+
   | RAW           | CHAR             | N/A                                          |
   +---------------+------------------+----------------------------------------------+
   | RAW           | VARCHAR2         | N/A                                          |
   +---------------+------------------+----------------------------------------------+
   | CLOB          | CHAR             | N/A                                          |
   +---------------+------------------+----------------------------------------------+
   | CLOB          | VARCHAR2         | N/A                                          |
   +---------------+------------------+----------------------------------------------+
   | CLOB          | NUMBER           | Raw data must consist of digits.             |
   +---------------+------------------+----------------------------------------------+
   | INT4          | CHAR             | N/A                                          |
   +---------------+------------------+----------------------------------------------+
