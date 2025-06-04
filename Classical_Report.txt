
*&---------------------------------------------------------------------*
*& Report ZREPORT_SINGLE_TABLE
*&---------------------------------------------------------------------*
REPORT zreport_single_table.
TABLES: vbak.

*
*INCLUDE ZREPORT_SINGLE_TABLE_TOP.
*INCLUDE ZREPORT_SINGLE_TABLE_SCR.
*INCLUDE ZREPORT_SINGLE_TABLE_F01.

*-------------------------------------------STRUCTURE---------------------------------------

TYPES: BEGIN OF ty_vbak,
         vbeln TYPE vbak-vbeln,
         erdat TYPE vbak-erdat,
         ernam TYPE vbak-ernam,
         audat TYPE vbak-audat,
         auart TYPE vbak-auart,
         WAERK TYPE vbak-WAERK,
       END OF ty_vbak.

*----------------------------------INTERNAL_TABLE_AND_WORK_AREA--------------------------------

DATA: it_vbak TYPE TABLE OF ty_vbak,
      wa_vbak TYPE ty_vbak,

      v_repid TYPE sy-repid, "Program name
      v_date  TYPE sy-datum, "Current date
      v_user  TYPE sy-uname. "User name

*----------------------------------INTERNAL_TABLE_AND_WORK_AREA--------------------------------

*----------------------------SELECTION_SCREEN---------------------------------------------------

SELECTION-SCREEN BEGIN OF BLOCK b1 WITH FRAME TITLE text-001.
SELECT-OPTIONS: s_vbeln FOR vbak-vbeln.

SELECT-OPTIONS: s_WAERK FOR vbak-WAERK No INTERVALS MODIF ID WAE.
PARAMETERS: p_Chk TYPE C AS CHECKBOX USER-COMMAND SELECT.
SELECTION-SCREEN END OF BLOCK b1.
*----------------------------SELECTION_SCREEN---------------------------------------------------


***********************EVENT_AT SELECTION-SCREEN OUTPUT**********************************************
AT SELECTION-SCREEN OUTPUT.
  LOOP AT SCREEN.
    IF screen-group1 = 'WAE'. " group1 name in MODIF ID
      IF p_chk = 'X'.  " If checkbox is selected, disable
        screen-active = 0.
      ELSE.            " If checkbox is unselected, enable
        screen-active = 1.
      ENDIF.
      MODIFY SCREEN.
    ENDIF.
  ENDLOOP.
***********************EVENT_AT SELECTION-SCREEN OUTPUT**********************************************


**********************************************************************

*
*-------------------------------------------------*
* AT SELECTION-SCREEN - Validate input
*-------------------------------------------------*
AT SELECTION-SCREEN.
*
*    IF s_vbeln IS INITIAL.
*    MESSAGE 'Please enter a Sales Document Number.' TYPE 'E'.
*  ENDIF.



*-------------------------------------------------*
* INITIALIZATION - Set default values
*-------------------------------------------------*
INITIALIZATION.
  v_repid = sy-repid.
  v_date  = sy-datum.
  v_user  = sy-uname.

*-------------------------------------------------*
* START-OF-SELECTION - Data fetching logic
*-------------------------------------------------*
START-OF-SELECTION.


SELECT vbeln
         erdat
         ernam
         audat
         auart
         WAERK
    FROM vbak
    INTO TABLE it_vbak
    WHERE vbeln IN S_vbeln.

*    UPTO 20 ROWS.

  IF sy-subrc <> 0.
    MESSAGE 'No records found for the given Sales Document Number.' TYPE 'I'.
  ENDIF.

  LOOP AT it_vbak INTO wa_vbak.
    WRITE: / wa_vbak-vbeln,
            20 wa_vbak-erdat,
             55 wa_vbak-WAERK,
             95 wa_vbak-ernam,
             140 wa_vbak-audat,
             170 wa_vbak-auart.
  ENDLOOP.

*-------------------------------------------------*
* END-OF-SELECTION - Footer line
*-------------------------------------------------*
END-OF-SELECTION.
  ULINE.
  WRITE: /70 text-005.

*-------------------------------------------------*
* TOP-OF-PAGE - Header content
*-------------------------------------------------*
TOP-OF-PAGE.
  WRITE: / 'Material Details',
         / 'Date: ',   12 v_date DD/MM/YYYY,
         / 'User: ',   12 v_user,
         / 'Report: ', 12 v_repid.
  ULINE.
  WRITE: / text-000, 20 text-001 ,55 'TEST', 95 text-002, 140 text-003, 165 text-004.
  ULINE.

*-------------------------------------------------*
* END-OF-PAGE - Optional footer
*-------------------------------------------------*
END-OF-PAGE.
