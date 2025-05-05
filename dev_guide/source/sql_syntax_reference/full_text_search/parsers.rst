:original_name: dws_06_0101.html

.. _dws_06_0101:

**Parsers**
===========

Text search parsers are responsible for splitting raw document text into tokens and identifying each token's type, where the set of types is defined by the parser itself. Note that a parser does not modify the text at all — it simply identifies plausible word boundaries. Because of this limited scope, there is less need for application-specific custom parsers than there is for custom dictionaries.

Currently, GaussDB(DWS) provides the following built-in parsers: pg_catalog.default for English configuration, and pg_catalog.ngram, pg_catalog.zhparser, and pg_catalog.pound for full text search in texts containing Chinese, or both Chinese and English.

The built-in parser is named **pg_catalog.default**. It recognizes 23 token types, shown in :ref:`Table 1 <en-us_topic_0000001764516246__tfcaeb83ea7fb42de882258f647b03890>`.

.. _en-us_topic_0000001764516246__tfcaeb83ea7fb42de882258f647b03890:

.. table:: **Table 1** Default parser's token types

   +-----------------+------------------------------------------+----------------------------------------------------------+
   | Alias           | Description                              | Examples                                                 |
   +=================+==========================================+==========================================================+
   | asciiword       | Word, all ASCII letters                  | elephant                                                 |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | word            | Word, all letters                        | mañana                                                   |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | numword         | Word, letters and digits                 | beta1                                                    |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | asciihword      | Hyphenated word, all ASCII               | up-to-date                                               |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | hword           | Hyphenated word, all letters             | lógico-matemática                                        |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | numhword        | Hyphenated word, letters and digits      | postgresql-beta1                                         |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | hword_asciipart | Hyphenated word part, all ASCII          | postgresql in the context postgresql-beta1               |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | hword_part      | Hyphenated word part, all letters        | lógico or matemática in the context lógico-matemática    |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | hword_numpart   | Hyphenated word part, letters and digits | beta1 in the context postgresql-beta1                    |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | email           | Email address                            | foo@example.com                                          |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | protocol        | Protocol head                            | http://                                                  |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | url             | URL                                      | example.com/stuff/index.html                             |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | host            | Host                                     | example.com                                              |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | url_path        | URL path                                 | /stuff/index.html, in the context of a URL               |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | file            | File or path name                        | /usr/local/foo.txt, if not within a URL                  |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | sfloat          | Scientific notation                      | -1.23E+56                                                |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | float           | Decimal notation                         | -1.234                                                   |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | int             | Signed integer                           | -1234                                                    |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | uint            | Unsigned integer                         | 1234                                                     |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | version         | Version number                           | 8.3.0                                                    |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | tag             | XML tag                                  | <a href="dictionaries.html">                             |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | entity          | XML entity                               | &amp;                                                    |
   +-----------------+------------------------------------------+----------------------------------------------------------+
   | blank           | Space symbols                            | (any whitespace or punctuation not otherwise recognized) |
   +-----------------+------------------------------------------+----------------------------------------------------------+

Note: The parser's notion of a "letter" is determined by the database's locale setting, specifically **lc_ctype**. Words containing only the basic ASCII letters are reported as a separate token type, since it is sometimes useful to distinguish them. In most European languages, token types word and asciiword should be treated alike.

**email** does not support all valid email characters as defined by RFC 5322. Specifically, the only non-alphanumeric characters supported for email user names are period, dash, and underscore.

It is possible for the parser to identify overlapping tokens in the same piece of text. As an example, a hyphenated word will be reported both as the entire word and as each component:

::

   SELECT alias, description, token FROM ts_debug('english','foo-bar-beta1');
         alias      |               description                |     token
   -----------------+------------------------------------------+---------------
    numhword        | Hyphenated word, letters and digits      | foo-bar-beta1
    hword_asciipart | Hyphenated word part, all ASCII          | foo
    blank           | Space symbols                            | -
    hword_asciipart | Hyphenated word part, all ASCII          | bar
    blank           | Space symbols                            | -
    hword_numpart   | Hyphenated word part, letters and digits | beta1

