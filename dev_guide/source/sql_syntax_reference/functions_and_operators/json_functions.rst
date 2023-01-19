:original_name: dws_06_0041.html

.. _dws_06_0041:

JSON Functions
==============

JSON functions are used to generate JSON data (see :ref:`JSON Types <dws_06_0020>`).

-  array_to_json(anyarray [, pretty_bool])

   Description: Returns the array as JSON. A multi-dimensional array becomes a JSON array of arrays. Line feeds will be added between dimension-1 elements if **pretty_bool** is **true**.

   Return type: json

   For example:

   ::

      SELECT array_to_json('{{1,5},{99,100}}'::int[]);
      array_to_json
      ------------------
      [[1,5],[99,100]]
      (1 row)

-  row_to_json(record [, pretty_bool])

   Description: Returns the row as JSON. Line feeds will be added between level-1 elements if **pretty_bool** is **true**.

   Return type: json

   For example:

   ::

      SELECT row_to_json(row(1,'foo'));
           row_to_json
      ---------------------
       {"f1":1,"f2":"foo"}
      (1 row)
