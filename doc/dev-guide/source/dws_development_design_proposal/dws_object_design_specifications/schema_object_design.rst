:original_name: dws_04_0110.html

.. _dws_04_0110:

Schema Object Design
====================

.. _en-us_topic_0000002100586262__en-us_topic_0000002135806925_section82725914114:

Suggestion 2.7: Avoiding the Creation of Objects Under Other Users' Private Schemas
-----------------------------------------------------------------------------------

.. note::

   A private schema refers to a schema with the same name as the user when the user is created. This schema is only accessible to the user.

   **Impact of rule violation:**

   -  When you create an object under someone else's private schema, the permissions for that object are determined by the schema owner.

   **Solution:**

   -  Create objects under your own private schema to have full control over the object permissions.
