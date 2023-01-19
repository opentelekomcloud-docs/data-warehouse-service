:original_name: dws_06_0025.html

.. _dws_06_0025:

XML
===

XML data type stores Extensible Markup Language (XML) formatted data. Such data can also be stored as text, but the advantage of the XML data type is that it checks whether each stored value is a well-formed XML value. XML can store well-formed documents and content fragments defined by XML standards. A content fragment can have multiple top-level elements or character nodes.

For functions that support the XML data type, see :ref:`XML Functions <dws_06_0067>`.

Configuring XML Parameters
--------------------------

The syntax is as follows:

::

   SET XML OPTION { DOCUMENT | CONTENT };
   SET xmloption TO { DOCUMENT | CONTENT };

If a string value is not converted to XML using the XMLPARSE or XMLSERIALIZE function, the XML OPTION session parameter determines the value, DOCUMENT or CONTENT.

The default value is CONTENT, indicating that all types of XML data are allowed.

Example:

::

   SET XML OPTION DOCUMENT;
   SET
   SET xmloption TO DOCUMENT;
   SET

Configuring Binary Data Encoding Format
---------------------------------------

Syntax:

::

   SET xmlbinary TO { base64 | hex};

Example:

::

   SET xmlbinary TO base64;
   SET

   SELECT xmlelement(name foo, bytea 'bar');
   xmlelement
   -----------------
   <foo>YmFy</foo>
   (1 row)

   SET xmlbinary TO hex;
   SET

   SELECT xmlelement(name foo, bytea 'bar');
   xmlelement
   -------------------
   <foo>626172</foo>
   (1 row)

Accessing XML Value
-------------------

The XML data type is special, and it does not provide any comparison operators, because there is no general comparison algorithm for XML data, so you cannot retrieve data rows by comparing an XML value with a search value. An XML data entry is typically accompanied by an ID for retrieving. Alternatively, you can convert XML values into character strings. However, this is not widely applicable to common scenarios of XML value comparison.
