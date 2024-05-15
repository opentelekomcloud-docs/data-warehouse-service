:original_name: dws_06_0354.html

.. _dws_06_0354:

Mapping a Table to XML
======================

The functions in this section map the contents of the relational table to XML values. This is similar to exporting table in XML format.

Function parameters:

-  **tbl**: table name.
-  **nulls**: indicates whether the output contains null values. If the value is **true**, the null value in the column is **<columnname xsi:nil="true"/>**. If the value is **false**, the columns containing null values are omitted from the output.
-  **tableforest**: If this parameter is set to **true**, XML fragments are generated. If this parameter is set to **false**, XML files are generated.
-  **targetns**: specifies the XML namespace of the desired result. If this parameter is not specified, an empty string is passed.
-  **query**: SQL query statement
-  **cursor**: cursor name
-  **count**: amount of data obtained from the cursor
-  **schema**: schema name

table_to_xml(tbl regclass, nulls boolean, tableforest boolean, targetns text)
-----------------------------------------------------------------------------

Description: Maps the contents of a table to XML values.

Return type: XML

If **tableforest** is false, the XML document in the result is similar to the following:

::

   <tablename>
     <row>
       <columnname1>data</columnname1>
       <columnname2>data</columnname2>
     </row>

     <row>
       ...
     </row>

     ...
   </tablename>

If **tableforest** is true, the XML content in the result is similar to the following:

::

   <tablename>
     <columnname1>data</columnname1>
     <columnname2>data</columnname2>
   </tablename>

   <tablename>
     ...
   </tablename>

   ...

table_to_xmlschema(tbl regclass, nulls boolean, tableforest boolean, targetns text)
-----------------------------------------------------------------------------------

Description: Maps a relational table schema to an XML schema document.

Return type: XML

The result of the schema content mapping is similar to the following:

::

   <schemaname>

   table1-mapping

   table2-mapping

   ...

   </schemaname>

The format of the table mapping depends on the **tableforest** parameter.

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

The result of the database content mapping may be similar to the following:

::

   <dbname>

   <schema1name>
     ...
   </schema1name>

   <schema2name>
     ...
   </schema2name>

   ...

   </dbname>

database_to_xmlschema(nulls boolean, tableforest boolean, targetns text)
------------------------------------------------------------------------

Description: Maps a database table to an XML schema document.

Return type: XML

database_to_xml_and_xmlschema(nulls boolean, tableforest boolean, targetns text)
--------------------------------------------------------------------------------

Description: Maps database tables to XML values and schema documents.

Return type: XML
