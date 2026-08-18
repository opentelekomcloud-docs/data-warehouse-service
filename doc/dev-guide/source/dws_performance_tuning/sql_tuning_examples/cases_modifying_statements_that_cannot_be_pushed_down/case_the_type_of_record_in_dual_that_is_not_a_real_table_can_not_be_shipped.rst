:original_name: dws_04_0142.html

.. _dws_04_0142:

Case: The Type of Record in DUAL That Is Not a Real Table Can Not Be Shipped
============================================================================

Possible Cause
--------------

The optimizer finds the record column, but the main table for this column is not a real table. The most common cause is that the USING part of the MERGE INTO statement uses the FROM DUAL clause.

**Solution**: Rewrite the SQL statement to eliminate the DUAL clause.

Case 1: Using the Subquery f (SELECT 0 AS inv_id,0 AS ovs_pack_num,0 AS ovs_pack_id FROM DUAL)
----------------------------------------------------------------------------------------------

Original statement

::

   MERGE INTO dwiscm.dwi_inv_balance_onhand t USING (
       SELECT
           f.inv_id,
           d.ovs_pack_id
       FROM (SELECT 0 AS inv_id,0 AS ovs_pack_num,0 AS ovs_pack_id FROM DUAL) f,
       dwicommon.dwi_inv_oversea_pack d
       WHERE f.ovs_pack_num = d.ovs_pack_num
   ) S ON (t.inv_id = s.inv_id)
   WHEN MATCHED THEN UPDATE
   SET t.ovs_pack_id = s.ovs_pack_id, t.last_upd_cycle_id = 20250301000000, t.dw_last_update_date = sysdate;

|image1|

Rewritten statement

::

   MERGE INTO dwiscm.dwi_inv_balance_onhand t USING (
       SELECT
           0 AS inv_id,
           d.ovs_pack_id
       FROM dwicommon.dwi_inv_oversea_pack d
       WHERE 0 = d.ovs_pack_num
   ) S ON (t.inv_id = s.inv_id)
   WHEN MATCHED THEN UPDATE
   SET t.ovs_pack_id = s.ovs_pack_id, t.last_upd_cycle_id = 20250301000000, t.dw_last_update_date = sysdate;

|image2|

Modification comparison

|image3|

.. |image1| image:: /_static/images/en-us_image_0000002526271425.png
.. |image2| image:: /_static/images/en-us_image_0000002526313205.png
.. |image3| image:: /_static/images/en-us_image_0000002493993772.png
