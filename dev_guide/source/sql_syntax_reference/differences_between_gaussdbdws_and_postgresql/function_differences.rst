:original_name: dws_06_0005.html

.. _dws_06_0005:

Function Differences
====================

For details about the functions supported by GaussDB(DWS), see :ref:`Functions and Operators <dws_06_0027>`.

The following PostgreSQL functions are not supported:

-  Enum support functions
-  Access privilege inquiry functions

   -  has_sequence_privilege(user, sequence, privilege)
   -  has_sequence_privilege(sequence, privilege)

-  System catalog information functions

   -  pg_get_triggerdef(trigger_oid)
   -  pg_get_triggerdef(trigger_oid, pretty_bool)

-  Line functions
-  pg_node_tree
