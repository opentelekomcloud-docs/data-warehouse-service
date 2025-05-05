:original_name: DWS_DS_037.html

.. _DWS_DS_037:

Managing Connections in the SQL Terminal
========================================

In Data Studio, you can either use an existing connection or set up a new one in the SQL terminal to view the execution plan and cost, visualize the plan explanation, and work with the result set. By default, the SQL terminal reuses existing connections. When multiple queries are queued for execution in an existing connection, consider using a new connection to avoid delays caused by sequential execution. However, it is best to reuse existing connections when working with temporary tables. For how to edit temporary tables, see :ref:`Editing Temporary Tables <dws_ds_020>`.

Perform the following steps to change the connection reuse setting:

#. Click |image1| to enable or disable the connection reuse function in the SQL terminal.

   For details about query execution behavior during connection reuse and creation, see :ref:`FAQs <en-us_topic_0000001813598528__en-us_topic_0185264547_li18661844113712>`.

   .. note::

      You can only edit temporary tables in existing connections.

.. |image1| image:: /_static/images/en-us_image_0000001860199201.png
