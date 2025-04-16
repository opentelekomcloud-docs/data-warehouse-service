:original_name: dws_16_0094.html

.. _dws_16_0094:

.. _en-us_topic_0000001860198905:

DELETE
======

**DELETE** (:ref:`short key <en-us_topic_0000001860198793>` abbreviated as **DEL**) is an ANSI-compliant SQL syntax operator used to delete existing records from a table. DSC supports the Teradata **DELETE** statement and its short key **DEL**. The **DELETE** statement that does not contain the **WHERE** clause is migrated to **TRUNCATE** in GaussDB(DWS). Use the :ref:`deleteToTruncate <en-us_topic_0000001813438796__en-us_topic_0000001432527901_li2884123118322>` parameter to enable or disable this behavior.

**Input: DELETE**

::

   DEL FROM tab1
    WHERE a =10;

**Output**

.. code-block:: text

   DELETE FROM tab1
    WHERE a =10;

**Input: DELETE without WHERE - Migrated to TRUNCATE** **if deletetoTruncate=TRUE**

.. code-block:: text

   DELETE FROM ${schemaname} . "tablename" ALL;

**Output**

::

   TRUNCATE
        TABLE
             ${schemaname} . "tablename";

**In DELETE, the same table is used in DELETE and FROM clauses with / without WHERE clause**

**Input**

.. code-block:: text

   DELETE DP_TMP.M_P_TX_SCV_REMAINING_PARTY
   FROM DP_TMP.M_P_TX_SCV_REMAINING_PARTY ALL ;
   ---
   DELETE DP_VMCTLFW.CTLFW_Process_Id
   FROM DP_VMCTLFW.CTLFW_Process_Id
   WHERE (Process_Name =  :_spVV2 )
   AND  (Process_Id  NOT IN (SELECT MAX(Process_Id )(NAMED Process_Id )
                                         FROM DP_VMCTLFW.CTLFW_Process_Id
                                        WHERE Process_Name =  :_spVV2 )
         );
   ---
   DELETE CPID
   FROM DP_VMCTLFW.CTLFW_Process_Id AS CPID
   WHERE (Process_Name =  :_spVV2 )
   AND  (Process_Id  NOT IN (SELECT MAX(Process_Id )(NAMED Process_Id )
                                         FROM DP_VMCTLFW.CTLFW_Process_Id
                                        WHERE Process_Name =  :_spVV2 )
         );

**Output**

.. code-block:: text

   DELETE FROM DP_TMP.M_P_TX_SCV_REMAINING_PARTY;
   ---
   DELETE FROM DP_VMCTLFW.CTLFW_Process_Id
   WHERE (Process_Name =  :_spVV2 )
   AND  (Process_Id  NOT IN (SELECT MAX(Process_Id )(NAMED Process_Id )
                                         FROM DP_VMCTLFW.CTLFW_Process_Id
                                        WHERE Process_Name =  :_spVV2 )
         );
   ---
   DELETE FROM DP_VMCTLFW.CTLFW_Process_Id AS CPID
   WHERE (Process_Name =  :_spVV2 )
   AND  (Process_Id  NOT IN (SELECT MAX(Process_Id )(NAMED Process_Id )
                                         FROM DP_VMCTLFW.CTLFW_Process_Id
                                        WHERE Process_Name =  :_spVV2 )
         );

DELETE table_alias FROM table
-----------------------------

**Input**

::

   SQL_Detail10124.sql
   delete a
     from ${BRTL_DCOR}.BRTL_CS_POT_CUST_UMPAY_INF_S as a
    where a.DW_Snsh_Dt = cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd')
    and a.DW_Job_Seq = 1 ;
   was migrated as below:
         DELETE FROM
              BRTL_DCOR.BRTL_CS_POT_CUST_UMPAY_INF_S AS a
                   USING
         WHERE a.DW_Snsh_Dt = CAST( lv_mig_v_Trx_Dt AS DATE )
              AND a.DW_Job_Seq = 1 ;
   SQL_Detail10449.sql
   delete a
     from ${BRTL_DCOR}.BRTL_EM_YISHITONG_USR_INF as a
    where a.DW_Job_Seq = 1 ;
   was migrated as below:
         DELETE FROM
              BRTL_DCOR.BRTL_EM_YISHITONG_USR_INF AS a
                   USING
         WHERE a.DW_Job_Seq = 1 ;
   SQL_Detail5742.sql
   delete a
     from ${BRTL_DCOR}.BRTL_PD_FP_NAV_ADT_INF as a;
   was migrated as
         DELETE a
    FROM
         BRTL_DCOR.BRTL_PD_FP_NAV_ADT_INF AS a ;

**Output**

::

   SQL_Detail10124.sql
   delete from ${BRTL_DCOR}.BRTL_CS_POT_CUST_UMPAY_INF_S as a
    where a.DW_Snsh_Dt = cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd')
    and a.DW_Job_Seq = 1 ;
   SQL_Detail10449.sql
   delete from ${BRTL_DCOR}.BRTL_EM_YISHITONG_USR_INF as a
    where a.DW_Job_Seq = 1 ;
   SQL_Detail5742.sql
   delete from ${BRTL_DCOR}.BRTL_PD_FP_NAV_ADT_INF as a;
