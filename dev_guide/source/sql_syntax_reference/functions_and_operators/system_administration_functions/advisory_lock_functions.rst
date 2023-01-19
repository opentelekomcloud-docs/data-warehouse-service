:original_name: dws_06_0059.html

.. _dws_06_0059:

Advisory Lock Functions
=======================

Advisory lock functions manage advisory locks. These functions are only for internal use currently.

-  pg_advisory_lock(key bigint)

   Description: Obtains an exclusive session-level advisory lock.

   Return type: void

   Note: **pg_advisory_lock** locks resources defined by an application. The resources can be identified using a 64-bit or two nonoverlapped 32-bit key values. If another session locks the resources, the function blocks the resources until they can be used. The lock is exclusive. Multiple locking requests are pushed into the stack. Therefore, if the same resource is locked three times, it must be unlocked three times so that it is released to another session.

-  pg_advisory_lock(key1 int, key2 int)

   Description: Obtains an exclusive session-level advisory lock.

   Return type: void

-  pg_advisory_lock_shared(key bigint)

   Description: Obtains a shared session-level advisory lock.

   Return type: void

-  pg_advisory_lock_shared(key1 int, key2 int)

   Description: Obtains a shared session-level advisory lock.

   Return type: void

   Note: **pg_advisory_lock_shared** works in the same way as **pg_advisory_lock**, except the lock can be shared with other sessions requesting shared locks. Only would-be exclusive lockers are locked out.

-  pg_advisory_unlock(key bigint)

   Description: Releases an exclusive session-level advisory lock.

   Return type: boolean

-  pg_advisory_unlock(key1 int, key2 int)

   Description: Releases an exclusive session-level advisory lock.

   Return type: boolean

   Note: **pg_advisory_unlock** releases the obtained exclusive advisory lock. If the release is successful, the function returns **true**. If the lock was not held, it will return **false**. In addition, a SQL warning will be reported by the server.

-  pg_advisory_unlock_shared(key bigint)

   Description: Releases a shared session-level advisory lock.

   Return type: boolean

-  pg_advisory_unlock_shared(key1 int, key2 int)

   Description: Releases a shared session-level advisory lock.

   Return type: boolean

   Note: **pg_advisory_unlock_shared** works in the same way as **pg_advisory_unlock**, except it releases a shared session-level advisory lock.

-  pg_advisory_unlock_all()

   Description: Releases all advisory locks owned by the current session.

   Return type: void

   Note: **pg_advisory_unlock_all** releases all advisory locks owned by the current session. The function is implicitly invoked when the session ends even if the client is abnormally disconnected.

-  pg_advisory_xact_lock(key bigint)

   Description: Obtains an exclusive transaction-level advisory lock.

   Return type: void

-  pg_advisory_xact_lock(key1 int, key2 int)

   Description: Obtains an exclusive transaction-level advisory lock.

   Return type: void

   Note: **pg_advisory_xact_lock** works in the same way as **pg_advisory_lock**, except the lock is automatically released at the end of the current transaction and cannot be released explicitly.

-  pg_advisory_xact_lock_shared(key bigint)

   Description: Obtains a shared transaction-level advisory lock.

   Return type: void

-  pg_advisory_xact_lock_shared(key1 int, key2 int)

   Description: Obtains a shared transaction-level advisory lock.

   Return type: void

   Note: **pg_advisory_xact_lock_shared** works in the same way as **pg_advisory_lock_shared**, except the lock is automatically released at the end of the current transaction and cannot be released explicitly.

-  pg_try_advisory_lock(key bigint)

   Description: Obtains an exclusive session-level advisory lock if available.

   Return type: boolean

   Note: **pg_try_advisory_lock** is similar to **pg_advisory_lock**, except **pg_try_advisory_lock** does not block the resource until the resource is released. **pg_try_advisory_lock** either immediately obtains the lock and returns **true** or returns **false**, which indicates the lock cannot be performed currently.

-  pg_try_advisory_lock(key1 int, key2 int)

   Description: Obtains an exclusive session-level advisory lock if available.

   Return type: boolean

-  pg_try_advisory_lock_shared(key bigint)

   Description: Obtains a shared session-level advisory lock if available.

   Return type: boolean

-  pg_try_advisory_lock_shared(key1 int, key2 int)

   Description: Obtains a shared session-level advisory lock if available.

   Return type: boolean

   Note: **pg_try_advisory_lock_shared** is similar to **pg_try_advisory_lock**, except **pg_try_advisory_lock_shared** attempts to obtain a shared lock instead of an exclusive lock.

-  pg_try_advisory_xact_lock(key bigint)

   Description: Obtains an exclusive transaction-level advisory lock if available.

   Return type: boolean

-  pg_try_advisory_xact_lock(key1 int, key2 int)

   Description: Obtains an exclusive transaction-level advisory lock if available.

   Return type: boolean

   Note: **pg_try_advisory_xact_lock** works in the same way as **pg_try_advisory_lock**, except the lock, if acquired, is automatically released at the end of the current transaction and cannot be released explicitly.

-  pg_try_advisory_xact_lock_shared(key bigint)

   Description: Obtains a shared transaction-level advisory lock if available.

   Return type: boolean

-  pg_try_advisory_xact_lock_shared(key1 int, key2 int)

   Description: Obtains a shared transaction-level advisory lock if available.

   Return type: boolean

   Note: **pg_try_advisory_xact_lock_shared** works in the same way as **pg_try_advisory_lock_shared**, except the lock, if acquired, is automatically released at the end of the current transaction and cannot be released explicitly.
