:original_name: dws_16_0078.html

.. _dws_16_0078:

.. _en-us_topic_0000001860318493:

ACCESS LOCK
===========

ACCESS LOCK allows you to read the data from a table that may have been locked for the READ or WRITE.

Use the :ref:`tdMigrateLOCKoption <en-us_topic_0000001813438796__en-us_topic_0000001432527901_li18084318118>` configuration parameter to configure migration of query containing the LOCK keyword. If **tdMigrateLOCKOption** is set to **false**, the tool will skip migration of the query and will log a message.

**Input - ACCESS LOCK** **(tdMigrateLOCKOption=True**)

::

   LOCKING TABLE tab1 FOR ACCESS
    INSERT INTO tab2
   SELECT …
    FROM …
     WHERE ...;

**Output:**

::

   /* LOCKING TABLE tab1 FOR ACCESS */
   INSERT INTO tab2
   SELECT …
    FROM …
    WHERE ...;
