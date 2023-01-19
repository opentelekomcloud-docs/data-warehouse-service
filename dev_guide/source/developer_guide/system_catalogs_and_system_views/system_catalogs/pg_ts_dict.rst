:original_name: dws_04_0626.html

.. _dws_04_0626:

PG_TS_DICT
==========

**PG_TS_DICT** records entries that define text search dictionaries. A dictionary depends on a text search template, which specifies all the implementation functions needed. The dictionary itself provides values for the user-settable parameters supported by the template.

This division of labor allows dictionaries to be created by unprivileged users. The parameters are specified by a text string **dictinitoption**, whose format and meaning vary depending on the template.

.. table:: **Table 1** PG_TS_DICT columns

   +----------------+------+-----------------------------------------+----------------------------------------------------------------+
   | Name           | Type | Reference                               | Description                                                    |
   +================+======+=========================================+================================================================+
   | oid            | oid  | ``-``                                   | Row identifier (hidden attribute; must be explicitly selected) |
   +----------------+------+-----------------------------------------+----------------------------------------------------------------+
   | dictname       | name | ``-``                                   | Text search dictionary name                                    |
   +----------------+------+-----------------------------------------+----------------------------------------------------------------+
   | dictnamespace  | oid  | :ref:`PG_NAMESPACE <dws_04_0600>`.oid   | OID of the namespace that contains the dictionary              |
   +----------------+------+-----------------------------------------+----------------------------------------------------------------+
   | dictowner      | oid  | :ref:`PG_AUTHID <dws_04_0574>`.oid      | Owner of the dictionary                                        |
   +----------------+------+-----------------------------------------+----------------------------------------------------------------+
   | dicttemplate   | oid  | :ref:`PG_TS_TEMPLATE <dws_04_0628>`.oid | OID of the text search template for this dictionary            |
   +----------------+------+-----------------------------------------+----------------------------------------------------------------+
   | dictinitoption | text | ``-``                                   | Initialization option string for the template                  |
   +----------------+------+-----------------------------------------+----------------------------------------------------------------+
