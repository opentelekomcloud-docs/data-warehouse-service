:original_name: dws_06_0356.html

.. _dws_06_0356:

JSON/JSONB Functions
====================

JSON/JSONB functions are used to generate JSON data (see :ref:`JSON Types <dws_06_0020>`).

.. note::

   Except the **array_to_json** and **row_to_json** functions, other JSON/JSONB functions and operators are supported only in 8.1.2 or later.

array_to_json(anyarray [, pretty_bool])
---------------------------------------

Description: Returns the array as JSON. A multi-dimensional array becomes a JSON array of arrays. Line feeds will be added between dimension-1 elements if **pretty_bool** is **true**.

Return type: json

Example:

::

   SELECT array_to_json('{{1,5},{99,100}}'::int[]);
   array_to_json
   ------------------
   [[1,5],[99,100]]
   (1 row)

row_to_json(record [, pretty_bool])
-----------------------------------

Description: Returns the row as JSON. Line feeds will be added between level-1 elements if **pretty_bool** is **true**.

Return type: json

Example:

::

   SELECT row_to_json(row(1,'foo'));
        row_to_json
   ---------------------
    {"f1":1,"f2":"foo"}
   (1 row)

json_agg(any)
-------------

Description: Aggregates values into a JSON array.

Return type: array-json

Example:

::

   SELECT * FROM classes;
    name | score
    -----+-------
    A    |     2
    A    |     3
    D    |     5
    D    |
   (4 rows)
   SELECT name, json_agg(score) score FROM classes group by name order by name;
    name |      score
    -----+-----------------
    A    | [2, 3]
    D    | [5, null]
         | [null]
   (3 rows)

json_object_agg(any, any)
-------------------------

Description: Aggregates values into a JSON object.

Return type: json

Example:

::

   SELECT * FROM classes;
    name | score
    -----+-------
    A    |     2
    A    |     3
    D    |     5
    D    |
   (4 rows)

   SELECT json_object_agg(name, score) FROM classes group by name order by name;
        json_object_agg
   -------------------------
    { "A" : 2, "A" : 3 }
    { "D" : 5, "D" : null }
   (2 rows)

json_build_array(VARIADIC "any")
--------------------------------

Description: Constructs a JSON array that may be of heterogeneous type from a variable parameter list.

Return type: json

Example:

::

   SELECT json_build_array(1,2,'3',4,5);
    json_build_array
   -------------------
    [1, 2, "3", 4, 5]
   (1 row)

json_build_object(VARIADIC "any")
---------------------------------

Description: Constructs a JSON object from a variable parameter list. The parameter list consists of alternate keys and values. The number of input parameters must be an even number. Every two input parameters form a key-value pair. Note that the value of a key cannot be null.

Return type: json

Example:

::

   SELECT json_build_object('foo',1,'bar',2);
      json_build_object
   ------------------------
    {"foo" : 1, "bar" : 2}
   (1 row)

json_object(text[]), json_object(text[], text[])
------------------------------------------------

Description: Constructs a JSON object from a text array.

This is an overloaded function. When the input parameter is a text array, the array length must be an even number, and members are considered alternate key-value pairs. When two text arrays are used, the first array is regarded as a key, and the second array is regarded as a value. The lengths of the two arrays must be the same. Note that the value of a key cannot be null.

Return type: json

Example:

::

   SELECT json_object('{a, 1, b, "def", c, 3.5}');
                 json_object
   ---------------------------------------
    {"a" : "1", "b" : "def", "c" : "3.5"}
   (1 row)

   SELECT json_object('{{a, 1},{b, "def"},{c, 3.5}}');
                 json_object
   ---------------------------------------
    {"a" : "1", "b" : "def", "c" : "3.5"}
   (1 row)

   SELECT json_object('{a,b,"a b c"}', '{a,1,1}');
                   json_object
    ---------------------------------------
    {"a" : "a", "b" : "1", "a b c" : "1"}
   (1 row)

to_json(anyelement)
-------------------

Description: Converts parameters to json.

Return type: json

Example:

