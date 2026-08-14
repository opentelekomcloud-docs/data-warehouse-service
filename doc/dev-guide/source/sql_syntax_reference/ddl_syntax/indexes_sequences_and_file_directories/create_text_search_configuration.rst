:original_name: dws_06_0182.html

.. _dws_06_0182:

CREATE TEXT SEARCH CONFIGURATION
================================

Function
--------

**CREATE TEXT SEARCH CONFIGURATION** creates a text search configuration.

A text search configuration uses a text search parser to divide a string into tokens, and then uses dictionaries to determine which tokens are of interest for searching. This meets search requirements and improves the efficiency of full-text search.

Precautions
-----------

-  If only the parser is specified, then the new text search configuration initially has no mappings from token types to dictionaries, and therefore will ignore all words. Subsequent **ALTER TEXT SEARCH CONFIGURATION** commands must be used to create mappings to make the configuration useful. If **COPY** option is specified, the parser, mapping and configuration option of text search configuration is copied automatically.
-  If a schema name is specified, the text search configuration is created in the specified schema. Otherwise, the text search configuration is created in the current schema.
-  The user who defines a text search configuration becomes its owner.
-  **PARSER** and **COPY** options are mutually exclusive, because when an existing configuration is copied, its parser configuration is also copied.

Syntax
------

::

   CREATE TEXT SEARCH CONFIGURATION name
       ( PARSER = parser_name | COPY = source_config )
       [ WITH ( {configuration_option = value} [, ...] )];

Parameter Description
---------------------

.. table:: **Table 1** CREATE TEXT SEARCH CONFIGURATION parameters

   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Description                                                                                                                                                                                                                   | Value Range                                                                                                                                                                                                                                                                                                                                          |
   +=======================+===============================================================================================================================================================================================================================+======================================================================================================================================================================================================================================================================================================================================================+
   | name                  | Specifies the name (optionally schema-qualified) of the text search configuration to be created.                                                                                                                              | A string, which must comply with the :ref:`naming convention <en-us_topic_0000001811634529__section1475018612353>`.                                                                                                                                                                                                                                  |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | parser_name           | Specifies the name of the text search parser to use for this configuration.                                                                                                                                                   | Currently, the **default**, **ngram**, and **zhparser** parsers are supported.                                                                                                                                                                                                                                                                       |
   |                       |                                                                                                                                                                                                                               |                                                                                                                                                                                                                                                                                                                                                      |
   |                       |                                                                                                                                                                                                                               | -  **default**: default parser. For details, see :ref:`Parsers <dws_06_0101>`.                                                                                                                                                                                                                                                                       |
   |                       |                                                                                                                                                                                                                               | -  **ngram**: n-gram word segmentation algorithm, which is used to divide text into tokens by fixed-length characters or words. It is suitable for scenarios that require flexible word segmentation granularity (such as short text and fuzzy search) or multi-language or unstructured text search (such as log analysis and short text matching). |
   |                       |                                                                                                                                                                                                                               | -  **zhparser**: dictionary-based semantic word segmentation method, which is used to divide Chinese text into tokens based on semantics or language rules. The underlying layer invokes the Simple Chinese Word Segmentation (SCWS) algorithm, which is suitable for Chinese word segmentation scenarios with semantic analysis.                    |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | source_config         | Specifies the name of an existing text search configuration to copy.                                                                                                                                                          | ``-``                                                                                                                                                                                                                                                                                                                                                |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | configuration_option  | Specifies the options of the text search configuration. It is used to configure options for the specified parser (**parser_name** parser or **source_config** hidden parser), thereby optimizing the word segmentation logic. | -  **configuration_option** is not available for the **default** parser.                                                                                                                                                                                                                                                                             |
   |                       |                                                                                                                                                                                                                               | -  For details about the options of the **ngram** and **zhparser** parsers, see :ref:`Table 2 <en-us_topic_0000001811515661__t5cd5026f6ee04580970c9fe0e49fef47>`.                                                                                                                                                                                    |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001811515661__t5cd5026f6ee04580970c9fe0e49fef47:

.. table:: **Table 2** Configuration parameters for ngram and zhparser parsers

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

Create a text search configuration named **ngram1**. (Use the **ngram** parser, set the word segmentation length to two characters, and retain graphical characters in the text.)

::

   DROP TEXT SEARCH CONFIGURATION IF EXISTS  ngram1;
   CREATE TEXT SEARCH CONFIGURATION ngram1 (parser=ngram) WITH (gram_size = 2, grapsymbol_ignore = false);

Create a text search configuration named **ngram2**. (Copy all parameters of the existing configuration **ngram1**.)

::

   DROP TEXT SEARCH CONFIGURATION IF EXISTS  ngram2;
   CREATE TEXT SEARCH CONFIGURATION ngram2 (copy=ngram1);

Create a text search configuration named **english_1**. (Use the **default** text parser.)

::

   DROP TEXT SEARCH CONFIGURATION IF EXISTS english_1;
   CREATE TEXT SEARCH CONFIGURATION english_1 (parser=default);

Helpful Links
-------------

:ref:`ALTER TEXT SEARCH CONFIGURATION <dws_06_0145>`, :ref:`DROP TEXT SEARCH CONFIGURATION <dws_06_0210>`, :ref:`Full Text Search <dws_06_0081>`
