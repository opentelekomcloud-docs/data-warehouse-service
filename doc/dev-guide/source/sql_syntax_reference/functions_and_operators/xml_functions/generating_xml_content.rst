:original_name: dws_06_0351.html

.. _dws_06_0351:

Generating XML Content
======================

Expressions of functions and class functions in this section can be used to generate XML content from SQL data. This method is used to format query results into XML documents for processing in client applications.

XMLPARSE ( { DOCUMENT \| CONTENT } *value*)
-------------------------------------------

Description: Generates an XML value from character data.

Return type: XML

Example:

::

   SELECT xmlparse(document '<foo>bar</foo>');
       xmlparse
   ----------------
    <foo>bar</foo>
   (1 row)

XMLSERIALIZE ( { DOCUMENT \| CONTENT } *value* AS *type*
--------------------------------------------------------

Description: Generates a string from XML values.

Return type: type, which can be character, character varying, or text (or its alias)

Example:

::

   SELECT xmlserialize(content 'good' AS CHAR(10));
    xmlserialize
   --------------
    good
   (1 row)

xmlcomment(text)
----------------

Description: Creates an XML note that uses the specified text as the content. The text cannot contain (--) or end with a hyphen (-). If the parameter is null, the result is also null.

Return type: XML

Example:

::

   SELECT xmlcomment('hello');
     xmlcomment
   --------------
    <!--hello-->
   (1 row)

xmlconcat(xml[, ...])
---------------------

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

xmlelement(name name [, xmlattributes(value [AS attname] [, ... ])] [, content, ...])
-------------------------------------------------------------------------------------

Description: Generates an XML element with the given name, attribute, and content.

Return type: XML

Example:

::

   SELECT xmlelement(name foo);
    xmlelement
   ------------
    <foo/>
   (1 row)

   SELECT xmlelement(name foo, xmlattributes('xyz' as bar));
       xmlelement
   ------------------
    <foo bar="xyz"/>
   (1 row)

   SELECT xmlelement(name foo, xmlattributes(current_date as bar), 'cont', 'ent');
                xmlelement
   -------------------------------------
    <foo bar="2023-08-16">content</foo>
   (1 row)

Element and attribute names without valid XML names are escaped by replacing invalid characters with the sequence **\_xHHHH\_**, where **HHHH** is the Unicode code points of the character expressed in hexadecimal format. Example:

::

   SELECT xmlelement(name "foo$bar", xmlattributes('xyz' as "a&b"));

               xmlelement
   ----------------------------------
    <foo_x0024_bar a_x0026_b="xyz"/>

If the attribute value is a column reference, you do not need to specify an explicit attribute name, and the column name is used as the attribute name by default. In other cases, the attribute must be given an explicit name. So this example is legal:

::

   CREATE TABLE test (a xml, b xml);
   SELECT xmlelement(name test, xmlattributes(a, b)) FROM test;

But these are illegal:

::

   SELECT xmlelement(name test, xmlattributes('constant'), a, b) FROM test;
   SELECT xmlelement(name test, xmlattributes(func(a, b))) FROM test;

The element content, if specified, will be formatted based on its data type. If the content itself is of the XML type, a complex XML document will be constructed. Example:

::

   SELECT xmlelement(name foo, xmlattributes('xyz' as bar),xmlelement(name abc),xmlcomment('test'),xmlelement(name xyz));
                     xmlelement
   ----------------------------------------------
    <foo bar="xyz"><abc/><!--test--><xyz/></foo>

Other types of content will be formatted into valid XML character data. This means that special characters **<**, **>**, and **&** will be converted to entities. Binary data (data type bytea) is represented as Base64 or hexadecimal code, depending on the configuration parameter **xmlbinary**.

xmlforest(content [AS name] [, ...])
------------------------------------

Description: Generates an XML forest (sequence) of an element with a given name and content.

Return type: XML

Example:

::

   SELECT xmlforest('abc' AS foo, 123 AS bar);
             xmlforest
   ------------------------------
    <foo>abc</foo><bar>123</bar>
   (1 row)

   SELECT xmlforest(table_name, column_name) FROM ALL_TAB_COLUMNS WHERE schema = 'pg_catalog';
                                                          xmlforest
   ------------------------------------------------------------------------------------------------------------------------
    <table_name>pg_authid</table_name><column_name>rolsuper</column_name>
    <table_name>pg_authid</table_name><column_name>rolinherit</column_name>
    <table_name>pg_authid</table_name><column_name>rolcreaterole</column_name>

xmlpi(name target [, content])
------------------------------

Description: Creates an XML processing instruction. The content cannot contain the character sequence of **?>**.

Return type: XML

Example:

::

   SELECT xmlpi(name php, 'echo "hello world";');
               xmlpi
   -----------------------------
    <?php echo "hello world";?>
   (1 row)

xmlroot(xml, version text \| no value [, standalone yes|no|no value])
---------------------------------------------------------------------

Description: Modifies the attributes of the root node of an XML value. If a version is specified, it replaces the value in the version declaration of the root node. If a **standalone** value is specified, it replaces the **standalone** value in the root node.

Return type: XML

Example:

::

   SELECT xmlroot(xmlparse(document '<?xml version="1.0" standalone="no"?><content>abc</content>'), version '1.1', standalone yes);
                              xmlroot
   --------------------------------------------------------------
    <?xml version="1.1" standalone="yes"?><content>abc</content>
   (1 row)

xmlagg(xml)
-----------

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
    <bar/><foo>abc</foo>
   (1 row)

Add an **ORDER BY** clause to an aggregate call to determine the concatenation sequence. For example:

::

   SELECT xmlagg(x ORDER BY y DESC) FROM test;
           xmlagg
   ----------------------
    <bar/><foo>abc</foo>
   (1 row)
