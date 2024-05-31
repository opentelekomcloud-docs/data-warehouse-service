:original_name: dws_06_0020.html

.. _dws_06_0020:

JSON Types
==========

JavaScript Object Notation (JSON) data types are used for storing JSON data.

It can be an independent scalar, an array, or a key-value object. An array and an object can be called a container.

#. Scalar: a number, Boolean, string, or null
#. Array: defined in a pair of square brackets ([]), in which elements can be of any JSON data type, and are not necessarily of the same type.
#. Object: defined in a pair of braces ({}), in which objects are stored in the format of **key:value**. Each key must be a string enclosed in double quotation marks (""), and its value can be of any JSON data type. In case of duplicate keys, the last key-value pair will be used.

GaussDB(DWS) supports the json and jsonb data types to store JSON data. Where:

-  json copies all entered strings and parses them when they are used. During this process, the entered spaces, duplicate keys, and sequence are retained.
-  jsonb parses the binary data of the input. During parsing, jsonb deletes semantic-irrelevant details and duplicate keys, and sorts key values, so that the data does not to be parsed again during use.

Both JSON and JSONB are of JSON data type, and the same strings can be entered as input. The main difference between JSON and JSONB is the efficiency. Because json data is an exact copy of the input text, the data must be parsed on every execution. In contrast, jsonb data is stored in a decomposed binary form and can be processed faster, though this makes it slightly slower to input due to the conversion mechanism. In addition, because the JSONB data form is normalized, it supports more operations, for example, comparing sizes according to a specific rule. JSONB also supports indexing, which is a significant advantage.

Input Format
------------

The input must be a JSON-compliant string, which is enclosed in single quotation marks ('').

Null (null-json): Only null is supported, and all letters are in lowercase.

::

   SELECT 'null'::json;   -- suc
   SELECT 'NULL'::jsonb;  -- err

Number (num-json): The value can be a positive or negative integer, decimal fraction, or 0. The scientific notation is supported.

::

   SELECT '1'::json;
   SELECT '-1.5'::json;
   SELECT '-1.5e-5'::jsonb, '-1.5e+2'::jsonb;
   SELECT '001'::json, '+15'::json, 'NaN'::json;  -- Redundant leading zeros, plus signs (+), NaN, and infinity are not supported.

Boolean (bool-json): The value can only be **true** or **false** in lowercase.

::

   SELECT 'true'::json;
   SELECT 'false'::jsonb;

String (str-json): The value must be a string enclosed in double quotation marks ("").

::

   SELECT '"a"'::json;
   SELECT '"abc"'::jsonb;

Array (array-json): Arrays are enclosed in square brackets ([]). Elements in the array can be any valid JSON data, and are unnecessarily of the same type.

::

   SELECT '[1, 2, "foo", null]'::json;
   SELECT '[]'::json;
   SELECT '[1, 2, "foo", null, [[]], {}]'::jsonb;

Object (object-json): The value is enclosed in braces ({}). The key must be a JSON-compliant string, and the value can be any valid JSON string.

::

   SELECT '{}'::json;
   SELECT '{"a": 1, "b": {"a": 2,  "b": null}}'::json;
   SELECT '{"foo": [true, "bar"], "tags": {"a": 1, "b": null}}'::jsonb;

.. caution::

   -  Note that **'null'::json** and **null::json** are different. The difference is similar to that between the strings **str=""** and **str=null**.
   -  For numbers, when scientific notation is used, JSONB expands them, while JSON stores an exact copy of the input text.

JSONB Advanced Features
-----------------------

The main difference between JSON and JSONB is the storage mode. JSONB stores parsed binary data, which reflects the JSON hierarchy and facilitates direct access. Therefore, JSONB has more advanced features than JSON.

**Normalizes formats**

-  After the input **object-json** string is parsed into JSONB binary, semantically irrelevant details are naturally discarded, for example, spaces:

   ::

      SELECT '   [1, " a ", {"a"   :1    }]  '::jsonb;
          jsonb
      ----------------------
      [1, " a ", {"a": 1}]
      (1 row)

-  For **object-json**, duplicate key-values are deleted and only the last key-value is retained. An example is as follows:

   ::

      SELECT '{"a" : 1, "a" : 2}'::jsonb;
      jsonb
      ----------
      {"a": 2}
      (1 row)

