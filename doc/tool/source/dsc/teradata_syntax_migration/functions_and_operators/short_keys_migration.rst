:original_name: dws_16_0054.html

.. _dws_16_0054:

.. _en-us_topic_0000001860198793:

Short Keys Migration
====================

:ref:`Table 1 <en-us_topic_0000001860198793__en-us_topic_0000001384550464_table39390038143532>` lists the short keys supported by Teradata and their equivalent syntax in GaussDB(DWS).

.. _en-us_topic_0000001860198793__en-us_topic_0000001384550464_table39390038143532:

.. table:: **Table 1** List of short keys

   ================== ===================
   Teradata Short Key GaussDB(DWS) Syntax
   ================== ===================
   SEL                SELECT
   INS                INSERT
   UPD                UPDATE
   DEL                DELETE
   CT                 CREATE TABLE
   CV                 CREATE VIEW
   BT                 START TRANSACTION
   ET                 COMMIT
   ================== ===================

**Input - BT**

.. code-block::

   BT;
   --
   delete from ${BRTL_DCOR}.BRTL_CS_CUST_CID_UID_REL
   where DW_Job_Seq = ${v_Group_No};

   .if ERRORCODE <> 0 then .quit 12;

   --
   insert into ${BRTL_DCOR}.BRTL_CS_CUST_CID_UID_REL
   (
      Cust_Id
     ,Cust_UID
     ,DW_Upd_Dt
     ,DW_Upd_Tm
     ,DW_Job_Seq
     ,DW_Etl_Dt
   )
   select
      a.Cust_Id
     ,a.Cust_UID
     ,current_date as Dw_Upd_Dt
     ,current_time(0) as DW_Upd_Tm
     ,cast(${v_Group_No} as byteint) as DW_Job_Seq
     ,cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd') as DW_Etl_Dt
   from ${BRTL_VCOR}.BRTL_CS_CUST_CID_UID_REL_S a
   where a.DW_Snsh_Dt = cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd');

   .if ERRORCODE <> 0 then .quit 12;

   ET;cd ..

**Output:**

.. code-block::

   BEGIN
   --
      BEGIN
         delete from ${BRTL_DCOR}.BRTL_CS_CUST_CID_UID_REL
          where DW_Job_Seq = ${v_Group_No};
            lv_mig_errorcode = 0;
      EXCEPTION
         WHEN OTHERS THEN
            lv_mig_errorcode = -1;
      END;

      IF lv_mig_errorcode <> 0 THEN
           RAISE EXCEPTION '12';
      END IF;

   --
      BEGIN
           insert into ${BRTL_DCOR}.BRTL_CS_CUST_CID_UID_REL
           (
             Cust_Id
            ,Cust_UID
            ,DW_Upd_Dt
            ,DW_Upd_Tm
            ,DW_Job_Seq
            ,DW_Etl_Dt
          )
         select
             a.Cust_Id
            ,a.Cust_UID
            ,current_date as Dw_Upd_Dt
            ,current_time(0) as DW_Upd_Tm
            ,cast(${v_Group_No} as byteint) as DW_Job_Seq
            ,cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd') as DW_Etl_Dt
          from ${BRTL_VCOR}.BRTL_CS_CUST_CID_UID_REL_S a
        where a.DW_Snsh_Dt = cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd');
      EXCEPTION
         WHEN OTHERS THEN
            lv_mig_errorcode = -1;
      END;

      IF lv_mig_errorcode <> 0 THEN
           RAISE EXCEPTION '12';
      END IF;

   END;
