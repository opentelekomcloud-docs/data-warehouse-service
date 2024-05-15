:original_name: dws_04_0926.html

.. _dws_04_0926:

Zone and Formatting
===================

This section describes parameters related to the time format setting.

.. _en-us_topic_0000001188323688__se661e54f98d0474a9cc9c79c8f2294c5:

DateStyle
---------

**Parameter description**: Specifies the display format for date and time values, as well as the rules for interpreting ambiguous date input values.

This variable contains two independent components: the output format specifications (ISO, Postgres, SQL, or German) and the input/output order of year/month/day (DMY, MDY, or YMD). The two components can be set separately or together. The keywords Euro and European are synonyms for DMY; the keywords US, NonEuro, and NonEuropean are synonyms for MDY.

**Type**: USERSET

**Value range**: a string

**Default value**: **ISO, MDY**

.. note::

   **gs_initdb** will initialize this parameter so that its value is the same as that of :ref:`lc_time <en-us_topic_0000001188323688__s753451ed7f6048c3853c7328cc321818>`.

**Suggestion**: The ISO format is recommended. Postgres, SQL, and German use abbreviations for time zones, such as **EST**, **WST**, and **CST**.

IntervalStyle
-------------

**Parameter description**: Specifies the display format for interval values.

**Type**: USERSET

**Value range**: enumerated values

-  **sql_standard** indicates that output matching SQL standards will be generated.
-  **postgres** indicates that output matching PostgreSQL 8.4 will be generated when the :ref:`DateStyle <en-us_topic_0000001188323688__se661e54f98d0474a9cc9c79c8f2294c5>` parameter is set to **ISO**.
-  **postgres_verbose** indicates that output matching PostgreSQL 8.4 will be generated when the :ref:`DateStyle <en-us_topic_0000001188323688__se661e54f98d0474a9cc9c79c8f2294c5>` parameter is set to **non_ISO**.
-  **iso_8601** indicates that output matching the time interval "format with designators" defined in ISO 8601 will be generated.
-  **oracle** indicates the output result that matches the numtodsinterval function in the Oracle database. For details, see numtodsinterval.

.. important::

   The **IntervalStyle** parameter also affects the interpretation of ambiguous interval input.

**Default value**: **postgres**

TimeZone
--------

**Parameter description**: Specifies the time zone for displaying and interpreting time stamps.

**Type**: USERSET

**Value range**: a string. You can obtain it by querying the :ref:`pg_timezone_names <dws_04_0787>` view.

**Default value**: **UTC**

.. note::

   **gs_initdb** will set a time zone value that is consistent with the system environment.

timezone_abbreviations
----------------------

**Parameter description**: Specifies the time zone abbreviations that will be accepted by the server.

**Type**: USERSET

**Value range**: a string. You can obtain it by querying the pg_timezone_names view.

**Default value**: **Default**

.. note::

   **Default** indicates an abbreviation that works in most of the world. There are also other abbreviations, such as **Australia** and **India** that can be defined for a particular installation.

extra_float_digits
------------------

**Parameter description**: Specifies the number of digits displayed for floating-point values, including float4, float8, and geometric data types. The parameter value is added to the standard number of digits (FLT_DIG or DBL_DIG as appropriate).

**Type**: USERSET

**Value range**: an integer ranging from -15 to 3

.. note::

   -  This parameter can be set to **3** to include partially-significant digits. It is especially useful for dumping float data that needs to be restored exactly.
   -  This parameter can also be set to a negative value to suppress unwanted digits.

**Default value**: **0**

client_encoding
---------------

**Parameter description**: Specifies the client-side encoding type (character set).

Set this parameter as needed. Try to keep the client code and server code consistent to improve efficiency.

**Type**: USERSET

**Value range**: encoding compatible with PostgreSQL. **UTF8** indicates that the database encoding is used.

.. note::

   -  You can run the **locale -a** command to check and set the system-supported zone and the corresponding encoding format.
   -  By default, **gs_initdb** will initialize the setting of this parameter based on the current system environment. You can also run the **locale** command to check the current configuration environment.
   -  To use consistent encoding for communication within a cluster, you are advised to retain the default value of **client_encoding**. Modification to this parameter in the **postgresql.conf** file (by using the **gs_guc** tool, for example) does not take effect.

**Default value**: **UTF8**

**Recommended value**: **SQL_ASCII** or **UTF8**

lc_messages
-----------

**Parameter description**: Specifies the language in which messages are displayed.

Valid values depend on the current system. On some systems, this zone category does not exist. Setting this variable will still work, but there will be no effect. In addition, translated messages for the desired language may not exist. In this case, you can still see the English messages.

**Type**: SUSET

**Value range**: a string

.. note::

   -  You can run the **locale -a** command to check and set the system-supported zone and the corresponding encoding format.
   -  By default, **gs_initdb** will initialize the setting of this parameter based on the current system environment. You can also run the **locale** command to check the current configuration environment.

**Default value**: **C**

lc_monetary
-----------

**Parameter description**: Specifies the display format of monetary values. It affects the output of functions such as to_char. Valid values depend on the current system.

**Type**: USERSET

**Value range**: a string

.. note::

   -  You can run the **locale -a** command to check and set the system-supported zone and the corresponding encoding format.
   -  By default, **gs_initdb** will initialize the setting of this parameter based on the current system environment. You can also run the **locale** command to check the current configuration environment.

**Default value**: **C**

lc_numeric
----------

**Parameter description**: Specifies the display format of numbers. It affects the output of functions such as to_char. Valid values depend on the current system.

**Type**: USERSET

**Value range**: a string

.. note::

   -  You can run the **locale -a** command to check and set the system-supported zone and the corresponding encoding format.
   -  By default, **gs_initdb** will initialize the setting of this parameter based on the current system environment. You can also run the **locale** command to check the current configuration environment.

**Default value**: **C**

.. _en-us_topic_0000001188323688__s753451ed7f6048c3853c7328cc321818:

lc_time
-------

**Parameter description**: Specifies the display format of time and zones. It affects the output of functions such as to_char. Valid values depend on the current system.

**Type**: USERSET

**Value range**: a string

.. note::

   -  You can run the **locale -a** command to check and set the system-supported zone and the corresponding encoding format.
   -  By default, **gs_initdb** will initialize the setting of this parameter based on the current system environment. You can also run the **locale** command to check the current configuration environment.

**Default value**: **C**

default_text_search_config
--------------------------

**Parameter description**: Specifies the text search configuration.

If the specified text search configuration does not exist, an error will be reported. If the specified text search configuration is deleted, set **default_text_search_config** again. Otherwise, an error will be reported, indicating incorrect configuration.

-  The text search configuration is used by text search functions that do not have an explicit argument specifying the configuration.
-  When a configuration file matching the environment is determined, gs_initdb will initialize the configuration file with a setting that corresponds to the environment.

**Type**: USERSET

**Value range**: a string

.. note::

   GaussDB(DWS) supports the following two configurations: pg_catalog.english and pg_catalog.simple.

**Default value**: **pg_catalog.english**
