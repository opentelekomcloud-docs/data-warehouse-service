:original_name: dws_16_0117.html

.. _dws_16_0117:

.. _en-us_topic_0000001860198665:

Functions and Expressions
=========================

Overview
--------

The functions and expressions in MySQL do not exist in GaussDB(DWS) or are different from those in GaussDB(DWS). DSC migrates the functions and expressions based on the supported types of GaussDB(DWS). (compatible with ADB for MySQL)

.. _en-us_topic_0000001860198665__en-us_topic_0000001382527666_section16664135316273:

Type Mapping
------------

.. table:: **Table 1** Type mapping

   +--------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------+-----------------------------------------------------+
   | MySQL/ADB Function | Description                                                                                                                                                                                          | MySQL INPUT                                   | GaussDB(DWS) OUTPUT                                 |
   +====================+======================================================================================================================================================================================================+===============================================+=====================================================+
   | CONVERT            | Converts a value to a specified data type or character set.                                                                                                                                          | CONVERT(A, B)                                 | CAST(A AS B)                                        |
   +--------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------+-----------------------------------------------------+
   | CURDATE            | Specifies the current date. It is synonymous with CURDATE().                                                                                                                                         | CURDATE/CURDATE()                             | CURRENT_DATE ()                                     |
   +--------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------+-----------------------------------------------------+
   | GET_JSON_OBJECT    | Parses a field in a JSON string.                                                                                                                                                                     | GET_JSON_OBJECT(column, '$.obj.arg')          | JSON_EXTRACT(column, 'obj', 'arg')                  |
   |                    |                                                                                                                                                                                                      |                                               |                                                     |
   |                    |                                                                                                                                                                                                      | GET_JSON_OBJECT(column, '$[i]')               | JSON_ARRAY_ELEMENT(column, 0)                       |
   +--------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------+-----------------------------------------------------+
   | JSON_EXTRACT       | Queries the value of a field in JSON.                                                                                                                                                                | JSON_EXTRACT(column, '$.obj')                 | JSON_EXTRACT(column, 'obj')                         |
   +--------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------+-----------------------------------------------------+
   | REGEXP             | Fuzzy match                                                                                                                                                                                          | REGEXP A                                      | ~ A                                                 |
   |                    |                                                                                                                                                                                                      |                                               |                                                     |
   |                    |                                                                                                                                                                                                      | NOT REGEXP A                                  | !~ A                                                |
   +--------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------+-----------------------------------------------------+
   | UUID               | Generates a unique value (sequence number).                                                                                                                                                          | UUID                                          | SYS_GUID                                            |
   +--------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------+-----------------------------------------------------+
   | SPLIT()[]          | Splits **string** on **delimiter** and returns the **field**\ th column (counting from text of the first appeared delimiter).                                                                        | SPLIT(string text, delimiter text)[field int] | SPLIT_PART(string text, delimiter text,field int)   |
   +--------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------+-----------------------------------------------------+
   | RAND               | Obtains a random number ranging from 0.0 to 1.0.                                                                                                                                                     | RAND()                                        | RANDOM()                                            |
   +--------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------+-----------------------------------------------------+
   | SLICE              | Concatenates two or more strings, placing a separator between each one. The separator is specified by the first argument.                                                                            | SLICE()                                       | CONCAT_WS(sep text, str"any" [, str"any" [, ...] ]) |
   +--------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------+-----------------------------------------------------+
   | TRY_CAST           | Converts **x** into the type specified by **y**.                                                                                                                                                     | TRY_CAST(X AS Y)                              | CAST(X AS Y)                                        |
   +--------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------+-----------------------------------------------------+
   | CAST               | Converts **x** into the type specified by **y**.                                                                                                                                                     | CAST(X AS Y)                                  | CAST(X AS Y)                                        |
   |                    |                                                                                                                                                                                                      |                                               |                                                     |
   |                    | The second argument of the cast function in the ADB can be forcibly converted to the string or double-precision data type. In GaussDB(DWS), it is converted to the varchar or double precision type. |                                               |                                                     |
   +--------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------+-----------------------------------------------------+

