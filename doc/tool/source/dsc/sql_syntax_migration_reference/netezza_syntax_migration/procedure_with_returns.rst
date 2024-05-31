:original_name: dws_07_6822.html

.. _dws_07_6822:

.. _en-us_topic_0000001772696312:

PROCEDURE with RETURNS
======================

PROCEDURE with RETURNS will be modified to FUNCTION with RETURN.

+----------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+
| Netezza Syntax                                                                                                 | Syntax After Migration                                                                                                |
+================================================================================================================+=======================================================================================================================+
| ::                                                                                                             | ::                                                                                                                    |
|                                                                                                                |                                                                                                                       |
|    CREATE OR REPLACE PROCEDURE "DWDB"."EDW"."SP_O_HXYW_LNSACCTINFO_H"(CHARACTER VARYING(8))                    |    CREATE OR REPLACE FUNCTION "EDW"."SP_O_HXYW_LNSACCTINFO_H"(CHARACTER VARYING(8))                                   |
|    RETURNS INTEGER                                                                                             |    RETURN INTEGER                                                                                                     |
|    LANGUAGE NZPLSQL AS                                                                                         |    AS                                                                                                                 |
|    BEGIN_PROC                                                                                                  |        V_PAR_DAY ALIAS for $1;                                                                                        |
|    DECLARE                                                                                                     |        V_PRCNAME    NCHAR VARYING(50):= 'SP_O_HXYW_LNSACCTINFO_H';                                                    |
|        V_PAR_DAY ALIAS for $1;                                                                                 |        V_CNT        INTEGER;                                                                                          |
|        V_PRCNAME    NVARCHAR(50):= 'SP_O_HXYW_LNSACCTINFO_H';                                                  |        V_STEP_INFO   NCHAR VARYING(500);                                                                              |
|        V_CNT        INTEGER;                                                                                   |        D_TIME_START TIMESTAMP:= CURRENT_TIMESTAMP;                                                                    |
|        V_STEP_INFO   NVARCHAR(500);                                                                            |        O_RETURN INTEGER;                                                                                              |
|        D_TIME_START TIMESTAMP:= CURRENT_TIMESTAMP;                                                             |    BEGIN                                                                                                              |
|        O_RETURN INTEGER;                                                                                       |      O_RETURN := 0;                                                                                                   |
|    BEGIN                                                                                                       |                                                                                                                       |
|      O_RETURN := 0;                                                                                            |      /* Writes logs and starts the recording process. */                                                              |
|                                                                                                                |      SP_LOG_EXEC(V_PRCNAME,V_PAR_DAY,D_TIME_START,0,0,'The process running',' ');                                     |
|      --Writes logs and starts the recording process.                                                           |                                                                                                                       |
|      CALL SP_LOG_EXEC(V_PRCNAME,V_PAR_DAY,D_TIME_START,0,0,'The process running ',' ');                        |      V_STEP_INFO := '1.Initialization';                                                                               |
|                                                                                                                |      BEGIN                                                                                                            |
|      V_STEP_INFO := '1.Initialization';                                                                        |        /* 1.1 */                                                                                                      |
|      BEGIN                                                                                                     |        SELECT COUNT(*) INTO V_CNT FROM information_schema.columns WHERE TABLE_NAME=lower('TMPO_HXYW_LNSACCTINFO_H1'); |
|        --1.1                                                                                                   |        if V_CNT>0 then                                                                                                |
|        SELECT COUNT(*) INTO V_CNT FROM information_schema.columns WHERE TABLE_NAME='TMPO_HXYW_LNSACCTINFO_H1'; |           EXECUTE IMMEDIATE 'DROP TABLE TMPO_HXYW_LNSACCTINFO_H1';                                                    |
|        if V_CNT>0 then                                                                                         |        end if;                                                                                                        |
|           EXECUTE IMMEDIATE 'DROP TABLE TMPO_HXYW_LNSACCTINFO_H1';                                             |      END;                                                                                                             |
|        end if;                                                                                                 |                                                                                                                       |
|      END;                                                                                                      |     RETURN O_RETURN;                                                                                                  |
|                                                                                                                |                                                                                                                       |
|     RETURN O_RETURN;                                                                                           |    END;                                                                                                               |
|                                                                                                                |    /                                                                                                                  |
|    END;                                                                                                        |                                                                                                                       |
|    END_PROC;                                                                                                   |                                                                                                                       |
+----------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001772696312__en-us_topic_0000001657865518_en-us_topic_0238518432_en-us_topic_0237362167_en-us_topic_0202686277_section1049710281559:

