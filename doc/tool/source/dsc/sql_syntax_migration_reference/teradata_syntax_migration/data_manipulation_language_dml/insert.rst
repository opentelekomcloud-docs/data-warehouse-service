:original_name: dws_16_0086.html

.. _dws_16_0086:

INSERT
======

The Teradata **INSERT** (:ref:`short key <en-us_topic_0000001772696108>` INS) statement is used to insert records into a table. DSC supports the **INSERT** statement.

The **INSERT INTO TABLE table_name** syntax is used in Teradata SQL, but is not supported by GaussDB(DWS). GaussDB(DWS) supports only **INSERT INTO table_name**. Therefore, remove the keyword **TABLE** when using DSC.

**Input**

.. code-block::

   INSERT TABLE tab1
   SELECT col1, col2
     FROM tab2
    WHERE col3 > 0;

**Output**

.. code-block::

   INSERT INTO tab1
   SELECT col1, col2
     FROM tab2
    WHERE col3 > 0;
