:original_name: dws_06_0145.html

.. _dws_06_0145:

ALTER TEXT SEARCH CONFIGURATION
===============================

Function
--------

**ALTER TEXT SEARCH CONFIGURATION** modifies the definition of a text search configuration. You can modify its mappings from token types to dictionaries, change the configuration's name or owner, or modify the parameters.

Precautions
-----------

-  If a search configuration is referenced (to create an index), users are not allowed to modify it.
-  To use **ALTER TEXT SEARCH CONFIGURATION**, you must be the owner of the configuration.

Syntax
------

-  Add text search configuration string mapping.

::

   ALTER TEXT SEARCH CONFIGURATION name
       ADD MAPPING FOR token_type [, ... ] WITH dictionary_name [, ... ];

-  Modify the text search configuration dictionary syntax.

::

   ALTER TEXT SEARCH CONFIGURATION name
       ALTER MAPPING FOR token_type [, ... ] REPLACE old_dictionary WITH new_dictionary;

-  Modify the text search configuration string.

::

   ALTER TEXT SEARCH CONFIGURATION name
       ALTER MAPPING FOR token_type [, ... ] WITH dictionary_name [, ... ];

-  Change the text search configuration dictionary.

::

   ALTER TEXT SEARCH CONFIGURATION name
       ALTER MAPPING REPLACE old_dictionary WITH new_dictionary;

-  Remove text search configuration string mapping.

::

   ALTER TEXT SEARCH CONFIGURATION name
       DROP MAPPING [ IF EXISTS ] FOR token_type [, ... ];

-  Rename the owner of text search configuration.

::

   ALTER TEXT SEARCH CONFIGURATION name OWNER TO new_owner;

-  Rename the name of text search configuration.

::

   ALTER TEXT SEARCH CONFIGURATION name RENAME TO new_name;

-  Rename the namespace of text search configuration.

::

   ALTER TEXT SEARCH CONFIGURATION name SET SCHEMA new_schema;

-  Modify the attributes of text search configuration.

::

   ALTER TEXT SEARCH CONFIGURATION name SET ( { configuration_option = value } [, ...] );

-  Reset the attributes of text search configuration.

::

   ALTER TEXT SEARCH CONFIGURATION name RESET ( {configuration_option} [, ...] );

.. note::

   -  The **ADD MAPPING FOR** form installs a list of dictionaries to be consulted for the specified token types; an error will be generated if there is already a mapping for any of the token types.
   -  The **ALTER MAPPING FOR** form removes existing mapping for those token types and then adds specified mappings.
   -  ALTER MAPPING REPLACE ... WITH ... and **ALTER MAPPING FOR**... REPLACE ... **WITH ...** options replace **old_dictionary** with **new_dictionary**. Note that only when **pg_ts_config_map** has tuples corresponding to **maptokentype** and **old_dictionary**, the update will succeed. If the update fails, no messages are returned.
   -  The **DROP MAPPING FOR** form deletes all dictionaries for the specified token types in the text search configuration. If **IF EXISTS** is not specified and the string type mapping specified by **DROP MAPPING FOR** does not exist in text search configuration, an error will occur in database.

Parameter description
---------------------

-  **name**

   Specifies the name (optionally schema-qualified) of an existing text search configuration.

-  **token_type**

   Specifies the name of a token type that is emitted by the configuration's parser. For details, see :ref:`Text Search Parser <dws_06_0101>`.

-  **dictionary_name**

   Specifies the name of a text search dictionary to be consulted for the specified token types. If multiple dictionaries are listed, they are consulted in the specified order.

-  **old_dictionary**

   Specifies the name of a text search dictionary to be replaced in the mapping.

-  **new_dictionary**

   Specifies the name of a text search dictionary to be substituted for **old_dictionary**.

-  **new_owner**

   Specifies the new owner of the text search configuration.

-  **new_name**

   Specifies the new name of the text search configuration.

-  **new_schema**

   Specifies the new schema for the text search configuration.

-  **configuration_option**

   Text search configuration option. For details, see :ref:`CREATE TEXT SEARCH CONFIGURATION <dws_06_0182>`.

-  **value**

   Specifies the value of text search configuration option.

Examples
--------

Create a text search configuration:

::

   DROP TEXT SEARCH CONFIGURATION IF EXISTS ngram1;
   CREATE TEXT SEARCH CONFIGURATION ngram1 (parser=ngram) WITH (gram_size = 2, grapsymbol_ignore = false);

Add a type mapping for the text search type **ngram1**.

::

   ALTER TEXT SEARCH CONFIGURATION ngram1 ADD MAPPING FOR multisymbol WITH simple;

Change the owner of text search configuration.

::

   CREATE ROLE joe password '{Password}';
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

Create a text search configuration:

::

   DROP TEXT SEARCH CONFIGURATION IF EXISTS english_1;
   CREATE TEXT SEARCH CONFIGURATION english_1 (parser=default);

Add text search configuration string mapping.

::

   ALTER TEXT SEARCH CONFIGURATION english_1 ADD MAPPING FOR word WITH simple,english_stem;

Add text search configuration string mapping.

::

   ALTER TEXT SEARCH CONFIGURATION english_1 ADD MAPPING FOR email WITH english_stem, french_stem;

Modify text search configuration string mapping.

::

   ALTER TEXT SEARCH CONFIGURATION english_1 ALTER MAPPING REPLACE french_stem with german_stem;

Query information about the text search configuration.

::

   SELECT b.cfgname,a.maptokentype,a.mapseqno,a.mapdict,c.dictname FROM pg_ts_config_map a,pg_ts_config b, pg_ts_dict c WHERE a.mapcfg=b.oid AND a.mapdict=c.oid AND b.cfgname='english_1' ORDER BY 1,2,3,4,5;
     cfgname  | maptokentype | mapseqno | mapdict |   dictname
   -----------+--------------+----------+---------+--------------
    english_1 |            2 |        1 |    3765 | simple
    english_1 |            2 |        2 |   12960 | english_stem
    english_1 |            4 |        1 |   12960 | english_stem
    english_1 |            4 |        2 |   12966 | german_stem
   (4 rows)

Links
-----

:ref:`CREATE TEXT SEARCH CONFIGURATION <dws_06_0182>`, :ref:`DROP TEXT SEARCH CONFIGURATION <dws_06_0210>`
