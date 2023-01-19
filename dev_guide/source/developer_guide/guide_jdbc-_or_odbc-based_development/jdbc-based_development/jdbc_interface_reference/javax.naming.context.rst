:original_name: dws_04_0114.html

.. _dws_04_0114:

javax.naming.Context
====================

This section describes **javax.naming.Context**, the context interface for connection configuration.

.. table:: **Table 1** Support status for javax.naming.Context

   ====================================== =========== ==============
   Method Name                            Return Type Support JDBC 4
   ====================================== =========== ==============
   bind(Name name, Object obj)            void        Yes
   bind(String name, Object obj)          void        Yes
   lookup(Name name)                      Object      Yes
   lookup(String name)                    Object      Yes
   rebind(Name name, Object obj)          void        Yes
   rebind(String name, Object obj)        void        Yes
   rename(Name oldName, Name newName)     void        Yes
   rename(String oldName, String newName) void        Yes
   unbind(Name name)                      void        Yes
   unbind(String name)                    void        Yes
   ====================================== =========== ==============
