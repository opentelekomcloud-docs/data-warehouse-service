:original_name: dws_04_0109.html

.. _dws_04_0109:

USER Object Design
==================

.. _en-us_topic_0000002136265437__en-us_topic_0000002100207550_section348916349406:

Rule 2.5: Following the Least Privilege Principle and Avoiding Running Services Using Users with Special Permissions
--------------------------------------------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  Administrators have full access to a lot of things in the system and using these users to run services can pose security and control risks.

   **Solution:**

   -  It is advised to use common users for service running, reserving users with special permissions for management operations.

.. _en-us_topic_0000002136265437__en-us_topic_0000002100207550_section20111104754013:

Rule 2.6: Avoiding the Use of a Single Database Account for All Services
------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  Using a single database user for all services hinders effective service management and control. In abnormal situations, it becomes impossible to isolate specific users for emergency purposes.

   **Solution:**

   -  Create administrators, service operation users, and O&M users for different purposes.
   -  Use different users to run different services for improved management and allocation of services and resources.