::

   SELECT to_json('Fred said "Hi."'::text);
          to_json
   ---------------------
    "Fred said \"Hi.\""
   (1 row)
   - -- Convert the column-store table json_tbl_2 to JSON:
   postgres=# SELECT * FROM json_tbl_2;
     a |  b
    ---+-----
     1 | aaa
     1 | bbb
     2 | ccc
     2 | ddd
    (4 rows)
   postgres=# SELECT to_json(t.*) FROM json_tbl_2 t;
         to_json
   -------------------
    {"a":1,"b":"bbb"}
    {"a":2,"b":"ddd"}
    {"a":1,"b":"aaa"}
    {"a":2,"b":"ccc"}
   (4 rows)

json_strip_nulls(json)
----------------------

Description: All object fields with null values are ignored, and other values remain unchanged.

Return type: json

Example:

::

   SELECT json_strip_nulls('[{"f1":1,"f2":null},2,null,3]');
     json_strip_nulls
   ---------------------
    [{"f1":1},2,null,3]
   (1 row)

json_object_field(json, text)
-----------------------------

Description: Same as the operator **->**, which returns the value of a specified key in an object.

Return type: json

Example:

::

   SELECT json_object_field('{"a": {"b":"foo"}}','a');
    json_object_field
   -------------------
    {"b":"foo"}
   (1 row)

json_object_field_text(object-json, text)
-----------------------------------------

Description: Same as the operator **->>**, which returns the value of a specified key in an object.

Return type: text

Example:

::

   SELECT json_object_field_text('{"a": {"b":"foo"}}','a');
    json_object_field_text
   ------------------------
    {"b":"foo"}
   (1 row)

json_array_element(array-json, integer)
---------------------------------------

Description: Same as the operator **->**, which returns the element with the specified subscript in the array.

Return type: json

Example:

::

   SELECT json_array_element('[1,true,[1,[2,3]],null]',2);
    json_array_element
   --------------------
    [1,[2,3]]
   (1 row)

json_array_element_text(array-json, integer)
--------------------------------------------

Description: Same as the operator **->>**, which returns the element with the specified subscript in the array.

Return type: text

Example:

::

   SELECT json_array_element_text('[1,true,[1,[2,3]],null]',2);
    json_array_element_text
   -------------------------
    [1,[2,3]]
   (1 row)

json_extract_path(json, VARIADIC text[])
----------------------------------------

Description: Same as the operator **#>**, which returns the JSON value of the path specified by *$2*.

Return type: json

Example:

::

   SELECT json_extract_path('{"f2":{"f3":1},"f4":{"f5":99,"f6":"stringy"}}', 'f4','f6');
    json_extract_path
   -------------------
    "stringy"
   (1 row)

json_extract_path_text(json, VARIADIC text[])
---------------------------------------------

Description: Same as the operator **#>>**, which returns the text value of the path specified by *$2*.

Return type: text

Example:

::

   SELECT json_extract_path_text('{"f2":{"f3":1},"f4":{"f5":99,"f6":"stringy"}}', 'f4','f6');
    json_extract_path_text
   ------------------------
    stringy
   (1 row)

json_array_elements(array-json)
-------------------------------

Description: Splits an array. Each element returns a row.

Return type: json

Example:

::

   SELECT json_array_elements('[1,true,[1,[2,3]],null]');
    json_array_elements
   ---------------------
    1
    true
    [1,[2,3]]
    null
   (4 rows)

json_array_elements_text(array-json)
------------------------------------

Description: Splits an array. Each element returns a row.

Return type: text

Example:

::

   SELECT * FROM json_array_elements_text('[1,true,[1,[2,3]],null]');
      value
   -----------
    1
    true
    [1,[2,3]]

   (4 rows)

json_array_length(array-json)
-----------------------------

Description: Returns the array length.

Return type: integer

Example:

::

   SELECT json_array_length('[1,2,3,{"f1":1,"f2":[5,6]},4,null]');
    json_array_length
   -------------------
           6
   (1 row)

json_object_keys(object-json)
-----------------------------

Description: Returns all keys at the top layer of the object.

