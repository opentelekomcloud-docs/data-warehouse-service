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

      ALTER TEXT SEARCH DICTIONARY name ( option = value | option [, ...] );

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

.. table:: **Table 1** ALTER TEXT SEARCH DICTIONARY parameters

   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Description                                                                                                                                                                                                                                     | Value Range                                                                                                                        |
   +=======================+=================================================================================================================================================================================================================================================+====================================================================================================================================+
   | name                  | Specifies the name of the full-text search dictionary to be modified, which can include a schema.                                                                                                                                               | Name of a valid full-text search dictionary.                                                                                       |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | option                | Specifies the parameter name of the full-text search dictionary specific option to be modified. Each type of dictionaries has a template containing their custom parameters. Parameters function in a way irrelevant to their setting sequence. | For details about the parameters, see the **option** parameter in :ref:`Table 1 <en-us_topic_0000001811634845__table61064414712>`. |
   |                       |                                                                                                                                                                                                                                                 |                                                                                                                                    |
   |                       | -  The **TEMPLATE** parameter in a dictionary cannot be modified.                                                                                                                                                                               |                                                                                                                                    |
   |                       | -  To specify a dictionary, specify both the dictionary definition file path (**FILEPATH**) and the file name (the parameter varies based on dictionary types).                                                                                 |                                                                                                                                    |
   |                       | -  The name of a dictionary definition file can contain only lowercase letters, numbers, and underscores (_).                                                                                                                                   |                                                                                                                                    |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | value                 | Specifies the new value of a parameter. If **=** and *value* are omitted, the previous settings of the parameter will be deleted and the default value will be used.                                                                            | Corresponding **option** definition.                                                                                               |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | new_name              | Specifies the new name of the full-text search dictionary.                                                                                                                                                                                      | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`.                   |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | new_owner             | Specifies the new owner of the full-text search dictionary.                                                                                                                                                                                     | Valid username.                                                                                                                    |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
   | new_schema            | Specifies the new schema of the full-text search dictionary.                                                                                                                                                                                    | Valid schema name.                                                                                                                 |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------+

Examples
--------

Create a full-text search dictionary **my_dict**.

::

   CREATE TEXT SEARCH DICTIONARY my_dict (
       TEMPLATE = snowball,
       Language = english,
       StopWords = english
   );

Change the **Language** setting for the full-text search dictionary **my_dict** and remove the stop word definition.

::

   ALTER TEXT SEARCH DICTIONARY my_dict (Language = dutch,StopWords);

Update the definition of the full-text search dictionary **my_dict** without changing any content.

::

   ALTER TEXT SEARCH DICTIONARY my_dict (dummy);

Helpful Links
-------------

:ref:`CREATE TEXT SEARCH DICTIONARY <dws_06_0183>` and :ref:`DROP TEXT SEARCH DICTIONARY <dws_06_0211>`
