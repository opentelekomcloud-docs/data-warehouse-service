:original_name: dws_16_0083.html

.. _dws_16_0083:

COMMENT
=======

The figure below illustrates the process of simplifying **COMMENT** statements. A complex statement is entered and then simplified through migration.

**Input:**

::

   CREATE MULTISET TABLE PDAT.t_tbl_comment
       (
           Data_Dt  DATE           NOT NULL
          ,Data_Src  VARCHAR(4)   NOT NULL
          ,List_Make_Stat  CHAR(1)
       ) PRIMARY INDEX(Data_Dt,Data_Src) ;
   COMMENT ON PDAT.t_tbl_comment IS 'comment test on table';
   ---------------
   REPLACE VIEW  PVW.v_vw_comment AS
   LOCKING ROW FOR ACCESS
   SELECT Data_Dt, Data_Src, List_Make_Stat
   FROM PDAT.t_tbl_comment;
   COMMENT ON PVW.v_vw_comment IS 'comment test on view';
   COMMENT ON PVW.v_vw_comment.Data_Dt IS 'comment test on view column';

**Output:**

.. code-block::

   COMMENT ON TABLE PDAT.t_tbl_comment IS 'comment test on table';
   COMMENT ON VIEW PVW.v_vw_comment IS 'comment test on view';
   COMMENT ON COLUMN PVW.v_vw_comment.Data_Dt IS 'comment test on view column';