Return type: text

Example:

::

   SELECT json_object_keys('{"f1":"abc","f2":{"f3":"a", "f4":"b"}, "f1":"abcd"}');
    json_object_keys
   ------------------
    f1
    f2
    f1
   (3 rows)

json_each(object-json)
----------------------

Description: Splits each key-value pair of an object into one row and two columns.

Return type: setof(key text, value json)

Example:

::

   SELECT * FROM json_each('{"f1":[1,2,3],"f2":{"f3":1},"f4":null}');
    key |  value
   -----+----------
    f1  | [1,2,3]
    f2  | {"f3":1}
    f4  | null
   (3 rows)

json_each_text(object-json)
---------------------------

Description: Splits each key-value pair of an object into one row and two columns.

Return type: setof(key text, value text)

Example:

::

   SELECT * FROM json_each_text('{"f1":[1,2,3],"f2":{"f3":1},"f4":null}');
    key |  value
   -----+----------
    f1  | [1,2,3]
    f2  | {"f3":1}
    f4  |
   (3 rows)

json_populate_record(anyelement, object-json [, bool])
------------------------------------------------------

Description: *$1* must be a compound parameter. Each key-value in **object-json** is split. The key is used as the column name to match the column name in *$1* and fill in the *$1* format.

Return type: anyelement

Example:

::

   CREATE TYPE jpop  AS (a text, b INT, c timestamp);
   SELECT * FROM json_populate_record(null::jpop,'{"a":"blurfl","x":43.2}');
      a    | b | c
   --------+---+---
    blurfl |   |
   (1 row)

json_populate_recordset(anyelement, array-json [, bool])
--------------------------------------------------------

Description: Performs the preceding operations on each element in the *$2* array by referring to the **json_populate_record** and **jsonb_populate_record** functions. Therefore, each element in the *$2* array must be of the **object-json** type.

Return type: setof anyelement

Example:

::

   CREATE TYPE jpop AS (a text, b INT, c timestamp);
   SELECT * FROM json_populate_recordset(null::jpop, '[{"a":1,"b":2},{"a":3,"b":4}]');
    a | b | c
   ---+---+---
    1 | 2 |
    3 | 4 |
   (2 rows)

json_to_record(object-json)
---------------------------

Description: Like all functions that return **record**, the caller must explicitly define the structure of the record using an **AS** clause. The key-value pair of **object-json** is split and reassembled. The key is used as a column name to match and fill in the structure of the record specified by the **AS** clause.

Return type: record

Example:

::

   SELECT * FROM json_to_record('{"a":1,"b":"foo","c":"bar"}'::json) as x(a int, b text, d text);
    a |  b  | d
   ---+-----+---
    1 | foo |
   (1 row)

json_to_recordset(array-json)
-----------------------------

Description: Executes the preceding function on each element in the array by referring to the **json_to_record** function. Therefore, each element in the array must be **object-json**.

Return type: SETOF record

Example:

::

   SELECT * FROM json_to_recordset('[{"a":1,"b":{"d":"foo"},"c":true},{"a":2,"c":false,"b":{"d":"bar"}}]') AS x(a INT, b json, c BOOLEAN);
    a |      b      | c
   ---+-------------+---
    1 | {"d":"foo"} | t
    2 | {"d":"bar"} | f
   (2 rows)

   SELECT * FROM json_to_recordset('[{"a":1,"b":"foo","d":false},{"a":2,"b":"bar","c":true}]') AS x(a INT, b text, c BOOLEAN);
    a |  b  | c
   ---+-----+---
    1 | foo |
    2 | bar | t
   (2 rows)

json_typeof(json)
-----------------

Description: Checks the JSON type.

Return type: text

Example:

::

   SELECT value, json_typeof(value) from (values (json '123.4'), (json '"foo"'), (json 'true'), (json 'null'), (json '[1, 2, 3]'), (json '{"x":"foo", "y":123}'), (NULL::json)) as data(value);
           value         | json_typeof
   ----------------------+-------------
    123.4                | number
    "foo"                | string
    true                 | boolean
    null                 | null
    [1, 2, 3]            | array
    {"x":"foo", "y":123} | object
                         |
   (7 rows)