**Input example: CONVERT**

::

   SELECT CONVERT (IFNULL (BUSINESS_ID, 0) , DECIMAL(18, 2)) FROM ACCOUNT;

**Output**

::

   SELECT cast (ifnull (business_id, 0) AS decimal(18, 2))FROM account;

**Input example: CURDATE**

::

   SELECT CURDATE;
   SELECT CURDATE();

**Output**

::

   SELECT CURRENT_DATE();
   SELECT CURRENT_DATE();

**Input example:** **GET_JSON_OBJECT**

::

   SELECT GET_JSON_OBJECT(COL_JSON, '$.STORE.BICYCLE.PRICE');

   SELECT GET_JSON_OBJECT(COL_JSON, '$.STORE.FRUIT[0]');

**Output**

::

   SELECT
     JSON_EXTRACT_PATH(COL_JSON, 'STORE', 'BICYCLE', 'PRICE');
   SELECT
     JSON_ARRAY_ELEMENT(JSON_EXTRACT_PATH(COL_JSON, 'STORE', 'FRUIT'), 0);

**Input example: JSON_EXTRACT**

::

   SELECT JSON_EXTRACT(EVENT_ATTR,'$.TOPIC_ID');

**Output**

::

   SELECT JSON_EXTRACT_PATH(EVENT_ATTR, 'TOPIC_ID');

**Input example: REGEXP**

::

   SELECT * FROM USERS WHERE NAME NOT REGEXP '^ Wang';
   SELECT * FROM USERS WHERE TEL REGEXP '[^4-5]{11}';

**Output**

::

   SELECT * FROM USERS WHERE NAME !~ '^ Wang';
   SELECT * FROM USERS WHERE TEL ~ '[^4-5]{11}';

**Input example: UUID**

::

   SELECT CURDATE(str1), UUID(str2, str3) FROM T1;
   SELECT A FROM B WHERE uuid() > 2;

**Output**

::

   SELECT current_date (str1),sys_guid (str2, str3) FROM T1;
   SELECT A FROM  B WHERE sys_guid () > 2;

**Input example: SPLIT()[]**

::

   SELECT split('a-b-c-d-e', '-')[4];

**Output**

::

   SELECT split_part('a-b-c-d-e', '-', 4);

**Input Example RAND**

::

   SELECT rand();

**Output**

::

   SELECT random ();

**Input Example: SLICE**

::

   SELECT slice(split('2021_08_01','_'),1,3) from dual;

**Output**

::

   SELECT
     concat_ws(
       split_part('2021_08_01', '_', 1),
       split_part('2021_08_01', '_', 2),
       split_part('2021_08_01', '_', 3)
     )
   FROM
     dual;

**Input example: TRY_CAST**

::

   select * from ods_pub where try_cast(pay_time AS timestamp) >= 1;
   select try_cast(pay_time as timestamp) from obs_pub;

**Output**

::

   SELECT * FROM ods_pub WHERE cast (pay_time AS timestamp) >= 1;
   SELECT cast (pay_time as timestamp) FROM obs_pub;

**Input Example: CAST**

::

   select cast(ifnull(c1, 0) as string) from t1;
   select cast(ifnull(c1, 0) as varchar) from t1;
   select cast(ifnull(c1, 0) as double) from t1;
   select cast(ifnull(c1, 0) as int) from t1;

**Output**

::

   SELECT cast (ifnull (c1, 0) as varchar) FROM t1;
   SELECT cast (ifnull (c1, 0) as varchar) FROM t1;
   SELECT cast (ifnull (c1, 0) as double precision) FROM t1;
   SELECT cast (ifnull (c1, 0) as int) FROM t1;
