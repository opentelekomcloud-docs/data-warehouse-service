:original_name: dws_04_0616.html

.. _dws_04_0616:

PG_SHDEPEND
===========

**PG_SHDEPEND** records the dependency relationships between database objects and shared objects, such as roles. This information allows GaussDB(DWS) to ensure that those objects are unreferenced before attempting to delete them.

See also :ref:`PG_DEPEND <dws_04_0585>`, which performs a similar function for dependencies involving objects within a single database.

Unlike most system catalogs, **PG_SHDEPEND** is shared across all databases of a cluster: there is only one copy of **PG_SHDEPEND** per cluster, not one per database.

.. table:: **Table 1** PG_SHDEPEND columns

   +------------+---------+--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name       | Type    | Reference                            | Description                                                                                                                                                |
   +============+=========+======================================+============================================================================================================================================================+
   | dbid       | OID     | :ref:`PG_DATABASE <dws_04_0582>`.oid | OID of the database the dependent object is in. The value is **0** for a shared object.                                                                    |
   +------------+---------+--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | classid    | OID     | :ref:`PG_CLASS <dws_04_0578>`.oid    | OID of the system catalog the dependent object is in.                                                                                                      |
   +------------+---------+--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | objid      | OID     | Any OID column                       | OID of the specific dependent object                                                                                                                       |
   +------------+---------+--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | objsubid   | Integer | ``-``                                | For a table column, this is the column number (the **objid** and **classid** refer to the table itself). For all other object types, this column is **0**. |
   +------------+---------+--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | refclassid | OID     | :ref:`PG_CLASS <dws_04_0578>`.oid    | OID of the system catalog the referenced object is in (must be a shared catalog)                                                                           |
   +------------+---------+--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | refobjid   | OID     | Any OID column                       | OID of the specific referenced object                                                                                                                      |
   +------------+---------+--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | deptype    | Char    | ``-``                                | Code segment defining the specific semantics of this dependency relationship. See the following text for details.                                          |
   +------------+---------+--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | objfile    | Text    | ``-``                                | Path of the user-defined C function library file.                                                                                                          |
   +------------+---------+--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+

In all cases, a **pg_shdepend** entry indicates that the referenced object cannot be dropped without also dropping the dependent object. However, there are several subflavors defined by **deptype**:

-  SHARED_DEPENDENCY_OWNER (o)

   The referenced object (which must be a role) is the owner of the dependent object.

-  SHARED_DEPENDENCY_ACL (a)

   The referenced object (which must be a role) is mentioned in the ACL (access control list, i.e., privileges list) of the dependent object. (A **SHARED_DEPENDENCY_ACL** entry is not made for the owner of the object, since the owner will have a **SHARED_DEPENDENCY_OWNER** entry anyway.)

-  SHARED_DEPENDENCY_PIN (p)

   There is no dependent object. This type of entry is a signal that the system itself depends on the referenced object, and so that object must never be deleted. Entries of this type are created only by **initdb**. The columns for the dependent object contain zeroes.
