:original_name: dws_06_0146.html

.. _dws_06_0146:

ALTER TEXT SEARCH DICTIONARY
============================

Function
--------

Modifies the definition of a full-text retrieval dictionary, including its parameters, name, owner, and schema.

Precautions
-----------

-  **ALTER** is not supported by predefined dictionaries.
-  Only the owner of a dictionary can do **ALTER** to the dictionary. System administrators have this permission by default.
-  After a dictionary is created or modified, any modification to the user-defined dictionary definition file in the directory specified by **FilePath** will not affect the dictionary in the database. To make such modifications take effect in the dictionary in the database, run the **ALTER TEXT SEARCH DICTIONARY** statement to update the definition file of the dictionary.

Syntax
------

-  Modify the dictionary definition.

   ::

      ALTER TEXT SEARCH DICTIONARY name (
          option [ = value ] [, ... ]
      );

-  Rename a dictionary.

   ::

      ALTER TEXT SEARCH DICTIONARY name RENAME TO new_name;

-  Set the schema of a dictionary.

   ::

      ALTER TEXT SEARCH DICTIONARY name SET SCHEMA new_schema;

-  Change the owner of a dictionary.

   ::

      ALTER TEXT SEARCH DICTIONARY name OWNER TO new_owner;

Parameter Description
---------------------

-  *name*

   Specifies the name of an existing dictionary. (If you do not specify a schema name, the dictionary in the current schema will be used.)

   Value range: name of an existing dictionary

-  *option*

   Specifies the name of a parameter to be modified. Each type of dictionaries has a template containing their custom parameters. Parameters function in a way irrelevant to their setting sequence. For details about parameters, see :ref:`option <en-us_topic_0000001188270514__li1286812455448>`.

   .. note::

      -  The **TEMPLATE** parameter in a dictionary cannot be modified.
      -  To specify a dictionary, specify both the dictionary definition file path (**FILEPATH**) and the file name (the parameter varies based on dictionary types).
      -  The name of a dictionary definition file can contain only lowercase letters, digits, and underscores (_).

-  *value*

   Specifies the new value of a parameter. If **=** and *value* are omitted, the previous settings of the parameter will be deleted and the default value will be used.

   Value range: valid values defined for the parameter

-  *new_name*

   Specifies the new name of a dictionary.

   Value range: a string, which complies with the identifier naming convention. A value can contain a maximum of 63 characters.

-  *new_owner*

   Specifies the new owner of a dictionary.

   Value range: an existing user name

-  *new_schema*

   Specifies the new schema of a dictionary.

   Value range: an existing schema name

Examples
--------

.. code-block::

   CREATE TEXT SEARCH DICTIONARY my_dict (
       TEMPLATE = snowball,
       Language = english,
       StopWords = english
   );

Modify the definition of stop words in **Snowball** dictionaries. Retain the values of other parameters.

.. important::

   Hard-coded or plaintext AK and SK are risky. For security purposes, encrypt your AK and SK and store them in the configuration file or environment variables.

::

   ALTER TEXT SEARCH DICTIONARY my_dict ( StopWords = newrussian, FilePath = 'obs://bucket_name/path accesskey=ak secretkey=sk region=rg' );

Modify the **Language** parameter in **Snowball** dictionaries and delete the definition of stop words.

::

   ALTER TEXT SEARCH DICTIONARY my_dict ( Language = dutch, StopWords );

Update the dictionary definition and do not change any other content.

::

   ALTER TEXT SEARCH DICTIONARY my_dict ( dummy );

Helpful Links
-------------

:ref:`CREATE TEXT SEARCH DICTIONARY <dws_06_0183>`, :ref:`DROP TEXT SEARCH DICTIONARY <dws_06_0211>`
