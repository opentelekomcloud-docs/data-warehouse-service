:original_name: dws_04_0902.html

.. _dws_04_0902:

Checkpoints
===========

.. _en-us_topic_0000001098655058__sd7daa0171e7245968cf6a44d968e1ad5:

checkpoint_segments
-------------------

**Parameter description**: Specifies the minimum number of WAL segment files in the period specified by :ref:`checkpoint_timeout <en-us_topic_0000001098655058__s8acbdb7a59af4102bf229fcf4c889f37>`. The size of each log file is 16 MB.

**Type**: SIGHUP

**Value range**: an integer. The minimum value is **1**.

**Default value**: **64**

.. important::

   Increasing the value of this parameter speeds up the export of big data. Set this parameter based on :ref:`checkpoint_timeout <en-us_topic_0000001098655058__s8acbdb7a59af4102bf229fcf4c889f37>` and :ref:`shared_buffers <en-us_topic_0000001145694783__s9292cfbf38fa4b17b93e9a47330da753>`. This parameter affects the number of WAL log segment files that can be reused. Generally, the maximum number of reused files in the **pg_xlog** folder is twice the number of checkpoint segments. The reused files are not deleted and are renamed to the WAL log segment files which will be later used.

.. _en-us_topic_0000001098655058__s8acbdb7a59af4102bf229fcf4c889f37:

checkpoint_timeout
------------------

**Parameter description**: Specifies the maximum time between automatic WAL checkpoints.

**Type**: SIGHUP

**Value range:** an integer ranging from 30 to 3600 (s)

**Default value**: **15min**

.. important::

   If the value of :ref:`checkpoint_segments <en-us_topic_0000001098655058__sd7daa0171e7245968cf6a44d968e1ad5>` is increased, you need to increase the value of this parameter. The increase of them further requires the increase of :ref:`shared_buffers <en-us_topic_0000001145694783__s9292cfbf38fa4b17b93e9a47330da753>`. Consider all these parameters during setting.

checkpoint_completion_target
----------------------------

**Parameter description**: Specifies the target of checkpoint completion, as a fraction of total time between checkpoints.

**Type**: SIGHUP

**Value range**: 0.0 to 1.0. The default value 0.5 indicates that each checkpoint must be completed within 50% of the checkpoint interval.

**Default value**: **0.5**

checkpoint_warning
------------------

**Parameter description**: Specifies a time in seconds. If the checkpoint interval is close to this time due to filling of checkpoint segment files, a message is sent to the server log to increase the value of :ref:`checkpoint_segments <en-us_topic_0000001098655058__sd7daa0171e7245968cf6a44d968e1ad5>`.

**Type**: SIGHUP

**Value range**: an integer (unit: s). **0** indicates that warning is disabled.

**Default value**: **5min**

**Recommended value**: **5min**

checkpoint_wait_timeout
-----------------------

**Parameter description**: Specifies the longest time that the checkpoint waits for the checkpointer thread to start.

**Type**: SIGHUP

**Value range**: an integer ranging from 2 to 3600 (s)

**Default value**: **1min**
