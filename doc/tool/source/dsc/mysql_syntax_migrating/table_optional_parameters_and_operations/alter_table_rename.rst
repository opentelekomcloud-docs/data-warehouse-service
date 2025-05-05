:original_name: dws_16_0121.html

.. _dws_16_0121:

ALTER TABLE RENAME
==================

GaussDB(DWS) prohibits including schema names in the **rename** clause, therefore, DSC supports renaming solely within the same schema. Renaming within the same schema omits the schema clause from the conversion result, while attempts to rename across schemas trigger an error report.

**Input**

::

   ALTER TABLE `shce1`.`t1` rename to `t2`;
   ALTER TABLE `shce1`.`t1` rename to t2;
   ALTER TABLE `charge_data`.`group_shengfen2022` RENAME `charge_data`.`group_shengfen2022_jiu`;
   ALTER TABLE `charge_data`.`group_shengfen2022` RENAME `charge_data`.`group_shengfen2022_jiu`, RENAME `charge_data`.`group_shengfen2023_jiu`, RENAME `charge_data`.`group_shengfen2024_jiu`;

**Output**

::

   ALTER TABLE "shce1"."t1" RENAME TO "t2";
   ALTER TABLE "shce1"."t1" RENAME TO "t2";
   ALTER TABLE "charge_data"."group_shengfen2022" RENAME TO "group_shengfen2022_jiu";
   ALTER TABLE "charge_data"."group_shengfen2022" RENAME TO "group_shengfen2022_jiu", RENAME TO "group_shengfen2023_jiu", RENAME TO "group_shengfen2024_jiu";
