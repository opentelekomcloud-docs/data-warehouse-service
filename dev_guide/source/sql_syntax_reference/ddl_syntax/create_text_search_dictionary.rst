:original_name: dws_06_0183.html

.. _dws_06_0183:

CREATE TEXT SEARCH DICTIONARY
=============================

Function
--------

**CREATE TEXT SEARCH DICTIONARY** creates a full-text search dictionary. A dictionary is used to identify and process specified words during full-text search.

Dictionaries are created by using predefined templates (defined in the **PG_TS_TEMPLATE** system catalog). Five types of dictionaries can be created, **Simple**, **Ispell**, **Synonym**, **Thesaurus**, and **Snowball**. Each type of dictionaries is used to handle different tasks.

Precautions
-----------

-  A user with the **SYSADMIN** permission can create a dictionary. Then, the user automatically becomes the owner of the dictionary.
-  A dictionary cannot be created in **pg_temp** mode.
-  After a dictionary is created or modified, any modification to the user-defined dictionary definition file will not affect the dictionary in the database. To make such modifications take effect in the dictionary in the database, run the **ALTER** statement to update the definition file of the dictionary.

Syntax
------

::

   CREATE TEXT SEARCH DICTIONARY name (
       TEMPLATE = template
       [, option = value [, ... ]]
   );

Parameter Description
---------------------

-  *name*

   Specifies the name of a dictionary to be created. (If you do not specify a schema name, the dictionary will be created in the current schema.)

   Value range: a string, which complies with the identifier naming convention. A value can contain a maximum of 63 characters.

-  *template*

   Specifies a template name.

   Value range: templates (**Simple**, **Synonym**, **Thesaurus**, **Ispell**, and **Snowball**) defined in the **PG_TS_TEMPLATE** system catalog

