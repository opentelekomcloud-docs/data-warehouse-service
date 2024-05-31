:original_name: dws_16_0121.html

.. _dws_16_0121:

ALTER TABLE RENAME
==================

In GaussDB(DWS), the rename clause cannot contain the schema name. The DSC tool supports only the rename operation in the same schema. After the rename operation, the schema clause is removed from the conversion result. If the rename operation is performed across schemas, an error is reported.

**Input**

.. code-block::

   alter table `shce1`.`t1` rename to `t2`;
   alter table `shce1`.`t1` rename to t2;
   ALTER TABLE `charge_data`.`group_shengfen2022` RENAME `charge_data`.`group_shengfen2022_jiu`;
   ALTER TABLE `charge_data`.`group_shengfen2022` RENAME `charge_data`.`group_shengfen2022_jiu`, RENAME `charge_data`.`group_shengfen2023_jiu`, RENAME `charge_data`.`group_shengfen2024_jiu`;

**Output**

.. code-block::

   ALTER TABLE "shce1"."t1" RENAME TO "t2";
   ALTER TABLE "shce1"."t1" RENAME TO "t2";
   ALTER TABLE "charge_data"."group_shengfen2022" RENAME TO "group_shengfen2022_jiu";
   ALTER TABLE "charge_data"."group_shengfen2022" RENAME TO "group_shengfen2022_jiu", RENAME TO "group_shengfen2023_jiu", RENAME TO "group_shengfen2024_jiu";
