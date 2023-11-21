:original_name: dws_mt_0221.html

.. _dws_mt_0221:

Creating a Schema for Package
=============================

The pacakge declaration is miagrated as a schema named after the package. The migration can be performed after **pkgSchemaNaming** is set to **false**.

**Input - Create schema for Package**

.. code-block::

   CREATE OR REPLACE EDITIONABLE PACKAGE "PACK_DEMO"."PACKAGE_GET_NOVA_INFO" AS

         TYPE  novalistcur  is REF CURSOR;
       PROCEDURE getNovaInfo (
           i_appEnShortName     IN       VARCHAR2,
           o_flag           OUT    VARCHAR2,
           o_errormsg   OUT    VARCHAR2,
           o_novalist      OUT    novalistcur
       );

**Output**

.. code-block::

   /*~~PACKAGE_GET_NOVA_INFO~~*/
   CREATE
        SCHEMA PACKAGE_GET_NOVA_INFO
   ;