Qualifying Language
-------------------

Migrate the nzplSQL language to the plpgSQL language or delete the language.

+----------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+
| Netezza Syntax                                                                                                 | Syntax After Migration                                                                                                |
+================================================================================================================+=======================================================================================================================+
| ::                                                                                                             | ::                                                                                                                    |
|                                                                                                                |                                                                                                                       |
|    CREATE OR REPLACE PROCEDURE "DWDB"."EDW"."SP_O_HXYW_LNSACCTINFO_H"(CHARACTER VARYING(8))                    |    CREATE OR REPLACE FUNCTION "EDW"."SP_O_HXYW_LNSACCTINFO_H"(CHARACTER VARYING(8))                                   |
|    RETURNS INTEGER                                                                                             |    RETURN INTEGER                                                                                                     |
|    LANGUAGE NZPLSQL AS                                                                                         |    AS                                                                                                                 |
|    BEGIN_PROC                                                                                                  |        V_PAR_DAY ALIAS for $1;                                                                                        |
|    DECLARE                                                                                                     |        V_PRCNAME    NCHAR VARYING(50):= 'SP_O_HXYW_LNSACCTINFO_H';                                                    |
|        V_PAR_DAY ALIAS for $1;                                                                                 |        V_CNT        INTEGER;                                                                                          |
|        V_PRCNAME    NVARCHAR(50):= 'SP_O_HXYW_LNSACCTINFO_H';                                                  |        V_STEP_INFO   NCHAR VARYING(500);                                                                              |
|        V_CNT        INTEGER;                                                                                   |        D_TIME_START TIMESTAMP:= CURRENT_TIMESTAMP;                                                                    |
|        V_STEP_INFO   NVARCHAR(500);                                                                            |        O_RETURN INTEGER;                                                                                              |
|        D_TIME_START TIMESTAMP:= CURRENT_TIMESTAMP;                                                             |    BEGIN                                                                                                              |
|        O_RETURN INTEGER;                                                                                       |      O_RETURN := 0;                                                                                                   |
|    BEGIN                                                                                                       |                                                                                                                       |
|      O_RETURN := 0;                                                                                            |      /* Writes logs and starts the recording process. */                                                              |
|                                                                                                                |      SP_LOG_EXEC(V_PRCNAME,V_PAR_DAY,D_TIME_START,0,0,'The process running',' ');                                     |
|      --Writes logs and starts the recording process.                                                           |                                                                                                                       |
|      CALL SP_LOG_EXEC(V_PRCNAME,V_PAR_DAY,D_TIME_START,0,0,'The process running ',' ');                        |      V_STEP_INFO := '1.Initialization';                                                                               |
|                                                                                                                |      BEGIN                                                                                                            |
|      V_STEP_INFO := '1.Initialization';                                                                        |        /* 1.1 */                                                                                                      |
|      BEGIN                                                                                                     |        SELECT COUNT(*) INTO V_CNT FROM information_schema.columns WHERE TABLE_NAME=lower('TMPO_HXYW_LNSACCTINFO_H1'); |
|        --1.1                                                                                                   |        if V_CNT>0 then                                                                                                |
|        SELECT COUNT(*) INTO V_CNT FROM information_schema.columns WHERE TABLE_NAME='TMPO_HXYW_LNSACCTINFO_H1'; |           EXECUTE IMMEDIATE 'DROP TABLE TMPO_HXYW_LNSACCTINFO_H1';                                                    |
|        if V_CNT>0 then                                                                                         |        end if;                                                                                                        |
|           EXECUTE IMMEDIATE 'DROP TABLE TMPO_HXYW_LNSACCTINFO_H1';                                             |      END;                                                                                                             |
|        end if;                                                                                                 |                                                                                                                       |
|      END;                                                                                                      |     RETURN O_RETURN;                                                                                                  |
|                                                                                                                |                                                                                                                       |
|     RETURN O_RETURN;                                                                                           |    END;                                                                                                               |
|                                                                                                                |    /                                                                                                                  |
|    END;                                                                                                        |                                                                                                                       |
|    END_PROC;                                                                                                   |                                                                                                                       |
+----------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001772696312__en-us_topic_0000001657865518_en-us_topic_0238518432_en-us_topic_0237362167_en-us_topic_0202686277_section92805525715:

