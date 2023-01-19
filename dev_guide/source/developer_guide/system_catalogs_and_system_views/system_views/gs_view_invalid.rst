:original_name: dws_04_0717.html

.. _dws_04_0717:

GS_VIEW_INVALID
===============

**GS_VIEW_INVALID** queries all unavailable views visible to the current user. If the base table, function, or synonym that the view depends on is abnormal, the **validtype** column of the view is displayed as "invalid".

.. table:: **Table 1** GS_VIEW_INVALID columns

   ========== ==== ======================
   Column     Type Description
   ========== ==== ======================
   oid        oid  OID of the view
   schemaname name View space name
   viewname   name Name of the view
   viewowner  name Owner of the view
   definition text Definition of the view
   validtype  text View validity flag
   ========== ==== ======================
