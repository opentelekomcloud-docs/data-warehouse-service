:original_name: dws_mt_0216.html

.. _dws_mt_0216:

CONNECT BY
==========

**Input-CONNECT BY**

.. code-block::

   select id from city_branch start with id=roleBranchId connect by prior id=parent_id;
   SELECT T.BRANCH_LEVEL, t.ID
                       FROM city_branch c
                      WHERE     (c.branch_level = '1' OR T.BRANCH_LEVEL = '2')
                            AND (T.SIGN = '1' OR T.SIGN = '4' OR T.SIGN = '8')
                            AND T.STATUS = '1'
                 START WITH c.ID = I_BRANCH_ID
                 CONNECT BY c.ID = PRIOR c.parent_id
                   ORDER BY c.branch_level DESC ;

**Output**

.. code-block::

   WITH RECURSIVE migora_cte AS (
        SELECT
                  id
                  ,1 AS LEVEL
             FROM
                  city_branch
             WHERE
                  id = roleBranchId
        UNION
        ALL SELECT
                mig_ora_cte_join_alias.id
                ,mig_ora_cte_tab_alias.LEVEL + 1 AS LEVEL
             FROM
                migora_cte mig_ora_cte_tab_alias INNER JOIN city_branch mig_ora_cte_join_alias
                    ON mig_ora_cte_tab_alias.id = mig_ora_cte_join_alias.parent_id
   ) SELECT
             id
        FROM
             migora_cte
        ORDER BY
             LEVEL
   ;

   WITH RECURSIVE migora_cte AS (
        SELECT
                  BRANCH_LEVEL
                  ,ID
                  ,SIGN
                  ,STATUS
                  ,parent_id
                  ,1 AS LEVEL
             FROM
                  city_branch c
             WHERE
                  c.ID = I_BRANCH_ID
        UNION
        ALL SELECT
                  c.BRANCH_LEVEL
                  ,c.ID
                  ,c.SIGN
                  ,c.STATUS
                  ,c.parent_id
                  ,mig_ora_cte_tab_alias.LEVEL + 1 AS LEVEL
             FROM
                  migora_cte mig_ora_cte_tab_alias INNER JOIN city_branch c
                       ON c.ID = mig_ora_cte_tab_alias.parent_id
   ) SELECT
             BRANCH_LEVEL
             ,ID
        FROM
             migora_cte c
        WHERE
             (
                  c.branch_level = '1'
                  OR T.BRANCH_LEVEL = '2'
             )
             AND( T.SIGN = '1' OR T.SIGN = '4' OR T.SIGN = '8' )
             AND T.STATUS = '1'
        ORDER BY
             c.branch_level DESC
   ;

**Input - CONNECT BY multiple tables**

The syntax shows the relationship between each child row and its parent row. It uses the **CONNECT BY** *xxx* **PRIOR** clause to define the relationship between the current row (child row) and the previous row (parent row).

.. code-block::

   SELECT DISTINCT a.id menuId,
                  F.name menuName,
                  a.status menuState,
                  a.parent_id menuParentId,
                  '-1' menuPrivilege,
                  a.serialNo menuSerialNo
     FROM CTP_MENU a, CTP_MENU_NLS F
     START WITH a.serialno in (1, 2, 3)
   CONNECT BY a.id = PRIOR a.parent_id
          AND f.locale = Language
          AND a.id = f.id
   ORDER BY menuId, menuParentId;

**Output**

.. code-block::

   WITH RECURSIVE migora_cte AS (
           SELECT pr.service_product_id
                , t.enabled_flag
                , pr.operation_id
                , pr.enabled_flag
                , pr.product_code
                , 1 AS LEVEL
             FROM asms.cppsv_operation_sort t
                , asms.cppsv_product_class pr
            WHERE level_id = 3
              AND pr.operation_id = t.operation_id(+)
         UNION ALL
            SELECT pr.service_product_id
                 , t.enabled_flag
                 , pr.operation_id
                 , pr.enabled_flag
                 , pr.product_code
                 , mig_ora_cte_tab_alias.LEVEL + 1 AS LEVEL
              FROM migora_cte mig_ora_cte_tab_alias
                 , asms.cppsv_operation_sort t
                 , asms.cppsv_product_class pr
             WHERE mig_ora_cte_tab_alias.service_product_id = pr.service_product_father_id
               AND pr.operation_id = t.operation_id(+) )
   SELECT pr.service_product_id
     FROM migora_cte
    WHERE nvl( UPPER( enabled_flag ) ,'Y' ) = 'Y'
      AND nvl( enabled_flag ,'Y' ) = 'Y'
      AND pr.product_code = rec_product1.service_product_code
    ORDER BY LEVEL;
