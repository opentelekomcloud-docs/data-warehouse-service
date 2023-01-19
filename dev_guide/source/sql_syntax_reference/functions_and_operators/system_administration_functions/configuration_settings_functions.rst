:original_name: dws_06_0053.html

.. _dws_06_0053:

Configuration Settings Functions
================================

Configuration setting functions are used for querying and modifying configuration parameters during running.

-  current_setting(setting_name)

   Description: Specifies the current setting.

   Return type: text

   Note: **current_setting** obtains the current setting of **setting_name** by query. It is equivalent to the **SHOW** statement. For example:

   ::

      SELECT current_setting('datestyle');

       current_setting
      -----------------
       ISO, MDY
      (1 row)

-  set_config(setting_name, new_value, is_local)

   Description: Sets the parameter and returns a new value.

   Return type: text

   Note: **set_config** sets the parameter **setting_name** to **new_value**. If **is_local** is **true**, the new value will only apply to the current transaction. If you want the new value to apply for the current session, use **false** instead. The function corresponds to the **SET** statement. For example:

   ::

      SELECT set_config('log_statement_stats', 'off', false);

       set_config
      ------------
       off
      (1 row)