This behavior is desirable since it allows searches to work for both the whole compound word and for components. Here is another instructive example:

::

   SELECT alias, description, token FROM ts_debug('english','http://example.com/stuff/index.html');
     alias   |  description  |            token
   ----------+---------------+------------------------------
    protocol | Protocol head | http://
    url      | URL           | example.com/stuff/index.html
    host     | Host          | example.com
    url_path | URL path      | /stuff/index.html

N-gram is a mechanical word segmentation method, and applies to no semantic Chinese segmentation scenarios. The N-gram segmentation method ensures the completeness of the segmentation. However, to cover all the possibilities, it but adds unnecessary words to the index, resulting in a large number of index items. N-gram supports Chinese coding, including GBK and UTF-8. Six built-in token types are shown in :ref:`Table 2 <en-us_topic_0000001764516246__t7682dee3b51a4bdbac3572a7d5621298>`.

.. _en-us_topic_0000001764516246__t7682dee3b51a4bdbac3572a7d5621298:

.. table:: **Table 2** Token types

   =========== ===============
   Alias       Description
   =========== ===============
   zh_words    chinese words
   en_word     english word
   numeric     numeric data
   alnum       alnum string
   grapsymbol  graphic symbol
   multisymbol multiple symbol
   =========== ===============

Zhparser is a dictionary-based semantic word segmentation method. The bottom-layer calls the Simple Chinese Word Segmentation (SCWS) algorithm (https://github.com/hightman/scws), which applies to Chinese segmentation scenarios. SCWS is a term frequency and dictionary-based mechanical Chinese words engine. It can split a whole paragraph Chinese text into words. The two Chinese coding formats, GBK and UTF-8, are supported. The 26 built-in token types are shown in :ref:`Table 3 <en-us_topic_0000001764516246__t2c6fd8cdf6bd48f3abda6e5b4273303f>`.

.. _en-us_topic_0000001764516246__t2c6fd8cdf6bd48f3abda6e5b4273303f:

.. table:: **Table 3** Token types

   ===== ==========================
   Alias Description
   ===== ==========================
   A     Adjective
   B     Differentiation
   C     Conjunction
   D     Adverb
   E     Exclamation
   F     Position
   G     Lexeme
   H     Preceding element
   I     Idiom
   J     Acronyms and abbreviations
   K     Subsequent element
   L     Common words
   M     Numeral
   N     Noun
   O     Onomatopoeia
   P     Preposition
   Q     Quantifiers
   R     Pronoun
   S     Space
   T     Time
   U     Auxiliary word
   V     Verb
   W     Punctuation
   X     Unknown
   Y     Interjection
   Z     Status words
   ===== ==========================

Pound segments words in a fixed format. It is used to segment to-be-parsed nonsense Chinese and English words that are separated by fixed separators. It supports Chinese encoding (including GBK and UTF8) and English encoding (including ASCII). Pound has six pre-configured token types (as listed in :ref:`Table 4 <en-us_topic_0000001764516246__table18356541133518>`) and supports five separators (as listed in :ref:`Table 5 <en-us_topic_0000001764516246__table14245115444310>`). The default, the separator is **#**. Pound The maximum length of a token is 256 characters.

.. _en-us_topic_0000001764516246__table18356541133518:

.. table:: **Table 4** Token types

   =========== ===============
   Alias       Description
   =========== ===============
   zh_words    chinese words
   en_word     english word
   numeric     numeric data
   alnum       alnum string
   grapsymbol  graphic symbol
   multisymbol multiple symbol
   =========== ===============

.. _en-us_topic_0000001764516246__table14245115444310:

.. table:: **Table 5** Separator types

   ========= =================
   Delimiter Description
   ========= =================
   @         Special character
   #         Special character
   $         Special character
   %         Special character
   /         Special character
   ========= =================