jsonb_object(text[])
--------------------

Description: Constructs an **object-jsonb** from a text array. This is an overloaded function. When the input parameter is a text array, the array length must be an even number, and members are considered alternate key-value pairs.

Return type: jsonb

Example:

::

   SELECT jsonb_object('{a,1,b,2,3,NULL,"d e f","a b c"}');
                      jsonb_object
   ---------------------------------------------------
    {"3": null, "a": "1", "b": "2", "d e f": "a b c"}
   (1 row)

jsonb_object(text[], text[])
----------------------------

Description: When two text arrays are used, the first array is considered a key and the second array is considered a value. The lengths of the two arrays must be the same. Note that the value of a key cannot be null.

Return type: jsonb

Example:

::

   SELECT jsonb_object('{a,b,"a b c"}', '{a,1,1}');
               jsonb_object
   ------------------------------------
    {"a": "a", "b": "1", "a b c": "1"}
   (1 row)

to_jsonb(anyment)
-----------------

Description: Converts other types to the corresponding jsonb type.

Return type: jsonb

Example:

::

   SELECT to_jsonb(1.1);
    to_jsonb
   ----------
    1.1
   (1 row)

jsonb_agg
---------

Description: Aggregates jsonb objects into a jsonb array.

Return type: jsonb

Example:

::

   SELECT * FROM json_tbl_2;
    a |  b
   ---+-----
    1 | aaa
    1 | bbb
    2 | ccc
    2 | ddd
   (4 rows)

   SELECT a, jsonb_agg(b) FROM json_tbl_2 GROUP BY a ORDER BY a;
    a |   jsonb_agg
   ---+----------------
    1 | ["aaa", "bbb"]
    2 | ["ccc", "ddd"]
   (2 rows)

jsonb_object_agg
----------------

Description: Aggregates key-value pairs into a JSON object.

Return type: jsonb

Example:

::

   SELECT * FROM json_tbl_3;
    a |  b  | c
   ---+-----+----
    1 | aaa | 10
    1 | bbb | 20
    2 | ccc | 30
    2 | ddd | 40
   (4 rows)
   SELECT a, jsonb_object_agg(b, c) FROM json_tbl_3 GROUP BY a ORDER BY a;
    a |    jsonb_object_agg
   ---+------------------------
    1 | {"aaa": 10, "bbb": 20}
    2 | {"ccc": 30, "ddd": 40}
   (2 rows)

jsonb_build_array( [VARIADIC "any"] )
-------------------------------------

Description: Constructs a JSON array that may contain heterogeneous types from a variable parameter list.

Return type: jsonb

Example:

::

   SELECT jsonb_build_array('a',1,'b',1.2,'c',true,'d',null,'e',json '{"x": 3, "y": [1,2,3]}','');
                                  jsonb_build_array
   -------------------------------------------------------------------------------
    ["a", 1, "b", 1.2, "c", true, "d", null, "e", {"x": 3, "y": [1, 2, 3]}, null]
   (1 row)

jsonb_build_object( [VARIADIC "any"] )
--------------------------------------

Description: Constructs a JSON object from a variable parameter list. The number of input parameters must be an even number. Every two input parameters form a key-value pair. Note that the value of a key cannot be null.

Return type: jsonb

Example:

::

   SELECT jsonb_build_object(1,2);
    jsonb_build_object
   --------------------
    {"1": 2}
   (1 row)

jsonb_strip_nulls(jsonb)
------------------------

Description: All object fields with null values are omitted. Other null values remain unchanged.

Return type: jsonb

Example:

::

   SELECT jsonb_strip_nulls('[{"f1":1,"f2":null},2,null,3]');
       jsonb_strip_nulls
   -------------------------
    [{"f1": 1}, 2, null, 3]
   (1 row)

jsonb_object_field(jsonb, text)
-------------------------------

Description: Same as the operator **->**, which returns the value of a specified key in an object.

Return type: jsonb

Example:

::

   SELECT jsonb_object_field('{"a": {"b":"foo"}}','a');
    jsonb_object_field
   --------------------
    {"b": "foo"}
   (1 row)

jsonb_object_field_text(jsonb, text)
------------------------------------

Description: Same as the operator **->>**, which returns the value of a specified key in an object.

Return type: text

Example:

::

   SELECT jsonb_object_field_text('{"a": {"b":"foo"}}','a');
    jsonb_object_field_text
   -------------------------
    {"b": "foo"}
   (1 row)

jsonb_array_element(array-jsonb, integer)
-----------------------------------------

Description: Same as the operator **->**, which returns the element with the specified subscript in the array.

Return type: jsonb

Example:

::

   SELECT jsonb_array_element('[1,true,[1,[2,3]],null]',2);
    jsonb_array_element
   ---------------------
    [1, [2, 3]]
   (1 row)

jsonb_array_element_text(array-jsonb, integer)
----------------------------------------------

Description: Same as the operator **->>**, which returns the element with the specified subscript in the array.

Return type: text

Example:

::

   SELECT jsonb_array_element_text('[1,true,[1,[2,3]],null]',2);
    jsonb_array_element_text
   --------------------------
    [1, [2, 3]]
   (1 row)

jsonb_extract_path((jsonb, VARIADIC text[])
-------------------------------------------

Description: Same as the operator **#>**, which returns the value of the path specified by *$2*.

Return type: jsonb

Example:

::

   SELECT jsonb_extract_path('{"f2":{"f3":1},"f4":{"f5":99,"f6":"stringy"}}', 'f4','f6');
    jsonb_extract_path
   --------------------
    "stringy"
   (1 row)

jsonb_extract_path_text((jsonb, VARIADIC text[])
------------------------------------------------

Description: Same as the operator **#>>**, which returns the value of the path specified by *$2*.

Return type: text

Example:

::

   SELECT jsonb_extract_path_text('{"f2":{"f3":1},"f4":{"f5":99,"f6":"stringy"}}', 'f4','f6');
    jsonb_extract_path_text
   -------------------------
    stringy
   (1 row)

jsonb_array_elements(array-jsonb)
---------------------------------

Description: Splits an array. Each element returns a row.

Return type: jsonb

Example:

::

   SELECT jsonb_array_elements('[1,true,[1,[2,3]],null]');
    jsonb_array_elements
   ----------------------
    1
    true
    [1, [2, 3]]
    null
   (4 rows)

jsonb_array_elements_text(array-jsonb)
--------------------------------------

Description: Splits an array. Each element returns a row.

Return type: text

Example:

::

   SELECT * FROM jsonb_array_elements_text('[1,true,[1,[2,3]],null]');
       value
   -------------
    1
    true
    [1, [2, 3]]

   (4 rows)

jsonb_array_length(array-jsonb)
-------------------------------

Description: Returns the array length.

Return type: integer

Example:

::

   SELECT jsonb_array_length('[1,2,3,{"f1":1,"f2":[5,6]},4,null]');
    jsonb_array_length
   --------------------
             6
   (1 row)

jsonb_object_keys(object-jsonb)
-------------------------------

Description: Returns all keys at the top layer of the object.

Return type: SETOF text

Example:

::

   SELECT jsonb_object_keys('{"f1":"abc","f2":{"f3":"a", "f4":"b"}, "f1":"abcd"}');
    jsonb_object_keys
   -------------------
    f1
    f2
   (2 rows)

jsonb_each(object-jsonb)
------------------------

Description: Splits each key-value pair of an object into one row and two columns.

Return type: setof(key text, value jsonb)

Example:

::

   SELECT * FROM jsonb_each('{"f1":[1,2,3],"f2":{"f3":1},"f4":null}');
    key |   value
   -----+-----------
    f1  | [1, 2, 3]
    f2  | {"f3": 1}
    f4  | null
   (3 rows)

jsonb_each_text(object-jsonb)
-----------------------------

Description: Splits each key-value pair of an object into one row and two columns.

Return type: setof(key text, value text)

Example:

::

   SELECT * FROM jsonb_each_text('{"f1":[1,2,3],"f2":{"f3":1},"f4":null}');
    key |   value
   -----+-----------
    f1  | [1, 2, 3]
    f2  | {"f3": 1}
    f4  |
   (3 rows)

jsonb_populate_record(anyelement, object-jsonb [, bool])
--------------------------------------------------------

Description: *$1* must be a compound parameter. Each key-value in **object-json** is split. The key is used as the column name to match the column name in *$1* and fill in the *$1* format.

Return type: anyelement

Example:

::

   SELECT * FROM jsonb_populate_record(null::jpop,'{"a":"blurfl","x":43.2}');
      a    | b | c
   --------+---+---
    blurfl |   |
   (1 row)

jsonb_populate_record_set(anyelement, array-jsonb [, bool])
-----------------------------------------------------------

Description: Performs the preceding operations on each element in the *$2* array by referring to the **json_populate_record** and **jsonb_populate_record** functions. Therefore, each element in the *$2* array must be of the **object-json** type.

Return type: setof anyelement

Example:

::

   SELECT * FROM json_populate_recordset(null::jpop, '[{"a":1,"b":2},{"a":3,"b":4}]');
    a | b | c
   ---+---+---
    1 | 2 |
    3 | 4 |
   (2 rows)

jsonb_to_record(object-json)
----------------------------

Description: Like all functions that return **record**, the caller must explicitly define the structure of the record using an **AS** clause. The key-value pair of **object-json** is split and reassembled. The key is used as a column name to match and fill in the structure of the record specified by the **AS** clause.

Return type: record

Example:

::

   SELECT * FROM jsonb_to_record('{"a":1,"b":"foo","c":"bar"}'::jsonb) as x(a int, b text, d text);
    a |  b  | d
   ---+-----+---
    1 | foo |
   (1 row)

jsonb_to_recordset(array-json)
------------------------------

Description: Executes the preceding function on each element in the array by referring to the **jsonb_to_record** function. Therefore, each element in the array must be **object-jsonb**.

Return type: SETOF record

Example:

::

   SELECT * FROM jsonb_to_recordset('[{"a":1,"b":"foo","d":false},{"a":2,"b":"bar","c":true}]') AS x(a INT, b text, c boolean);
    a |  b  | c
   ---+-----+---
    1 | foo |
    2 | bar | t
   (2 rows)

jsonb_typeof(jsonb)
-------------------

Description: Checks the JSONB type.

Return type: text

Example:

::

   SELECT jsonb_typeof(to_jsonb(1.1));
    jsonb_typeof
   --------------
    number
   (1 row)

jsonb_ne(jsonb, jsonb)
----------------------

Description: Same as the operator **<>**, which compares two values.

Return type: Boolean

Example:

::

   SELECT jsonb_ne('{"a":1, "b":2}'::jsonb, '{"a":1, "b":3}'::jsonb);
    jsonb_ne
   ----------
    t
   (1 row)

jsonb_lt(jsonb, jsonb)
----------------------

Description: Same as the operator **<**, which compares two values.

Return type: Boolean

Example:

::

   SELECT jsonb_lt('{"a":1, "b":2}'::jsonb, '{"a":1, "b":3}'::jsonb);
    jsonb_lt
   ----------
    t
   (1 row)

jsonb_gt(jsonb, jsonb)
----------------------

Description: Same as the operator **>**, which compares two values.

Return type: Boolean

Example:

::

   SELECT jsonb_gt('{"a":1, "b":2}'::jsonb, '{"a":1, "b":3}'::jsonb);
    jsonb_gt
   ----------
    f
   (1 row)

jsonb_le(jsonb, jsonb)
----------------------

Description: Same as the operator **<=**, which compares two values.

Return type: Boolean

Example:

::

   SELECT jsonb_le('["a", "b"]', '{"a":1, "b":2}');
    jsonb_le
   ----------
    t
   (1 row)

jsonb_ge(jsonb, jsonb)
----------------------

Description: Same as the operator **>=**, which compares two values.

Return type: Boolean

Example:

::

   SELECT jsonb_ge('["a", "b"]', '{"a":1, "b":2}');
    jsonb_ge
   ----------
    f
   (1 row)

jsonb_eq(jsonb, jsonb)
----------------------

Description: Same as the operator **=**, which compares two values.

Return type: Boolean

Example:

::

   SELECT jsonb_eq('["a", "b"]', '{"a":1, "b":2}');
    jsonb_eq
   ----------
    f
   (1 row)

jsonb_cmp(jsonb, jsonb)
-----------------------

Description: Compares values. A positive value indicates greater than, a negative value indicates less than, and **0** indicates equal.

Return type: integer

Example:

::

   SELECT jsonb_cmp('["a", "b"]', '{"a":1, "b":2}');
    jsonb_cmp
   -----------
    -1
   (1 row)

jsonb_exists(jsonb, text)
-------------------------

Description: Same as the operator **?**, which determines whether all elements in the string array *$2* exist at the top layer of *$1* in the form of **key\\elem\\scalar**.

Return type: Boolean

Example:

::

   SELECT jsonb_exists('["1",2,3]', '1');
    jsonb_exists
   --------------
    t
   (1 row)

jsonb_exists_any(jsonb, text[])
-------------------------------

Description: Same as the operator **?\|**, which determines whether all elements in the string array *$2* exist at the top layer of *$1* in the form of **key\\elem\\scalar**.

Return type:

Example:

::

   SELECT jsonb_exists_any('["1","2",3]', '{1, 2, 4}');
    jsonb_exists_any
   ------------------
    t
   (1 row)

jsonb_exists_all(jsonb, text[])
-------------------------------

Description: Same as the operator **?&**, which determines whether all elements in the string array *$2* exist at the top layer of *$1* in the form of **key\\elem\\scalar**.

Return type:

bool

Example:

::

   SELECT jsonb_exists_all('["1","2",3]', '{1, 2}');
    jsonb_exists_all
   ------------------
    t
   (1 row)

jsonb_contained(jsonb, jsonb)
-----------------------------

Description: Same as the operator **<@**, which determines whether all elements in the string array *$1* exist at the top layer of *$2*.

Return type: Boolean

Example:

::

   SELECT jsonb_contained('[1,2,3]', '[1,2,3,4]');
    jsonb_contained
   -----------------
    t
   (1 row)

jsonb_contains(jsonb, jsonb)
----------------------------

Description: Same as the operator **@>**, which determines whether all elements in the string array *$2* exist at the top layer of *$1*.

Return type: Boolean

Example:

::

   SELECT jsonb_contains('{"a":1, "b":2, "c":3}'::jsonb, '{"a":1}');
    jsonb_contains
   -----------------
    t
   (1 row)

jsonb_concat(jsonb, jsonb)
--------------------------

Description: Combines two JSONB objects into one.

Return type: jsonb

Example:

::

   SELECT jsonb_concat('{"a":1, "b":2}'::jsonb, '{"c":3, "d":4}'::jsonb);
              jsonb_concat
   ----------------------------------
    {"a": 1, "b": 2, "c": 3, "d": 4}
   (1 row)

jsonb_delete(jsonb, text)
-------------------------

Description: Deletes the key-value pair corresponding to the key value in jsonb.

Return type: jsonb

Example:

::

   SELECT jsonb_delete('{"a":1, "b":2}'::jsonb, 'a');
    jsonb_delete
   --------------
    {"b": 2}
   (1 row)

jsonb_delete_idx(jsonb, text)
-----------------------------

Description: Deletes the element corresponding to an array subscript.

Return type: jsonb

Example:

::

   SELECT jsonb_delete_idx('[0,1,2,3,4]'::jsonb, 2);
    jsonb_delete_idx
   ------------------
    [0, 1, 3, 4]
   (1 row)

jsonb_delete_array(jsonb, VARIADIC text[])
------------------------------------------

Description: Deletes multiple elements from the jsonb array.

Return type: jsonb

Example:

::

   SELECT jsonb_delete_array('["a", "b", "c"]'::jsonb , 'a', 'b');
    jsonb_delete_array
   --------------------
    ["c"]
   (1 row)

jsonb_delete_path(jsonb, text[])
--------------------------------

Description: Deletes elements of a specified path from the jsonb array.

Return type: jsonb

Example:

::

   SELECT jsonb_delete_path('{"a":{"b":{"c":1, "d":2}}, "e":3}'::jsonb , array['a', 'b']);
    jsonb_delete_path
   -------------------
    {"a": {}, "e": 3}
   (1 row)

jsonb_set(target jsonb, path text[], new_value jsonb [, create_missing boolean])
--------------------------------------------------------------------------------

Description: Returns *target* with the section designated by *path* replaced by *new_value*, or with *new_value* added if **create_missing** is **true** (**true** by default) and the item designated by *path* does not exist. As with the path-oriented operators, negative integers that appear in *path* count from the end of JSON arrays.

Return type: jsonb

Example:

::

   SELECT jsonb_set('[{"f1":1,"f2":null},2,null,3]', '{0,f1}','[2,3,4]', false);
                     jsonb_set
   ---------------------------------------------
    [{"f1": [2, 3, 4], "f2": null}, 2, null, 3]
   (1 row)

jsonb_pretty(jsonb)
-------------------

Description: Returns in indented JSON text.

Return type: jsonb

Example:

::

   SELECT jsonb_pretty('{"a":{"b":{"c":1, "d":2}}, "e":3}'::jsonb);
       jsonb_pretty
   ---------------------
    {                  +
        "a": {         +
            "b": {     +
                "c": 1,+
                "d": 2 +
            }          +
        },             +
        "e": 3         +
    }
   (1 row)

jsonb_insert(target jsonb, path text[], new_value jsonb [, insert_after boolean])
---------------------------------------------------------------------------------

Description: Returns **target** and inserts **new_value**. If the **target** specified by path is in the JSONB array, **new_value** is inserted before the target or after **insert_after** is set to **true** (**false** by default). If the **target** is specified by path in the JSONB object, **new_value** is inserted only when the **target** does not exist. As with the path-oriented operators, negative integers that appear in *path* count from the end of JSON arrays.

Return type: jsonb

Example:

::

   SELECT jsonb_insert('{"a": [0,1,2]}', '{a, 1}', '"new_value"');
            jsonb_insert
   -------------------------------
    {"a": [0, "new_value", 1, 2]}
   (1 row)

ts_headline([ config regconfig, ] document jsonb, query tsquery [, options text ])
----------------------------------------------------------------------------------

Description: Highlights the jsonb search result.

Return type: jsonb

Example:

::

   SELECT ts_headline('english', '[{"id":9928,"user_id":4562,"user_name":"9LOHR4","create_time":"2021-06-22T16:28:16.504518+08:00"}, {"id":9959,"user_id":5524,"user_name":"YID07D","create_time":"2021-06-22T16:28:16.557228+08:00"}, {"id":9962,"user_id":7991,"user_name":"7C6QOM","create_time":"2021-06-22T16:28:16.56234+08:00"}]'::jsonb,
    to_tsquery('english', '9LOHR4'), 'StartSel = <, StopSel = >');
                                                                                                                                                            ts_headline
   ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    [{"id": 9928, "user_id": 4562, "user_name": "<9LOHR4>", "create_time": "2021-06-22T16:28:16.504518+08:00"}, {"id": 9959, "user_id": 5524, "user_name": "YID07D", "create_time": "2021-06-22T16:28:16.557228+08:00"}, {"id": 9962, "user_id": 7991, "user_name": "7C6QOM", "create_time": "2021-06-22T16:28:16.56234+08:00"}]
   (1 row)

json_to_tsvector(config regconfig, ] json, jsonb)
-------------------------------------------------

Description: Converts the json format to the tsvector file format that supports full-text search.

Return type: jsonb

Example:

::

   SELECT json_to_tsvector('{"a":1, "b":2, "c":3}'::json, to_jsonb('key'::text));
    json_to_tsvector
   ------------------
    'b':2 'c':4
   (1 row)