-  For **object-json**, key-values will be re-sorted. The sorting rule is as follows: 1. Longer key-values are sorted last. 2. If the key-values are of the same length, the key-values with a larger ASCII code are sorted last. An example is as follows:

   ::

      SELECT '{"aa" : 1, "b" : 2, "a" : 3}'::jsonb;
             jsonb
      ---------------------------
      {"a": 3, "b": 2, "aa": 1}
      (1 row)

**Compares sizes**

Format normalization ensures that only one form of JSONB data exists in the same semantics. Therefore, sizes can be compared according to a specific rule.

#. First, type comparison: **object-jsonb** > **array-jsonb** > **bool-jsonb** > **num-jsonb** > **str-jsonb** > **null-jsonb**
#. Content is compared if the data type is the same:

   -  **str-json**: The default text sorting rule of the database is used for comparison. A positive value indicates greater than, a negative value indicates less than, and **0** indicates equal.
   -  **num-json**: numeric comparison
   -  **bool-json**: **true** > **false**
   -  **array-jsonb**: long elements > short elements. If the lengths are the same, compare each element in sequence.
   -  **object-jsonb**: long key-value pairs > short key-value pairs. If the lengths are the same, compare each key-value pair in sequence, first the key and then the value.

.. caution::

   For comparison within the **object-jsonb** type, the final result after format sorting is used for comparison. Therefore, the comparison result may not be so intuitive as direct input.

**Creates an index**

B-Tree and GIN indexes can be created for the JSONB type.

If the entire JSONB column uses a Btree index, the following operators can be used: =, <, <=, >, and >=.

Example: Create the table **test** and insert data into it.

::

   CREATE TABLE test(id bigserial, data JSONB, PRIMARY KEY (id));
   INSERT INTO test(data) VALUES('{"name":"Jack", "age":10, "nick_name":["Jacky","baobao"], "phone_list":["1111","2222"]}'::jsonb);

-  Create a btree index.

   ::

      CREATE INDEX idx_test_data_age ON test USING btree(((data->>'age')::int));

   Use the Btree index to query data of "age>1".

   ::

      SELECT * FROM test WHERE (data->>'age')::int>1;

-  Creating a Gin Index

   ::

      CREATE INDEX idx_test_data ON test USING gin (data);

   -- Use the GIN index to check whether there are top-level keywords.

   ::

      SELECT * FROM test WHERE data ? 'id';
      SELECT * FROM test WHERE data ?| array['id','name'];

-  Use the GIN index to check whether there are non-top-level keywords.

   ::

      CREATE INDEX idx_test_data_nick_name ON test USING gin((data->'nick_name'));
      SELECT * FROM test WHERE data->'nick_name' ? 'Jacky';

-  Use @> to check whether JSON contains nested JSON objects.

   ::

      SELECT * FROM test WHERE data @> '{"age":10, "nick_name":["Jacky"]}';

**Includes elements in a JSON**

An important capability of JSONB is to query whether a JSON contains some elements or whether some elements exist in a JSON.

-  Simple scalar/original values contain only the same value:

   ::

      SELECT '"foo"'::jsonb @> '"foo"'::jsonb;

-  The array on the left contains the string on the right.

   ::

      SELECT '[1, "aa", 3]'::jsonb ? 'aa';

-  The array on the left contains all elements of the array on the right. The sequence and repetition are not important.

   ::

      SELECT '[1, 2, 3]'::jsonb @> '[1, 3, 1]'::jsonb;

-  The **object-json** on the left contains all key-values of the **object-json** on the right.

   ::

      SELECT '{"product": "PostgreSQL", "version": 9.4, "jsonb":true}'::jsonb @> '{"version":9.4}'::jsonb;

-  The array on the left does not contain all elements in the array on the right, because the three elements in the array on the left are **1**, **2**, and **[1,3]**, and the elements on the right are **1** and **3**.

   ::

      SELECT '[1, 2, [1, 3]]'::jsonb @> '[1, 3]'::jsonb; -- false

-  The array on the right does not contain all elements in the array on the left in the following example:

   ::

      SELECT '{"foo": {"bar": "baz"}}'::jsonb @> '{"bar": "baz"}'::jsonb; -- false