-  .. _en-us_topic_0000001145910877__li1286812455448:

   *option*

   Specifies a parameter name. Each type of dictionaries has a template containing their custom parameters. Parameters function in a way irrelevant to their setting sequence.

   -  Parameters for a **Simple** dictionary

      -  **STOPWORDS**

         Specifies the name of a file listing stop words. The default file name extension is .stop. For example, if the value of **STOPWORDS** is **french**, the actual file name is **french.stop**. In the file, each line defines a stop word. Dictionaries will ignore blank lines and spaces in the file and convert stop-word phrases into lowercase.

      -  **ACCEPT**

         Specifies whether to accept a non-stop word as recognized. The default value is **true**.

         If **ACCEPT=true** is set for a **Simple** dictionary, no token will be passed to subsequent dictionaries. In this case, you are advised to place the **Simple** dictionary at the end of the dictionary list. If **ACCEPT=false** is set, you are advised to place the **Simple** dictionary before at least one dictionary in the list.

      -  .. _en-us_topic_0000001145910877__en-us_topic_0000001134718906_li13533193132616:

         **FILEPATH**

         Specifies the directory for storing the stop word file. The stop word file can be stored locally or on the OBS server. If the file is stored locally, the directory format is **'file://**\ *absolute_path*\ **'**. If the file is stored on the OBS server, the directory format is **'obs://bucket/path accesskey\ =\ ak secretkey\ =\ sk region\ =\ region_name'**. The directory must be enclosed in single quotation marks ('). The default value is the directory where predefined dictionary files are located. Both the **FILEPATH** and **STOPWORDS** parameters need to be specified.

         To create a dictionary using the stop word file on the OBS server, perform the following steps:

         #. Upload the stop word file to the OBS server. For example, upload the **french.stop** file to the **gaussdb** bucket on the OBS server **obsv3.sa-fb-1.externaldemo.com**. The URL is **https://gaussdb.obsv3.sa-fb-1.externaldemo.com/french.stop**. For details about how to upload the file and query the URL, see the *OBS User Manual*.

         #. Add **"**\ *region_name*\ **": "**\ *obs domain*\ **"** to the **$GAUSSHOME/etc/region_map** file. *region_name* can be a string consisting of uppercase letters, lowercase letters, digits, slashes (/), or underscores (_). *obs domain* indicates the domain name of the OBS server.

            For example, if *region_name* is set to **rg**, **region_map** is as follows: **"rg": "obsv3.sa-fb-1.externaldemo.com"**.

            .. important::

               *region_name* and *obs domain* are enclosed in double quotation marks. There is no space on the left of the colon and one space on the right of the colon.

         #. Run the **CREATE TEXT SEARCH DICTIONARY** command to create a dictionary. The command is as follows:

         .. code-block:: text

               CREATE TEXT SEARCH DICTIONARY french_dict ( TEMPLATE = pg_catalog.simple, STOPWORDS = french, FILEPATH = 'obs://gaussdb accesskey=xxx secretkey=yyy region=rg' );

         The **french.stop** file is stored in the root directory of the **gaussdb** bucket. Therefore, the *path* is empty.

   -  Parameters for a **Synonym** dictionary

      -  **SYNONYM**

         Specifies the name of the definition file for a **Synonym** dictionary. The default file name extension is .syn.

         The file is a list of synonyms. Each line is in the format of *token synonym*, that is, token and its synonym separated by a space.

      -  **CASESENSITIVE**

         Specifies whether tokens and their synonyms are case sensitive. The default value is **false**, indicating that tokens and synonyms in dictionary files will be converted into lowercase. If this parameter is set to **true**, they will not be converted into lowercase.

      -  **FILEPATH**

         Specifies the directory for storing **Synonym** dictionary files. The directory can be a local directory or an OBS directory. The default value is the directory where predefined dictionary files are located. The directory format and the process of creating a **Synonym** dictionary using a file on the OBS server are the same as those of the :ref:`FILEPATH of the Simple dictionary <en-us_topic_0000001145910877__en-us_topic_0000001134718906_li13533193132616>`.

   -  Parameters for a **Thesaurus** dictionary

      -  **DICTFILE**

         Specifies the name of a dictionary definition file. The default file name extension is .ths.

         The file is a list of synonyms. Each line is in the format of *sample words* **:** *indexed words*. The colon (:) is used as a separator between a phrase and its substitute word. If multiple sample words are matched, the TZ selects the longest one.

      -  **DICTIONARY**

         Specifies the name of a subdictionary used for word normalization. This parameter is mandatory and only one subdictionary name can be specified. The specified subdictionary must exist. It is used to identify and normalize input text before phrase matching.

         If an input word cannot be recognized by the subdictionary, an error will be reported. In this case, remove the word or update the subdictionary to make the word recognizable. In addition, an asterisk (``*``) can be placed at the beginning of an indexed word to skip the application of a subdictionary on it, but all sample words must be recognizable by the subdictionary.

         If the sample words defined in the dictionary file contain stop words defined in the subdictionary, use question marks (?) to replace them. Assume that **a** and **the** are stop words defined in the subdictionary.

         .. code-block::

            ? one ? two : swsw

         **a one the two** and **the one a two** will be matched and output as **swsw**.

      -  **FILEPATH**

         Specifies the directory for storing dictionary definition files. The directory can be a local directory or an OBS directory. The default value is the directory where predefined dictionary files are located. The directory format and the process of creating a **Synonym** dictionary using a file on the OBS server are the same as those of the :ref:`FILEPATH of the Simple dictionary <en-us_topic_0000001145910877__en-us_topic_0000001134718906_li13533193132616>`.

   -  Parameters for an **Ispell** dictionary

      -  **DICTFILE**

         Specifies the name of a dictionary definition file. The default file name extension is .dict.

      -  **AFFFILE**

         Specifies the name of an affix file. The default file name extension is .affix.

      -  **STOPWORDS**

         Specifies the name of a file listing stop words. The default file name extension is .stop. The file content format is the same as that of the file for a **Simple** dictionary.

      -  **FILEPATH**

         Specifies the directory for storing dictionary files. The directory can be a local directory or an OBS directory. The default value is the directory where predefined dictionary files are located. The directory format and the process of creating a **Synonym** dictionary using a file on the OBS server are the same as those of the :ref:`FILEPATH of the Simple dictionary <en-us_topic_0000001145910877__en-us_topic_0000001134718906_li13533193132616>`.

   -  Parameters for a **Snowball** dictionary

      -  **LANGUAGE**

         Specifies the name of a language whose stemming algorithm will be used. According to spelling rules in the language, the algorithm normalizes the variants of an input word into a basic word or a stem.

      -  **STOPWORDS**

         Specifies the name of a file listing stop words. The default file name extension is .stop. The file content format is the same as that of the file for a **Simple** dictionary.

      -  **FILEPATH**

         Specifies the directory for storing dictionary definition files. The directory can be a local directory or an OBS directory. The default value is the directory where predefined dictionary files are located. Both the **FILEPATH** and **STOPWORDS** parameters need to be specified. The directory format and the process of creating a **Snowball** dictionary using a file on the OBS server are the same as those of the **Simple** dictionary.

   .. note::

      -  The predefined dictionary file is stored in the **$GAUSSHOME/share/postgresql/tsearch_data** directory.

      -  The name of a dictionary definition file can contain only lowercase letters, numbers, and underscores (_).

-  *value*

   Specifies a parameter value. If the value is not an identifier or a number, enclose it with single quotation marks (''). You can also enclose identifiers and numbers with single quotation marks.

Examples
--------

Create an **Ispell** dictionary **english_ispell** (the dictionary definition file is from the open source dictionary).

::

   CREATE TEXT SEARCH DICTIONARY english_ispell (
       TEMPLATE = ispell,
       DictFile = english,
       AffFile = english,
       StopWords = english,
       FilePath = 'obs://bucket_name/path accesskey=ak secretkey=sk region=rg'
   );

See examples in :ref:`Configuration Examples <dws_06_0110>`.

Helpful Links
-------------

:ref:`ALTER TEXT SEARCH DICTIONARY <dws_06_0146>`, :ref:`DROP TEXT SEARCH DICTIONARY <dws_06_0211>`
