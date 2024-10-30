:original_name: dws_06_0354.html

.. _dws_06_0354:

Mapping a Table to XML
======================

table_to_xml(tbl regclass, nulls boolean, tableforest boolean, targetns text)
-----------------------------------------------------------------------------

Description: Maps the contents of a table to XML values.

Return type: XML

table_to_xmlschema(tbl regclass, nulls boolean, tableforest boolean, targetns text)
-----------------------------------------------------------------------------------

Description: Maps a relational table schema to an XML schema document.

Return type: XML

table_to_xml_and_xmlschema(tbl regclass, nulls boolean, tableforest boolean, targetns text)
-------------------------------------------------------------------------------------------

Description: Maps a relational table to XML values and schema documents.

Return type: XML

query_to_xml(query text, nulls boolean, tableforest boolean, targetns text)
---------------------------------------------------------------------------

Description: Maps the contents of an SQL query to XML values.

Return type: XML

query_to_xmlschema(query text, nulls boolean, tableforest boolean, targetns text)
---------------------------------------------------------------------------------

Description: Maps an SQL query into an XML schema document.

Return type: XML

query_to_xml_and_xmlschema(query text, nulls boolean, tableforest boolean, targetns text)
-----------------------------------------------------------------------------------------

Description: Maps SQL queries to XML values and schema documents.

Return type: XML

cursor_to_xml(cursor refcursor, count int, nulls boolean, tableforest boolean, targetns text)
---------------------------------------------------------------------------------------------

Description: Maps a cursor query to an XML value.

Return type: XML

cursor_to_xmlschema(cursor refcursor, nulls boolean, tableforest boolean, targetns text)
----------------------------------------------------------------------------------------

Description: Maps a cursor query to an XML schema document.

Return type: XML

schema_to_xml(schema name, nulls boolean, tableforest boolean, targetns text)
-----------------------------------------------------------------------------

Description: Maps a table in a schema to an XML value.

Return type: XML

schema_to_xmlschema(schema name, nulls boolean, tableforest boolean, targetns text)
-----------------------------------------------------------------------------------

Description: Maps a table in a schema to an XML schema document.

Return type: XML

schema_to_xml_and_xmlschema(schema name, nulls boolean, tableforest boolean, targetns text)
-------------------------------------------------------------------------------------------

Description: Maps a table in a schema to an XML value and a schema document.

Return type: XML

database_to_xml(nulls boolean, tableforest boolean, targetns text)
------------------------------------------------------------------

Description: Maps a database table to an XML value.

Return type: XML

database_to_xmlschema(nulls boolean, tableforest boolean, targetns text)
------------------------------------------------------------------------

Description: Maps a database table to an XML schema document.

Return type: XML

database_to_xml_and_xmlschema(nulls boolean, tableforest boolean, targetns text)
--------------------------------------------------------------------------------

Description: Maps database tables to XML values and schema documents.

Return type: XML

.. note::

   The parameters for mapping a table to an XML value are described as follows:

   -  **tbl**: table name.
   -  **nulls**: indicates whether the output contains null values. If the value is **true**, the null value in the column is **<columnname xsi:nil="true"/>**. If the value is **false**, the columns containing null values are omitted from the output.
   -  **tableforest**: If this parameter is set to **true**, XML fragments are generated. If this parameter is set to **false**, XML files are generated.
   -  **targetns**: specifies the XML namespace of the desired result. If this parameter is not specified, an empty string is passed.
   -  **query**: SQL query statement
   -  **cursor**: cursor name
   -  **count**: amount of data obtained from the cursor
   -  **schema**: schema name
