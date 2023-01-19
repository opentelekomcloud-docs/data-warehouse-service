:original_name: dws_06_0108.html

.. _dws_06_0108:

Ispell Dictionary
=================

The Ispell dictionary template supports morphological dictionaries, which can normalize many different linguistic forms of a word into the same lexeme. For example, an English Ispell dictionary can match all declensions and conjugations of the search term **bank**, such as **banking**, **banked**, **banks**, **banks'**, and **bank's**.

GaussDB(DWS) does not provide any predefined Ispell dictionaries or dictionary files. The .dict files and .affix files support multiple open-source dictionary formats, including **Ispell**, **MySpell**, and **Hunspell**.

Procedure
---------

#. Obtain the dictionary definition file (.dict) and affix file (.affix).

   You can use an open-source dictionary. The name extensions of the open-source dictionary may be .aff and .dic. In this case, you need to change them to .affix and .dict. In addition, for some dictionary files (for example, Norwegian dictionary files), you need to run the following commands to convert the character encoding to UTF-8:

   ::

      iconv -f ISO_8859-1 -t UTF-8 -o nn_no.affix nn_NO.aff
      iconv -f ISO_8859-1 -t UTF-8 -o nn_no.dict nn_NO.dic

#. Create an Ispell dictionary.

   ::

      CREATE TEXT SEARCH DICTIONARY norwegian_ispell (
          TEMPLATE = ispell,
          DictFile = nn_no,
          AffFile = nn_no,
          FilePath = 'obs://bucket_name/path accesskey=ak secretkey=sk region=rg'
      );

   The full name of the Ispell dictionary file is **nn_no.dict** and **nn_no.affix**, and the dictionary is stored in the **'obs://bucket01/obs.xxx.xxx.com accesskey=xxxxx secretkey=xxxxx region=**\ *xx-xx-xx*'. For details about the syntax and parameters for creating an Ispell dictionary, see :ref:`CREATE TEXT SEARCH DICTIONARY <dws_06_0183>`.

#. Use the Ispell dictionary to split compound words.

   ::

      SELECT ts_lexize('norwegian_ispell', 'sjokoladefabrikk');
            ts_lexize
      ---------------------
       {sjokolade,fabrikk}
      (1 row)

   **MySpell** does not support compound words. **Hunspell** supports compound words. GaussDB(DWS) supports only the basic compound word operations of **Hunspell**. Generally, an Ispell dictionary recognizes a limited set of words, so they should be followed by another broader dictionary, for example, a Snowball dictionary, which recognizes everything.
