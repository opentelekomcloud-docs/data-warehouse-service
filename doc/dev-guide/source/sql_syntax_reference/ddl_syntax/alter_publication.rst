:original_name: dws_06_0284.html

.. _dws_06_0284:

ALTER PUBLICATION
=================

Function
--------

**ALTER PUBLICATION** modifies the publication attributes.

Precautions
-----------

-  This statement is supported by version 8.2.0.100 or later clusters.
-  This statement can be used by the owner of a publication and the system administrator only.
-  To alter the publication owner, you must also be a direct or indirect member of the new owning role, and that role must have CREATE privilege on the database.
-  In a publication with **FOR ALL TABLES**, the new publication owner must be the system administrator.
-  An administrator can change the owner relationship of any publication.

Syntax
------

-  Adds objects to a publication.

   ::

      ALTER PUBLICATION name ADD publication_object [, ...]

-  Deletes objects from a publication.

   ::

      ALTER PUBLICATION name DROP publication_object [, ...]

-  Replaces the current object with a specified object.

   ::

      ALTER PUBLICATION name SET publication_object [, ...]

-  Sets publication parameters. Retains the previous values of the parameters that are not mentioned.

   ::

      ALTER PUBLICATION name SET ( publication_parameter [= value] [, ... ] )

-  Changes the publication owner.

   ::

      ALTER PUBLICATION name OWNER TO new_owner

-  Renames the publication.

   ::

      ALTER PUBLICATION name RENAME TO new_name

The syntax of using **publication_object** is as follows:

.. code-block::

   TABLE table_name [, ...]
   | ALL TABLES IN SCHEMA schema_name [, ... ]

Parameter Description
---------------------

-  **name**

   Specifies the publication name you want to modify.

   Value range: A string. It must comply with the naming convention.

-  **table_name**

   Specifies the name of an existing table.

   Value range: A string. It must comply with the naming convention.

-  **schema_name**

   Specifies the name of an existing schema.

   Value range: A string. It must comply with the naming convention.

-  **SET ( publication_parameter [= value] [, ... ] )**

   Modifies the publication parameters initially set by **CREATE PUBLICATION**. For details about the parameters, see :ref:`Parameter description <en-us_topic_0000001811634585__li11304141792615>` of CREATE PUBLICATION.

-  **new_owner**

   Specifies the new publication owner.

-  **new_name**

   Specifies the new name of a publication.

Examples
--------

-  Add a table to a publication.

   .. code-block::

      ALTER PUBLICATION mypublication ADD TABLE mydata2;

-  Deletes a schema from a publication.

   .. code-block::

      ALTER PUBLICATION mypublication DROP ALL TABLES IN SCHEMA myschema1;

-  Reset a publication object.

   .. code-block::

      ALTER PUBLICATION mypublication SET TABLE mydata2, ALL TABLES IN SCHEMA myschema2;

-  Change the publication owner.

   .. code-block::

      ALTER PUBLICATION mypublication OWNER TO user1;

-  Change the publication name.

   .. code-block::

      ALTER PUBLICATION mypublication RENAME TO mypublication1;

Helpful Links
-------------

:ref:`CREATE PUBLICATION <dws_06_0285>`, :ref:`DROP PUBLICATION <dws_06_0286>`
