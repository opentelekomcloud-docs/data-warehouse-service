:original_name: dws_mt_0042.html

.. _dws_mt_0042:

SQL Formatter
=============

SQL Formatter improves the readability of SQL statements. It formats SQL statements by adding/deleting rows and indenting context, and is used to format migrated output files. It can also be used for importing files.

Use the :ref:`formattedSourceRequired <en-us_topic_0000001234042167__en-us_topic_0218440356_li236173918102>` parameter to enable/disable the SQL formatter for the source SQL files. If this parameter is set to **true**, the copy of the input file is formatted and saved to the *Output path*\ **\\formattedSource** directory.

SQL Formatter also supports Teradata SQL migration and Oracle SQL migration. SQL scripts in Teradata Perl file migration are also formatted. The Oracle (Beta) tool does not support SQL Formatter.

**Input: SQL FORMATTER**

::

   select                 p1.parti_encode                  ,p1.accting_type_cd                 ,p1.prod_code                ,p1.cust_type_cd                 ,p1.accting_amt_type_cd                 ,p1.accting_num_1                 ,p1.accting_num_2                  ,p1.accting_num_3                  ,p1.accting_num_4                 ,p1.accting_num_5                ,p1.accting_num_6                 ,p1.start_dt                  ,p1.pre_effect_debit_gl_num                  ,p1.pre_effect_crdt_gl_num                  ,p1.after_effect_debit_gl_num                  ,p1.after_effect_crdt_gl_num                ,coalesce( p2.start_dt ,cast( '30001231' as date format 'yyyymmdd' ) )                  ,p1.accting_term                  ,p1.etl_job             from                 (                     select                             rank (                                 start_dt asc                             ) as start_dt_id                             ,parti_encode                              ,accting_type_cd                              ,prod_code                             ,cust_type_cd                              ,accting_amt_type_cd                              ,accting_num_1                              ,accting_num_2                              ,accting_num_3                             ,accting_num_4                            ,accting_num_5                            ,accting_num_6                            ,start_dt                             ,pre_effect_debit_gl_num                             ,pre_effect_crdt_gl_num                              ,after_effect_debit_gl_num                              ,after_effect_crdt_gl_num                              ,accting_term                              ,etl_job                          from                             ccting_subj_para_h_mf0_a_cur_i                  ) p1 left join (                     select                             rank (                                 start_dt asc                             ) - 1 as start_dt_id                             ,parti_encode                             ,accting_type_cd                            ,prod_code                              ,cust_type_cd                             ,accting_amt_type_cd                             ,accting_num_1                             ,accting_num_2                             ,accting_num_3                           ,accting_num_4                              ,accting_num_5                             ,accting_num_6                              ,start_dt                           ,accting_term                          from                             ccting_subj_para_h_mf0_a_cur_i                 ) p2                     on p1.start_dt_id = p2.start_dt_id                 and p1.parti_encode = p2.parti_encode                 and p1.accting_type_cd = p2.accting_type_cd                 and p1.prod_code = p2.prod_code                 and p1.cust_type_cd = p2.cust_type_cd                 and p1.accting_amt_type_cd = p2.accting_amt_type_cd                 and p1.accting_num_1 = p2.accting_num_1                 and p1.accting_num_2 = p2.accting_num_2                 and p1.accting_num_3 = p2.accting_num_3                 and p1.accting_num_4 = p2.accting_num_4                 and p1.accting_num_5 = p2.accting_num_5                 and p1.accting_num_6 = p2.accting_num_6                 and p1.accting_term = p2.accting_term ;

**Output**

::

   SELECT
             p1.parti_encode                      ,p1.accting_type_cd
             ,p1.prod_code
             ,p1.cust_type_cd                     ,p1.accting_amt_type_cd
             ,p1.accting_num_1
             ,p1.accting_num_2
             ,p1.accting_num_3
             ,p1.accting_num_4
             ,p1.accting_num_5
             ,p1.accting_num_6
             ,p1.start_dt
             ,p1.pre_effect_debit_gl_num
             ,p1.pre_effect_crdt_gl_num
             ,p1.after_effect_debit_gl_num
             ,p1.after_effect_crdt_gl_num
             ,COALESCE( p2.start_dt ,CAST( '30001231' AS DATE ) )
             ,p1.accting_term                    ,p1.etl_job
        FROM
             (
                  SELECT
                            rank (
                            ) over( ORDER BY start_dt ASC ) AS start_dt_id
                            ,parti_encode
                            ,accting_type_cd
                            ,prod_code
                            ,cust_type_cd
                            ,accting_amt_type_cd
                            ,accting_num_1
                            ,accting_num_2
                            ,accting_num_3
                            ,accting_num_4
                            ,accting_num_5
                            ,accting_num_6
                            ,start_dt                                         ,pre_effect_debit_gl_num                          ,pre_effect_crdt_gl_num
                            ,after_effect_debit_gl_num
                            ,after_effect_crdt_gl_num
                            ,accting_term                                       ,etl_job                                       FROM
                            ccting_subj_para_h_mf0_a_cur_i
             ) p1 LEFT JOIN (
                  SELECT
                            rank (
                            ) over( ORDER BY start_dt ASC ) - 1 AS start_dt_id
                            ,parti_encode
                            ,accting_type_cd
                            ,prod_code
                            ,cust_type_cd
                            ,accting_amt_type_cd
                            ,accting_num_1
                            ,accting_num_2
                            ,accting_num_3
                            ,accting_num_4
                            ,accting_num_5
                            ,accting_num_6
                            ,start_dt
                            ,accting_term
                       FROM
                            ccting_subj_para_h_mf0_a_cur_i
             ) p2
                  ON p1.start_dt_id = p2.start_dt_id
             AND p1.parti_encode = p2.parti_encode
             AND p1.accting_type_cd = p2.accting_type_cd
             AND p1.prod_code = p2.prod_code
             AND p1.cust_type_cd = p2.cust_type_cd
             AND p1.accting_amt_type_cd = p2.accting_amt_type_cd
             AND p1.accting_num_1 = p2.accting_num_1
             AND p1.accting_num_2 = p2.accting_num_2
             AND p1.accting_num_3 = p2.accting_num_3
             AND p1.accting_num_4 = p2.accting_num_4
             AND p1.accting_num_5 = p2.accting_num_5
             AND p1.accting_num_6 = p2.accting_num_6
             AND p1.accting_term = p2.accting_term
   ;
