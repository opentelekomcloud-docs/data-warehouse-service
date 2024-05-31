:original_name: dws_06_0182.html

.. _dws_06_0182:

CREATE TEXT SEARCH CONFIGURATION
================================

Function
--------

**CREATE TEXT SEARCH CONFIGURATION** creates a text search configuration. A text search configuration specifies a text search parser that can divide a string into tokens, plus dictionaries that can be used to determine which tokens are of interest for searching.

Precautions
-----------

-  If only the parser is specified, then the new text search configuration initially has no mappings from token types to dictionaries, and therefore will ignore all words. Subsequent **ALTER TEXT SEARCH CONFIGURATION** commands must be used to create mappings to make the configuration useful. If **COPY** option is specified, the parser, mapping and configuration option of text search configuration is copied automatically.
-  If the schema name is specified, the text search configuration is created in the specified schema. Otherwise, the text search configuration is created in the current schema.
-  The user who defines a text search configuration becomes its owner.
-  **PARSER** and **COPY** options are mutually exclusive, because when an existing configuration is copied, its parser selection is copied too.

Syntax
------

::

   CREATE TEXT SEARCH CONFIGURATION name
       ( PARSER = parser_name | COPY = source_config )
       [ WITH ( {configuration_option = value} [, ...] )];

Parameter Description
---------------------

-  **name**

   Specifies the name of the text search configuration to be created. Specifies the name can be schema-qualified.

-  **parser_name**

   Specifies the name of the text search parser to use for this configuration.

-  **source_config**

   Specifies the name of an existing text search configuration to copy.

-  **configuration_option**

   Specifies the configuration parameter of text search configuration is mainly for the parser executed by **parser_name** or contained by **source_config**.

   Value range: The default, ngram, and zhparser parsers are supported. The parser of default type has no corresponding **configuration_option**. :ref:`Table 1 <en-us_topic_0000001188429080__t5cd5026f6ee04580970c9fe0e49fef47>` lists **configuration_option** for ngram and zhparser parsers.

   .. _en-us_topic_0000001188429080__t5cd5026f6ee04580970c9fe0e49fef47:

   .. table:: **Table 1** Configuration parameters for ngram and zhparser parsers

      +-----------------+----------------------------------+-------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------+
      | Parser          | Parameters for adding an account | Description                                                                                                                   | Value Range                                                                            |
      +=================+==================================+===============================================================================================================================+========================================================================================+
      | ngram           | gram_size                        | Length of word segmentation                                                                                                   | Integer, 1 to 4                                                                        |
      |                 |                                  |                                                                                                                               |                                                                                        |
      |                 |                                  |                                                                                                                               | Default value: 2                                                                       |
      +-----------------+----------------------------------+-------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------+
      |                 | punctuation_ignore               | Whether to ignore punctuations                                                                                                | -  **true** (default value): Ignore punctuations.                                      |
      |                 |                                  |                                                                                                                               | -  **false**: Do not ignore punctuations.                                              |
      +-----------------+----------------------------------+-------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------+
      |                 | grapsymbol_ignore                | Whether to ignore graphical characters                                                                                        | -  **true**: Ignore graphical characters.                                              |
      |                 |                                  |                                                                                                                               | -  **false** (default value): Do not ignore graphical characters.                      |
      +-----------------+----------------------------------+-------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------+
      | zhparser        | punctuation_ignore               | Whether to ignore special characters including punctuations (\\r and \\n will not be ignored) in the word segmentation result | -  **true** (default value): Ignore all the special characters including punctuations. |
      |                 |                                  |                                                                                                                               | -  **false**: Do not ignore all the special characters including punctuations.         |
      +-----------------+----------------------------------+-------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------+
      |                 | seg_with_duality                 | Whether to aggregate segments with duality                                                                                    | -  **true**: Aggregate segments with duality.                                          |
      |                 |                                  |                                                                                                                               | -  **false** (default value): Do not aggregate segments with duality.                  |
      +-----------------+----------------------------------+-------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------+
      |                 | multi_short                      | Whether to execute long words composite divide                                                                                | -  **true** (default value): Execute long words composite divide.                      |
      |                 |                                  |                                                                                                                               | -  **false**: Do not execute long words composite divide.                              |
      +-----------------+----------------------------------+-------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------+
      |                 | multi_duality                    | Whether to aggregate segments in long words with duality                                                                      | -  **true**: Aggregate segments in long words with duality.                            |
      |                 |                                  |                                                                                                                               | -  **false** (default value): Do not aggregate segments in long words with duality.    |
      +-----------------+----------------------------------+-------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------+
      |                 | multi_zmain                      | Whether to display key single words individually                                                                              | -  **true**: Display key single words individually.                                    |
      |                 |                                  |                                                                                                                               | -  **false** (default value): Do not display key single words individually.            |
      +-----------------+----------------------------------+-------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------+
      |                 | multi_zall                       | Whether to display all single words individually.                                                                             | -  **true**: Display all single words individually.                                    |
      |                 |                                  |                                                                                                                               | -  **false** (default value): Do not display all single words individually.            |
      +-----------------+----------------------------------+-------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------+

Examples
--------

Create a text search configuration:

::

   DROP TEXT SEARCH CONFIGURATION IF EXISTS ngram1;
   CREATE TEXT SEARCH CONFIGURATION ngram1 (parser=ngram) WITH (gram_size = 2, grapsymbol_ignore = false);

Create a text search configuration:

::

   DROP TEXT SEARCH CONFIGURATION IF EXISTS ngram2;
   CREATE TEXT SEARCH CONFIGURATION ngram2 (copy=ngram1) WITH (gram_size = 2, grapsymbol_ignore = false);

Create a text search configuration:

::

   DROP TEXT SEARCH CONFIGURATION IF EXISTS english_1;
   CREATE TEXT SEARCH CONFIGURATION english_1 (parser=default);

Helpful Links
-------------

:ref:`ALTER TEXT SEARCH CONFIGURATION <dws_06_0145>`, :ref:`DROP TEXT SEARCH CONFIGURATION <dws_06_0210>`