Process Compilation Specification
---------------------------------

The process which is started with **Begin_PROC** and ended with **END_PROC** should be removed.

+----------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+
| Netezza Syntax                                                                                                 | Syntax After Migration                                                                                                |
+================================================================================================================+=======================================================================================================================+
| ::                                                                                                             | ::                                                                                                                    |
|                                                                                                                |                                                                                                                       |
|    CREATE OR REPLACE PROCEDURE "DWDB"."EDW"."SP_O_HXYW_LNSACCTINFO_H"(CHARACTER VARYING(8))                    |    CREATE OR REPLACE FUNCTION "EDW"."SP_O_HXYW_LNSACCTINFO_H"(CHARACTER VARYING(8))                                   |
|    RETURNS INTEGER                                                                                             |    RETURN INTEGER                                                                                                     |
|    LANGUAGE NZPLSQL AS                                                                                         |    AS                                                                                                                 |
|    BEGIN_PROC                                                                                                  |        V_PAR_DAY ALIAS for $1;                                                                                        |
|    DECLARE                                                                                                     |        V_PRCNAME    NCHAR VARYING(50):= 'SP_O_HXYW_LNSACCTINFO_H';                                                    |
|        V_PAR_DAY ALIAS for $1;                                                                                 |        V_CNT        INTEGER;                                                                                          |
|        V_PRCNAME    NVARCHAR(50):= 'SP_O_HXYW_LNSACCTINFO_H';                                                  |        V_STEP_INFO  NCHAR VARYING(500);                                                                               |
|        V_CNT        INTEGER;                                                                                   |        D_TIME_START TIMESTAMP:= CURRENT_TIMESTAMP;                                                                    |
|        V_STEP_INFO   NVARCHAR(500);                                                                            |        O_RETURN INTEGER;                                                                                              |
|        D_TIME_START TIMESTAMP:= CURRENT_TIMESTAMP;                                                             |    BEGIN                                                                                                              |
|        O_RETURN INTEGER;                                                                                       |      O_RETURN := 0;                                                                                                   |
|    BEGIN                                                                                                       |                                                                                                                       |
|      O_RETURN := 0;                                                                                            |      /* Writes logs and starts the recording process. */                                                              |
|                                                                                                                |      SP_LOG_EXEC(V_PRCNAME,V_PAR_DAY,D_TIME_START,0,0,'The process running',' ');                                     |
|      --Writes logs and starts the recording process.                                                           |                                                                                                                       |
|      CALL SP_LOG_EXEC(V_PRCNAME,V_PAR_DAY,D_TIME_START,0,0,'The process running ',' ');                        |      V_STEP_INFO := '1.Initialization';                                                                               |
|                                                                                                                |      BEGIN                                                                                                            |
|      V_STEP_INFO := '1.Initialization';                                                                        |        /* 1.1 */                                                                                                      |
|      BEGIN                                                                                                     |        SELECT COUNT(*) INTO V_CNT FROM information_schema.columns WHERE TABLE_NAME=lower('TMPO_HXYW_LNSACCTINFO_H1'); |
|        --1.1                                                                                                   |        if V_CNT>0 then                                                                                                |
|        SELECT COUNT(*) INTO V_CNT FROM information_schema.columns WHERE TABLE_NAME='TMPO_HXYW_LNSACCTINFO_H1'; |           EXECUTE IMMEDIATE 'DROP TABLE TMPO_HXYW_LNSACCTINFO_H1';                                                    |
|        if V_CNT>0 then                                                                                         |        end if;                                                                                                        |
|           EXECUTE IMMEDIATE 'DROP TABLE TMPO_HXYW_LNSACCTINFO_H1';                                             |      END;                                                                                                             |
|        end if;                                                                                                 |                                                                                                                       |
|      END;                                                                                                      |     RETURN O_RETURN;                                                                                                  |
|                                                                                                                |                                                                                                                       |
|     RETURN O_RETURN;                                                                                           |    END;                                                                                                               |
|                                                                                                                |    /                                                                                                                  |
|    END;                                                                                                        |                                                                                                                       |
|    END_PROC;                                                                                                   |                                                                                                                       |
+----------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001772696312__en-us_topic_0000001657865518_en-us_topic_0238518432_en-us_topic_0237362167_en-us_topic_0202686277_section1272133175917:

