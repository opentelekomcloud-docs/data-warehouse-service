:original_name: dws_06_0066.html

.. _dws_06_0066:

Trigger Functions
=================

-  pg_get_triggerdef(oid)

   Description: Obtains the definition information of a trigger.

   Parameter: OID of the trigger to be queried

   Return type: text

   Example:

   ::

      select pg_get_triggerdef(oid) from pg_trigger;
                                                        pg_get_triggerdef
      ----------------------------------------------------------------------------------------------------------------------
       CREATE TRIGGER insert_trigger BEFORE INSERT ON test_trigger_src_tbl FOR EACH ROW EXECUTE PROCEDURE tri_insert_func()
      (1 row)

-  pg_get_triggerdef(oid, boolean)

   Description: Obtains the definition information of a trigger.

   Parameter: OID of the trigger to be queried and whether it is displayed in pretty mode

   Return type: text

   .. note::

      The Boolean parameters take effect only when the WHEN condition is specified during trigger creation.

   Example:

   ::

      select pg_get_triggerdef(oid,true)from pg_trigger;
                                                        pg_get_triggerdef
      ----------------------------------------------------------------------------------------------------------------------
       CREATE TRIGGER insert_trigger BEFORE INSERT ON test_trigger_src_tbl FOR EACH ROW EXECUTE PROCEDURE tri_insert_func()
      (1 row)

      select pg_get_triggerdef(oid,false)from pg_trigger;
                                                        pg_get_triggerdef
      ----------------------------------------------------------------------------------------------------------------------
       CREATE TRIGGER insert_trigger BEFORE INSERT ON test_trigger_src_tbl FOR EACH ROW EXECUTE PROCEDURE tri_insert_func()
      (1 row)
