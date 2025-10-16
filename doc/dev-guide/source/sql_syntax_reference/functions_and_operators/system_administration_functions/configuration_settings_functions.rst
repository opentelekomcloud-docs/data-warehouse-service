:original_name: dws_06_0053.html

.. _dws_06_0053:

Configuration Settings Functions
================================

Configuration setting functions are used for querying and modifying configuration parameters during running.

current_setting(setting_name)
-----------------------------

Description: Specifies the current setting.

Return type: text

Note: **current_setting** obtains the current setting of **setting_name** by query. It is equivalent to the **SHOW** statement. For example:

::

   SELECT current_setting('datestyle');

    current_setting
   -----------------
    ISO, MDY
   (1 row)

read_global_var(global_setting_name)
------------------------------------

Description: Reads the current value of a global variable.

Return type: text

Note: **read_global_var** is used query the current value of **global_setting_name**. It is equivalent to the **SHOW** statement.

Example:

::

   SET my.var = 1000;
   SELECT read_global_var('my.var');
    read_global_var
   -----------------
    1000
   (1 row)

.. note::

   **global_setting_name** indicates a variable similar to **my.var**. The variable name contains decimal points and the left and right sides of the decimal points are not empty. Variables of this type are called global variables.

   Global variables can be set only by running the **SET** command. Methods such as **gs_guc**, **ALTER DATABASE dbname SET paraname TO value**, and **ALTER USER username SET paraname TO value** are not supported. Global variables are visible to all users.

set_config(setting_name, new_value, is_local)
---------------------------------------------

Description: Sets the parameter and returns a new value.

Return type: text

Note: **set_config** sets the parameter **setting_name** to **new_value**. If **is_local** is **true**, the new value will only apply to the current transaction. If you want the new value to apply for the current session, use **false** instead. The function corresponds to the **SET** statement. For example:

::

   SELECT set_config('log_statement_stats', 'off', false);

    set_config
   ------------
    off
   (1 row)

.. caution::

   If **behavior_compat_options** is set to **DISABLE_SET_GLOBAL_VAR_ON_DATANODE**, you cannot use this function to set global variables on DNs.
