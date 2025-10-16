:original_name: dws_06_0352.html

.. _dws_06_0352:

XML Predicates
==============

The functions in this section check the attributes of an XML value.

xml IS DOCUMENT
---------------

Description: IS DOCUMENT returns true if the XML value of the parameter is a correct XML document; if the XML document is incorrect, false is returned. If the parameter is null, a null value is returned.

Return type: bool

::

   SELECT '<abc/>' is document;
    ?column?
   ----------
    t
   (1 row)

xml IS NOT DOCUMENT
-------------------

Description: Returns **true** if the XML value of the parameter is not a correct XML document. If the XML document is correct, **false** is returned. If the parameter is null, a null value is returned.

Return type: bool

::

   SELECT 'abc' is document;
    ?column?
   ----------
    f
   (1 row)

XMLEXISTS(text PASSING [BY REF] xml [BY REF])
---------------------------------------------

Description: If the **xpath** expression in the first parameter returns any node, the **XMLEXISTS** function returns true. Otherwise, the function returns false. (If any parameter is null, the result is null.) The BY REF clause is invalid and is used to maintain SQL compatibility.

Return type: bool

Example:

::

   SELECT xmlexists('//town[text() = ''TScity'']' PASSING BY REF '<towns><town>TScity</town><town>TOcity</town></towns>');
    xmlexists
   -----------
    t
   (1 row)

xml_is_well_formed(text)
------------------------

Description: Checks whether a text string is a well-formatted XML value and returns a Boolean result. If the **xmloption** parameter is set to **DOCUMENT**, the document is checked. If the **xmloption** parameter is set to **CONTENT**, the content is checked.

Return type: bool

Example:

::

   SET xmloption TO DOCUMENT;
   SET

   SELECT xml_is_well_formed('<>');
    xml_is_well_formed
   --------------------
    f
   (1 row)

   SELECT xml_is_well_formed('<abc/>');
    xml_is_well_formed
   --------------------
    t
   (1 row)

   SET xmloption TO CONTENT;
   SELECT xml_is_well_formed('abc');
    xml_is_well_formed
   --------------------
    t
   (1 row)

xml_is_well_formed_document(text)
---------------------------------

Description: Checks whether a text string is a well-formatted text and returns a Boolean result.

Return type: bool

Example:

::

   SELECT xml_is_well_formed_document('<test:foo xmlns:test="http://test.com/test">bar</test:foo>');
    xml_is_well_formed
   --------------------
    t
   (1 row)

   SELECT xml_is_well_formed_document('<test:foo xmlns:test="http://test.com/test">bar</my:foo>');
    xml_is_well_formed_document
   -----------------------------
    f
   (1 row)

xml_is_well_formed_content(text)
--------------------------------

Description: Checks whether a text string is a well-formatted content and returns a Boolean result.

Return type: bool

Example:

::

   SELECT xml_is_well_formed_content('content');
    xml_is_well_formed_content
   ----------------------------
    t
   (1 row)

   SELECT xml_is_well_formed_content('<content');
    xml_is_well_formed_content
   ----------------------------
    f
   (1 row)
