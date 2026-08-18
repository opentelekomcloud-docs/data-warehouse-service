:original_name: dws_06_0145.html

.. _dws_06_0145:

ALTER TEXT SEARCH CONFIGURATION
===============================

Function
--------

This syntax modifies the definition of a text search configuration.

Users can update the mapping from a token type to a dictionary, change the configuration's name or owner, and adjust the search configuration parameters.

Precautions
-----------

-  If a search configuration is referenced (to create an index), users are not allowed to modify it.
-  To use **ALTER TEXT SEARCH CONFIGURATION**, you must be the owner of the configuration.

Syntax
------

-  Add a mapping from a token type to a dictionary.

   ::

      ALTER TEXT SEARCH CONFIGURATION name
          ADD MAPPING FOR token_type [, ... ] WITH dictionary_name [, ... ];

-  Modify the dictionary for a specified token type in the text search configuration.

   ::

      ALTER TEXT SEARCH CONFIGURATION name
          ALTER MAPPING FOR token_type [, ... ] REPLACE old_dictionary WITH new_dictionary;

-  Modify the token type of the text search configuration.

   ::

      ALTER TEXT SEARCH CONFIGURATION name
          ALTER MAPPING FOR token_type [, ... ] WITH dictionary_name [, ... ];

-  Modify the text search configuration dictionary.

   ::

      ALTER TEXT SEARCH CONFIGURATION name
          ALTER MAPPING REPLACE old_dictionary WITH new_dictionary;

-  Delete a mapping from a token type to a dictionary.

   ::

      ALTER TEXT SEARCH CONFIGURATION name
          DROP MAPPING [ IF EXISTS ] FOR token_type [, ... ];

-  Change the owner of the text search configuration.

   ::

      ALTER TEXT SEARCH CONFIGURATION name OWNER TO new_owner;

-  Rename the text search configuration.

   ::

      ALTER TEXT SEARCH CONFIGURATION name RENAME TO new_name;

-  Rename the text search configuration schema.

   ::

      ALTER TEXT SEARCH CONFIGURATION name SET SCHEMA new_schema;

-  Modify the attributes of the text search configuration.

   ::

      ALTER TEXT SEARCH CONFIGURATION name SET ( { configuration_option = value } [, ...] );

-  Reset the text search configuration attributes.

   ::

      ALTER TEXT SEARCH CONFIGURATION name RESET ( {configuration_option} [, ...] );

.. note::

   -  **ADD MAPPING FOR** adds a mapping for the token type to the text search configuration. If the mapping for any token type after **ADD MAPPING FOR** already exists in the text search configuration, the system reports an error.
   -  **ALTER MAPPING FOR** clears the existing token type mapping and then adds the specified token type mapping.
   -  ALTER MAPPING REPLACE ... WITH ... and **ALTER MAPPING FOR**... REPLACE ... **WITH ...** options replace **old_dictionary** with **new_dictionary**. Note that only when **pg_ts_config_map** has tuples corresponding to **maptokentype** and **old_dictionary**, the update will succeed. If the update fails, no messages are returned.
   -  **DROP MAPPING FOR** deletes the token type mapping specified in the current text search configuration. **If EXISTS** is not specified and the token type mapping specified by **DROP MAPPING FOR** does not exist in the text search configuration, the database reports an error.

Parameter Description
---------------------

.. table:: **Table 1** ALTER TEXT SEARCH CONFIGURATION parameters

   +----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | Parameter            | Description                                                                                                                                                                   | Value Range                                                                                                      |
   +======================+===============================================================================================================================================================================+==================================================================================================================+
   | name                 | Specifies the name of the text search configuration to be modified, which can include a schema.                                                                               | ``-``                                                                                                            |
   +----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | token_type           | Specifies the name of the token type associated with the configured parser.                                                                                                   | For details, see :ref:`Parsers <dws_06_0101>`.                                                                   |
   +----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | dictionary_name      | Specifies the name of a text search dictionary to be consulted for the specified token types. If multiple dictionaries are listed, they are consulted in the specified order. | ``-``                                                                                                            |
   +----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | old_dictionary       | Specifies the name of the text search dictionary to be replaced in the mapping.                                                                                               | ``-``                                                                                                            |
   +----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | new_dictionary       | Specifies the name of a text search dictionary to be substituted for **old_dictionary**.                                                                                      | ``-``                                                                                                            |
   +----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | new_owner            | Specifies the new owner of the text search configuration.                                                                                                                     | Valid username.                                                                                                  |
   +----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | new_name             | Specifies the new name of the text search configuration.                                                                                                                      | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`. |
   +----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | new_schema           | Specifies the new schema name of the text search configuration.                                                                                                               | Valid schema name.                                                                                               |
   +----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | configuration_option | Specifies the text search configuration item.                                                                                                                                 | For details, see :ref:`CREATE TEXT SEARCH CONFIGURATION <dws_06_0182>`.                                          |
   +----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | value                | Specifies the value of text search configuration option.                                                                                                                      | ``-``                                                                                                            |
   +----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+

Examples
--------

Create a text search configuration named **ngram1**.

::

   DROP TEXT SEARCH CONFIGURATION IF EXISTS ngram1;
   CREATE TEXT SEARCH CONFIGURATION ngram1 (parser=ngram) WITH (gram_size = 2, grapsymbol_ignore = false);

Add a mapping to the text search type **ngram1**.

::

   ALTER TEXT SEARCH CONFIGURATION ngram1 ADD MAPPING FOR multisymbol WITH simple;

Change the owner of text search configuration.

::

   CREATE USER joe password '{Password}';
   ALTER TEXT SEARCH CONFIGURATION ngram1 OWNER TO joe;

Change the schema of text search configuration.

::

   ALTER TEXT SEARCH CONFIGURATION ngram1 SET SCHEMA joe;

Rename a text search configuration.

::

   ALTER TEXT SEARCH CONFIGURATION joe.ngram1 RENAME TO ngram_1;

Delete type mapping.

::

   ALTER TEXT SEARCH CONFIGURATION joe.ngram_1 DROP MAPPING IF EXISTS FOR multisymbol;

Create a text search configuration named **english_1**.

::

   DROP TEXT SEARCH CONFIGURATION IF EXISTS english_1;
   CREATE TEXT SEARCH CONFIGURATION english_1 (parser=default);

Add a mapping to the text search configuration.

::

   ALTER TEXT SEARCH CONFIGURATION english_1 ADD MAPPING FOR word WITH simple,english_stem;

Update the text search configuration by adding mappings.

::

   ALTER TEXT SEARCH CONFIGURATION english_1 ADD MAPPING FOR email WITH english_stem, french_stem;

Updating the text search configuration.

::

   ALTER TEXT SEARCH CONFIGURATION english_1 ALTER MAPPING REPLACE french_stem with german_stem;

Query text search configurations.

::

   SELECT b.cfgname,a.maptokentype,a.mapseqno,a.mapdict,c.dictname FROM pg_ts_config_map a,pg_ts_config b, pg_ts_dict c WHERE a.mapcfg=b.oid AND a.mapdict=c.oid AND b.cfgname='english_1' ORDER BY 1,2,3,4,5;

|image1|

Helpful Links
-------------

:ref:`CREATE TEXT SEARCH CONFIGURATION <dws_06_0182>`, :ref:`DROP TEXT SEARCH CONFIGURATION <dws_06_0210>`, :ref:`Full Text Search <dws_06_0081>`

.. |image1| image:: /_static/images/en-us_image_0000002627037910.png
