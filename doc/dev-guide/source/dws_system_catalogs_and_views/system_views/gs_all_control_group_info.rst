:original_name: dws_04_0688.html

.. _dws_04_0688:

GS_ALL_CONTROL_GROUP_INFO
=========================

**GS_ALL_CONTROL_GROUP_INFO** displays all Cgroup information in a database.

.. table:: **Table 1** GS_ALL_CONTROL_GROUP_INFO columns

   ======== ====== ==================================================
   Column   Type   Description
   ======== ====== ==================================================
   Name     Text   Name of the Cgroup
   type     Text   Type of the Cgroup
   gid      Bigint Cgroup ID
   classgid Bigint ID of the Class Cgroup to which a Workload belongs
   class    Text   Class Cgroup
   workload Text   Workload Cgroup
   shares   Bigint CPU quota allocated to a Cgroup
   limits   Bigint Limit of CPUs allocated to a Cgroup
   wdlevel  Bigint Workload Cgroup level
   cpucores Text   Usage of CPU cores in a Cgroup
   ======== ====== ==================================================
