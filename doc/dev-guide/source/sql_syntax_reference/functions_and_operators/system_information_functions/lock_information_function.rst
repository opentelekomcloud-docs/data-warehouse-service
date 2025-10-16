:original_name: dws_06_0346.html

.. _dws_06_0346:

Lock Information Function
=========================

pgxc_get_lock_conflicts()
-------------------------

Description: Obtains information about conflicting locks in the cluster. When a lock is waiting for another lock or another lock is waiting for it, a lock conflict occurs.

Return type: SETOF record

Example:

::

   SELECT * from pgxc_get_lock_conflicts();
                                                pgxc_get_lock_conflicts

   -----------------------------------------------------------------------------------------------------------------
    (relation,cn_5001,gaussdb,u1,test,,,,,omm,648406,,,"<insufficient privilege>",140651661784832,ExclusiveLock,f,2024-10-29 17:24:13.425672+08,)
   (1 row
