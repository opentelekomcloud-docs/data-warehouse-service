:original_name: dws_04_0702.html

.. _dws_04_0702:

GS_WLM_OPERATOR_HISTORY
=======================

**GS_WLM_OPERATOR_HISTORY** displays the records of operators in jobs that have been executed by the current user on the current CN.

This view is used to query data from the GaussDB(DWS). Data in the GaussDB(DWS) is cleared periodically. If the GUC parameter :ref:`enable_resource_record <en-us_topic_0000001510522653__s5f116e109a2944e3854abcc56772eaa1>` is set to **on**, records in the view will be dumped to the system catalog :ref:`GS_WLM_OPERATOR_INFO <dws_04_0565>` every 3 minutes and deleted from the view. If **enable_resource_record** is set to **off**, the records will be deleted from the view after the retention period expires. The recorded data is the same as that described in :ref:`Table 1 <en-us_topic_0000001510522205__ta2c1b3b6469742cd9a313f0843410206>`.
