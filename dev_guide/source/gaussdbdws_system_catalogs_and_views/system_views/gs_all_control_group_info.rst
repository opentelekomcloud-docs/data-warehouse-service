:original_name: dws_04_0688.html

.. _dws_04_0688:

GS_ALL_CONTROL_GROUP_INFO
=========================

**GS_ALL_CONTROL_GROUP_INFO** displays all Cgroup information in a database.

.. table:: **Table 1** GS_ALL_CONTROL_GROUP_INFO columns

   ======== ====== ==================================================
   Name     Type   Description
   ======== ====== ==================================================
   name     text   Name of the Cgroup
   type     text   Type of the Cgroup
   gid      bigint Cgroup ID
   classgid bigint ID of the Class Cgroup to which a Workload belongs
   class    text   Class Cgroup
   workload text   Workload Cgroup
   shares   bigint CPU quota allocated to a Cgroup
   limits   bigint Limit of CPUs allocated to a Cgroup
   wdlevel  bigint Workload Cgroup level
   cpucores text   Usage of CPU cores in a Cgroup
   ======== ====== ==================================================
