:original_name: dws_06_0067.html

.. _dws_06_0067:

XML Functions
=============

Generating XML Content
----------------------

-  XMLPARSE ( { DOCUMENT \| CONTENT } *value*)

Description: Generates an XML value from character data.

Return type: XML

Example:

::

   SELECT xmlparse(document '<foo>bar</foo>');
   xmlparse
   ----------------
   <foo>bar</foo>
   (1 row)

-  XMLSERIALIZE ( { DOCUMENT \| CONTENT } value AS type

Description: Generates a string from XML values.

Return type: type, which can be character, character varying, or text (or its alias)

Example:

::

   SELECT xmlserialize(content 'good' AS CHAR(10));
   xmlserialize
   --------------
   good
   (1 row)

-  xmlcomment(text)

Description: Creates an XML note that uses the specified text as the content. The text cannot contain two consecutive hyphens (--) or end with a hyphen (-). If the parameter is null, the result is also null.

Return type: XML

Example:

::

   SELECT xmlcomment('hello');
   xmlcomment
   --------------
   <!--hello-->
   (1 row)

-  xmlconcat(xml[, ...])

Description: Concatenates a list of XML values into a single value. Null values are ignored. If all parameters are null, the result is also null.

Return type: XML

Example:

::

   SELECT xmlconcat('<abc/>', '<bar>foo</bar>');
   xmlconcat
   ----------------------
   <abc/><bar>foo</bar>
   (1 row)

Note: If XML declarations exist and they are the same XML version, the result will use the version. Otherwise, the result does not use any version. If all XML values have the **standalone** attribute whose status is **yes**, the **standalone** attribute in the result is **yes**. If at least one XML value's **standalone** attribute is **no**, the **standalone** attribute in the result is **no**. Otherwise, the result does not contain the **standalone** attribute.

Example:

::

   SELECT xmlconcat('<?xml version="1.1"?><foo/>', '<?xml version="1.1" standalone="no"?><bar/>');
   xmlconcat
   -----------------------------------
   <?xml version="1.1"?><foo/><bar/>
   (1 row)

-  xmlelement(name name [, xmlattributes(value [AS attname] [, ... ])] [, content, ...])

Description: Generates an XML element with the given name, attribute, and content.

Return type: XML

Example:

::

   SELECT xmlelement(name foo, xmlattributes(current_date as bar), 'cont', 'ent');
   xmlelement
   -------------------------------------
   <foo bar="2020-08-15">content</foo>
   (1 row)

-  xmlforest(content [AS name] [, ...])

Description: Generates an XML forest (sequence) of an element with a given name and content.

Return type: XML

Example:

::

   SELECT xmlforest('abc' AS foo, 123 AS bar);
   xmlforest
   ------------------------------
   <foo>abc</foo><bar>123</bar>
   (1 row)

-  xmlpi(name target [, content])

Description: Creates an XML processing instruction. The content cannot contain the character sequence of **?>**.

Return type: XML

Example:

::

   SELECT xmlpi(name php, 'echo "hello world";');
   xmlpi
   -----------------------------
   <?php echo "hello world";?>
   (1 row)

-  xmlroot(xml, version text \| no value [, standalone yes|no|no value])

Description: Modifies the attributes of the root node of an XML value. If a version is specified, it replaces the value in the version declaration of the root node. If a **standalone** value is specified, it replaces the **standalone** value in the root node.

Return type: XML

Example:

::

   SELECT xmlroot(xmlparse(document '<?xml version="1.0" standalone="no"?><content>abc</content>'), version '1.1', standalone yes);
   xmlroot
   --------------------------------------------------------------
   <?xml version="1.1" standalone="yes"?><content>abc</content>
   (1 row)

-  xmlagg(xml)

Description: The **xmlagg** function is an aggregate function that concatenates input values.

Return type: XML

Example:

::

   CREATE TABLE test (y int, x xml);
   INSERT INTO test VALUES (1, '<foo>abc</foo>');
   INSERT INTO test VALUES (2, '<bar/>');
   SELECT xmlagg(x) FROM test;
   xmlagg
   ----------------------
   <foo>abc</foo><bar/>
   (1 row)

To determine the concatenation sequence, you can add an ORDER BY clause for an aggregate call, for example:

::

   SELECT xmlagg(x ORDER BY y DESC) FROM test;
   xmlagg
   ----------------------
   <bar/><foo>abc</foo>
   (1 row)

XML Predicates
--------------

-  xml IS DOCUMENT

Description: IS DOCUMENT returns true if the XML value of the parameter is a correct XML document; if the XML document is incorrect, false is returned. If the parameter is null, a null value is returned.

Return type: bool

-  xml IS NOT DOCUMENT

Description: Returns **true** if the XML value of the parameter is not a correct XML document. If the XML document is correct, **false** is returned. If the parameter is null, a null value is returned.

Return type: bool

-  XMLEXISTS(text PASSING [BY REF] xml [BY REF])

Description: If the **xpath** expression in the first parameter returns any node, the **XMLEXISTS** function returns true. Otherwise, the function returns false. (If any parameter is null, the result is null.) The BY REF clause is invalid and is used to maintain SQL compatibility.

Return type: bool

Example:

::

   SELECT xmlexists('//town[text() = ''Toronto'']' PASSING BY REF '<towns><town>Toronto</town><town>Ottawa</town></towns>');
   xmlexists
   -----------
   t
   (1 row)

-  xml_is_well_formed(text)

Description: Checks whether a text string is a well-formatted XML value and returns a Boolean result. If the **xmloption** parameter is set to **DOCUMENT**, the document is checked. If the **xmloption** parameter is set to **CONTENT**, the content is checked.

Return type: bool

Example:

::

   SELECT xml_is_well_formed('<abc/>');
   xml_is_well_formed
   --------------------
   t
   (1 row)

-  xml_is_well_formed_document(text)

Description: Checks whether a text string is a well-formatted text and returns a Boolean result.

Return type: bool

Example:

::

   SELECT xml_is_well_formed_document('<test:foo xmlns:test="http://test.com/test">bar</test:foo>');
   xml_is_well_formed_document
   -----------------------------
   t
   (1 row)

-  xml_is_well_formed_content(text)

Description: Checks whether a text string is a well-formatted content and returns a Boolean result.

Return type: bool

Example:

::

   SELECT xml_is_well_formed_content('content');
   xml_is_well_formed_content
   ----------------------------
   t
   (1 row)

Processing XML
--------------

-  xpath(xpath, xml [, nsarray])

Description: Returns an array of XML values corresponding to the set of nodes produced by the **xpath** expression. If the **xpath** expression returns a scalar value instead of a set of nodes, an array of individual elements is returned. The second parameter **xml** must be a complete XML document, which must have a root node element. The third parameter is an array map of a namespace. The array should be a two-dimensional text array, and the length of the second dimension should be **2**. (It should be an array of arrays, each containing exactly two elements). The first element of each array item is the alias of the namespace name, and the second element is the namespace URI. The alias provided in this array does not have to be the same as the alias used in the XML document itself. In other words, in the context of both XML documents and **xpath** functions, aliases are local.

Return type: XML value array

Example:

::

   SELECT xpath('/my:a/text()', '<my:a xmlns:my="http://example.com">test</my:a>', ARRAY[ARRAY['my', 'http://example.com']]);
   xpath
   --------
   {test}
   (1 row)

-  xpath_exists(xpath, xml [, nsarray])

Description: The **xpath_exists** function is a special form of the **xpath** function. This function does not return an XML value that satisfies the **xpath** function; it returns a Boolean value indicating whether the query is satisfied. This function is equivalent to the standard **XMLEXISTS** predicate, but it also provides support for a namespace mapping parameter.

Return type: bool

Example:

::

   SELECT xpath_exists('/my:a/text()', '<my:a xmlns:my="http://example.com">test</my:a>', ARRAY[ARRAY['my', 'http://example.com']]);
   xpath_exists
   --------------
   t
   (1 row)

-  xmltable

Description: Generates a table based on the input XML data, **XPath** expression, and column definition. An **xmltable** is similar to a function in syntax, but it can appear only as a table in the FROM clause of a query.

Return value: setof record

Syntax:

::

   XMLTABLE ( [ XMLNAMESPACES ( namespace_uri AS namespace_name [,  ...] ), ]
                   row_expression PASSING [ BY  { REF | VALUE } ]
   document_expression [ BY  { REF | VALUE } ]
   COLUMNS name  { type  [ PATH column_expression  ] [ DEFAULT default_expression ] [ NOT NULL | NULL ] | FOR ORDINALITY }
   [, ...]
   )

Parameter:

-  The optional XMLNAMESPACES clause is a comma-separated list of namespace definitions, where each **namespace_uri** is a text-type expression and each **namespace_name** is a simple identifier. XMLNAMESPACES specifies the XML namespaces used in the document and their aliases. The default namespace declaration is not supported.
-  The mandatory parameter **row_expression** is an **XPath** 1.0 expression. This expression calculates the sequence of XML nodes based on the provided XML document **document_expression**. The sequence is the sequence of converting **xmltable** to output lines. If the **document_expression** value is **NULL** or an empty node set generated by **row_expression**, no line is returned.
-  The **document_expression** parameter is used to input an XML document. The input document must be in the XML format. XML fragment data or XML documents in incorrect format are not accepted. The BY REF and BY VALUE clauses do not take effect. They are used only to implement SQL standard compatibility.
-  The COLUMNS clause specifies the column list definition in the output table. The column name and column data type are mandatory, and the path, default value, and whether the clause is empty are optional.

   -  **column_expression** of a column is an **XPath** 1.0 expression used to calculate the value of the column extracted from the current row based on **row_expression**. If **column_expression** is not specified, the field name is used as an implicit path.
   -  A column can be marked as **NOT NULL**. If **column_expression** in the **NOT NULL** column does not return any data, and there is no DEFAULT clause or the calculation result of **default_expression** is **NULL**, an error is reported.
   -  The columns marked as **FOR ORDINALITY** are filled with row numbers starting from **1**. The sequence is the node sequence retrieved from the **row_expression** result set. A maximum of one column can be marked as **FOR ORDINALITY**.

      .. important::

         **XPath** 1.0 does not specify the order for nodes, so the order in which results are returned depends on the order in which data is obtained.

Example:

::

   SELECT * FROM XMLTABLE('/ROWS/ROW'
   PASSING '<ROWS><ROW id="1"><COUNTRY_ID>AU</COUNTRY_ID><COUNTRY_NAME>Australia</COUNTRY_NAME></ROW><ROW id="2"><COUNTRY_ID>FR</COUNTRY_ID><COUNTRY_NAME>France</COUNTRY_NAME></ROW><ROW id="3"><COUNTRY_ID>SG</COUNTRY_ID><COUNTRY_NAME>Singapore</COUNTRY_NAME></ROW></ROWS>'
   COLUMNS id INT PATH '@id',
   _id FOR ORDINALITY,
   country_id TEXT PATH 'COUNTRY_ID',
   country_name TEXT PATH 'COUNTRY_NAME' NOT NULL);
   id  |   _id  | country_id | country_name
   ----+-----+---------------+--------------
     1 |      1 | AU         | Australia
     2 |      2 | FR         | France
     3 |      3 | SG         | Singapore
   (3 rows)

Mapping a Table to XML
----------------------

-  table_to_xml(tbl regclass, nulls boolean, tableforest boolean, targetns text)

Description: Maps the contents of a table to XML values.

Return type: XML

-  table_to_xmlschema(tbl regclass, nulls boolean, tableforest boolean, targetns text)

Description: Maps a relational table schema to an XML schema document.

Return type: XML

-  table_to_xml_and_xmlschema(tbl regclass, nulls boolean, tableforest boolean, targetns text)

Description: Maps a relational table to XML values and schema documents.

Return type: XML

-  query_to_xml(query text, nulls boolean, tableforest boolean, targetns text)

Description: Maps the contents of an SQL query to XML values.

Return type: XML

-  query_to_xmlschema(query text, nulls boolean, tableforest boolean, targetns text)

Description: Maps an SQL query into an XML schema document.

Return type: XML

-  query_to_xml_and_xmlschema(query text, nulls boolean, tableforest boolean, targetns text)

Description: Maps SQL queries to XML values and schema documents.

Return type: XML

-  cursor_to_xml(cursor refcursor, count int, nulls boolean, tableforest boolean, targetns text)

Description: Maps a cursor query to an XML value.

Return type: XML

-  cursor_to_xmlschema(cursor refcursor, nulls boolean, tableforest boolean, targetns text)

Description: Maps a cursor query to an XML schema document.

Return type: XML

-  schema_to_xml(schema name, nulls boolean, tableforest boolean, targetns text)

Description: Maps a table in a schema to an XML value.

Return type: XML

-  schema_to_xmlschema(schema name, nulls boolean, tableforest boolean, targetns text)

Description: Maps a table in a schema to an XML schema document.

Return type: XML

-  schema_to_xml_and_xmlschema(schema name, nulls boolean, tableforest boolean, targetns text)

Description: Maps a table in a schema to an XML value and a schema document.

Return type: XML

-  database_to_xml(nulls boolean, tableforest boolean, targetns text)

Description: Maps a database table to an XML value.

Return type: XML

-  database_to_xmlschema(nulls boolean, tableforest boolean, targetns text)

Description: Maps a database table to an XML schema document.

Return type: XML

-  database_to_xml_and_xmlschema(nulls boolean, tableforest boolean, targetns text)

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