DECLARE Keyword to Declare the Local Variables
----------------------------------------------

DECLARE should be modified to AS.

+----------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+
| Netezza Syntax                                                                                                 | Syntax After Migration                                                                                                |
+================================================================================================================+=======================================================================================================================+
| ::                                                                                                             | ::                                                                                                                    |
|                                                                                                                |                                                                                                                       |
|    CREATE OR REPLACE PROCEDURE "DWDB"."EDW"."SP_O_HXYW_LNSACCTINFO_H"(CHARACTER VARYING(8))                    |    CREATE OR REPLACE FUNCTION "EDW"."SP_O_HXYW_LNSACCTINFO_H"(CHARACTER VARYING(8))                                   |
|    RETURNS INTEGER                                                                                             |    RETURN INTEGER                                                                                                     |
|    LANGUAGE NZPLSQL AS                                                                                         |    AS                                                                                                                 |
|    BEGIN_PROC                                                                                                  |        V_PAR_DAY ALIAS for $1;                                                                                        |
|    DECLARE                                                                                                     |        V_PRCNAME    NCHAR VARYING(50):= 'SP_O_HXYW_LNSACCTINFO_H';                                                    |
|        V_PAR_DAY ALIAS for $1;                                                                                 |        V_CNT        INTEGER;                                                                                          |
|        V_PRCNAME    NVARCHAR(50):= 'SP_O_HXYW_LNSACCTINFO_H';                                                  |        V_STEP_INFO  NCHAR VARYING(500);                                                                               |
|        V_CNT        INTEGER;                                                                                   |        D_TIME_START TIMESTAMP:= CURRENT_TIMESTAMP;                                                                    |
|        V_STEP_INFO   NVARCHAR(500);                                                                            |        O_RETURN INTEGER;                                                                                              |
|        D_TIME_START TIMESTAMP:= CURRENT_TIMESTAMP;                                                             |    BEGIN                                                                                                              |
|        O_RETURN INTEGER;                                                                                       |      O_RETURN := 0;                                                                                                   |
|    BEGIN                                                                                                       |                                                                                                                       |
|      O_RETURN := 0;                                                                                            |      /* Writes logs and starts the recording process. */                                                              |
|                                                                                                                |      SP_LOG_EXEC(V_PRCNAME,V_PAR_DAY,D_TIME_START,0,0,'The process running',' ');                                     |
|      --Writes logs and starts the recording process.                                                           |                                                                                                                       |
|      CALL SP_LOG_EXEC(V_PRCNAME,V_PAR_DAY,D_TIME_START,0,0,'The process running ',' ');                        |      V_STEP_INFO := '1.Initialization';                                                                               |
|                                                                                                                |      BEGIN                                                                                                            |
|      V_STEP_INFO := '1.Initialization';                                                                        |        /* 1.1 */                                                                                                      |
|      BEGIN                                                                                                     |        SELECT COUNT(*) INTO V_CNT FROM information_schema.columns WHERE TABLE_NAME=lower('TMPO_HXYW_LNSACCTINFO_H1'); |
|        --1.1                                                                                                   |        if V_CNT>0 then                                                                                                |
|        SELECT COUNT(*) INTO V_CNT FROM information_schema.columns WHERE TABLE_NAME='TMPO_HXYW_LNSACCTINFO_H1'; |           EXECUTE IMMEDIATE 'DROP TABLE TMPO_HXYW_LNSACCTINFO_H1';                                                    |
|        if V_CNT>0 then                                                                                         |        end if;                                                                                                        |
|           EXECUTE IMMEDIATE 'DROP TABLE TMPO_HXYW_LNSACCTINFO_H1';                                             |      END;                                                                                                             |
|        end if;                                                                                                 |                                                                                                                       |
|      END;                                                                                                      |     RETURN O_RETURN;                                                                                                  |
|                                                                                                                |                                                                                                                       |
|     RETURN O_RETURN;                                                                                           |    END;                                                                                                               |
|                                                                                                                |    /                                                                                                                  |
|    END;                                                                                                        |                                                                                                                       |
|    END_PROC;                                                                                                   |                                                                                                                       |
+----------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+
