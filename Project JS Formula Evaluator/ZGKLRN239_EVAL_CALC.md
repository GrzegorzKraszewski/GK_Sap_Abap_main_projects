

<h2>This program shows the way how you can generate formula/calculations using javascript evaluator class and macros in SAP ABAP </h2><br>

<h3>Key factors</h3><br>
<p>This tool provides two main options => you can check the  calculations/formula using some of the js notation and  it gives possibilites to manage the  calculations/formula ( save, edit, delete in/from Database )  </p>
<p>We aren't declairng variables in editor as let, var etc.</p>
<p>Posible variables to use are embedded in var listbox => there are used macros for it and we creating a list with accessible variables to use in program</p>
<p>Program present possible useful js suoported operators methods etc.</p>
<h3>The final look of the program:</h3><br>

![Final_look](https://github.com/user-attachments/assets/3ae086db-db1d-4e98-8fce-2c59361eada0)

<br>
<h6 id="usageb"></h6>
<h3> Too see usage/behavior of program please click <a href="#usage">here</a></h3><br><br>


<h3>ABAP Source code:</h3> <br><br><br>
*&---------------------------------------------------------------------*<br>
*& Report ZGKLRN239_EVAL_CALC<br>
*&---------------------------------------------------------------------*<br>
*& <br>
*&---------------------------------------------------------------------*<br>
REPORT zgklrn239_eval_calc MESSAGE-ID zlrn239_msg_eval_cal.<br>
<br>

<h6 id="top1b"></h6>
<a href="#top1">INCLUDE zgklrn239_eval_calc_top_1</a>

<h6 id="d011b"></h6>
<a href="#d01">INCLUDE zgklrn239_eval_calc_d01_1</a>

<h6 id="i011b"></h6>
<a href="#i01">INCLUDE zgklrn239_eval_calc_i01_1</a>

<h6 id="sfs11b"></h6>
<a href="#sfs1">INCLUDE zgklrn239_eval_calc_sfs1_1</a>

<h6 id="pbo11b"></h6>
<a href="#pbo1">INCLUDE zgklrn239_eval_calc_pbo1_1</a>

<h6 id="pbo21b"></h6>
<a href="#pbo2">INCLUDE zgklrn239_eval_calc_pbo2_1</a>

<h6 id="pai11b"></h6>
<a href="#pai1">INCLUDE zgklrn239_eval_calc_pai1_1</a>



<p>*&---------------------------------------------------------------------*</p><h5 id="top1"> <a href="#top1b">INCLUDE zgklrn239_eval_calc_top_1</a></h5>

*&---------------------------------------------------------------------*  


```console

TYPE-POOLS : vrm.

TYPES: BEGIN OF ty_main,
         formula    TYPE string,
         matnr      TYPE mara-matnr,
         laeng      TYPE mara-laeng,
         breit      TYPE mara-breit,
         hoehe      TYPE mara-hoehe,
         result_out TYPE char11,
       END OF   ty_main.

TYPES: BEGIN OF ty_read_val,
         key  TYPE char40,
         text TYPE char80,
       END OF ty_read_val.

TYPES: BEGIN OF ty_text_set2,
         text TYPE string,
       END OF  ty_text_set2.


TYPES: BEGIN OF ty_text_from_edit,
         line TYPE i,
         text TYPE char200,
       END OF ty_text_from_edit.


DATA : lv_zmienne_id TYPE vrm_id,
       lv_values        TYPE vrm_values,
       lv_value         LIKE LINE OF lv_values,

       lv_fx_id      TYPE vrm_id,
       lv_values2    TYPE vrm_values,
       lv_value2     LIKE LINE OF lv_values,

       lv_operators_id   TYPE vrm_id,
       lv_values3       TYPE vrm_values,
       lv_value3        LIKE LINE OF lv_values,


       ls_main       TYPE ty_main,
       ls_save       TYPE zformula_tab,
       ls_delete     TYPE zformula_tab,
       ls_exist      TYPE zformula_tab,

       lv_flag_exist TYPE char1,


      ok_code     TYPE sy-ucomm,
      lr_container TYPE REF TO cl_gui_custom_container,
      lr_editor    TYPE REF TO cl_gui_textedit,

      matnr_in   TYPE mara-matnr,
      len_in     TYPE mara-laeng,
      wid_in     TYPE mara-breit,
      hei_in     TYPE mara-hoehe,
      result_out TYPE char200,
      formula_name TYPE char10,


      lv_from_line     TYPE i,
      lv_from_pos      TYPE i,
      lv_to_line       TYPE i,
      lv_to_pos        TYPE i,
      lv_string_lengh  TYPE i,
      lv_string_sub    TYPE i,
      lv_string_part1  TYPE string,
      lv_string_part2  TYPE string,
      lv_str_main_part TYPE string,
      lv_real_position TYPE i,


      lv_str_main  TYPE string,
      lv_str       TYPE string,
      lt_split_tab TYPE TABLE OF string,

      ls_read_values TYPE ty_read_val,

      lt_t_text_set2 TYPE TABLE OF ty_text_set2,
      ls_t_text_set2 TYPE ty_text_set2,


      lt_t_text             TYPE TABLE OF char200,
      ls_t_text              TYPE char200,
      lt_t_text2             TYPE STANDARD TABLE OF ty_text_from_edit,
      lv_string_formula      TYPE string,
      lv_string_formula_main TYPE string,


      lv_pierwsz TYPE char1,
      gv_flag(1),

      gv_reminder TYPE i,
      gv_evenodd TYPE i,

      lv_spac  TYPE char1 VALUE '#',
      lv_spaces  TYPE string,
      lv_line_edit_len  TYPE i,
      lv_line_edit_sub  TYPE i.


            

```


<p>*&---------------------------------------------------------------------*</p><h5 id="d01"><a href="#d011b">INCLUDE zgklrn239_eval_calc_d01_1</a></h5>
<!--  <a href="#d011b">Back to main includes</a> <br> -->
*&---------------------------------------------------------------------* <br><br> <br><br> 

```console

CLASS lcl_main DEFINITION.

  PUBLIC SECTION.

    CLASS-METHODS:
      screen_100,
      pbo,
      pai,
      create_containers_txt,
      fill_tab_text,
      fill_structure,
      save_entrie,
      delete_entrie,
      clear_entrie,
      clear_result,
      pop_up_save,
      pop_up_save_exist,
      pop_up_delete,
      fill_struct_exist,
      vrm_variables,
      get_matnr_data,
      macros_variables,
      evaluator,
      set_variables,
      set_functions,
      set_operators,
      set_display_text,
      set_change_text,
      check_exist,
      check_set_exist.


ENDCLASS.




```


<p>*&---------------------------------------------------------------------*</p><h5 id="i01"><a href="#i011b">INCLUDE zgklrn239_eval_calc_i01_1</a></h5>
 <!--  <a href="#d011b">Back to main includes</a> <br> -->
*&---------------------------------------------------------------------* <br><br> <br><br> 

```console

CLASS lcl_main IMPLEMENTATION.

  METHOD screen_100.
    CALL SCREEN 100.
  ENDMETHOD.

  METHOD pbo.

    lcl_main=>create_containers_txt( ).

    lcl_main=>set_display_text( ).


  ENDMETHOD.

  METHOD pai.


    CASE ok_code.
      WHEN 'BACK'.

        LEAVE TO SCREEN 0.
      WHEN 'BUT_4'.

        LEAVE TO SCREEN 0.
      WHEN 'BUT_8'.
        lcl_main=>get_matnr_data( ).
    ENDCASE.




    lcl_main=>fill_structure( ).
    lcl_main=>fill_tab_text( ).

    CASE ok_code.
      WHEN 'BUT_3'.
        lcl_main=>clear_entrie( ).
        CLEAR ok_code.
      WHEN 'BUT_1'.
        lcl_main=>pop_up_save( ).
        CLEAR ok_code.
      WHEN 'BUT_5'.
        lcl_main=>evaluator( ).
        CLEAR ok_code.
      WHEN 'BUT_6'. " variables
        lcl_main=>set_variables( ).
        CLEAR ok_code.
      WHEN 'BUT_7'. " fx
        lcl_main=>set_functions( ).
        CLEAR ok_code.
      WHEN 'EDIT'.
        lcl_main=>set_change_text( ).
      WHEN 'BUT_10'.
        lcl_main=>clear_entrie( ).
      WHEN 'BUT_11'.
        lcl_main=>clear_result( ).
       WHEN 'INS_OP'.
       lcl_main=>set_operators( ).
       WHEN 'GET_FO'.
         lcl_main=>check_set_exist( ).
       WHEN 'DEL'.
         lcl_main=>pop_up_delete( ).

    ENDCASE.



  ENDMETHOD.


  METHOD create_containers_txt.

    IF lr_editor IS INITIAL.


      lr_container = NEW #( container_name = 'G_CUSTOM' ).


      lr_editor = NEW #(
           wordwrap_mode     = 2
           wordwrap_position = 90
           parent            = lr_container ).


    ENDIF.


  ENDMETHOD.

  METHOD fill_tab_text.


    CLEAR:  lv_str_main,
            lv_str,
            lt_split_tab,
            lt_t_text.




    lr_editor->get_text_as_r3table(
      IMPORTING
        table           = lt_t_text
      EXCEPTIONS
        error_dp        = 1
        error_dp_create = 2
        OTHERS          = 3 ).



    IF lt_t_text IS NOT INITIAL.

      LOOP AT lt_t_text INTO DATA(ls_s_text).
        CLEAR lv_str.
        IF ls_s_text IS NOT INITIAL.

          lv_line_edit_len = strlen( ls_s_text ).
          lv_line_edit_sub = 90 - lv_line_edit_len.

          CLEAR lv_spaces.

          DO lv_line_edit_sub TIMES.

            CONCATENATE lv_spaces ` ` INTO lv_spaces.
          ENDDO.


          lv_str = ls_s_text.
          CONCATENATE lv_str_main lv_str lv_spaces INTO lv_str_main RESPECTING BLANKS.

        ENDIF.


        CLEAR ls_s_text.
      ENDLOOP.

      SHIFT lv_str_main LEFT DELETING LEADING space.

      CLEAR ls_s_text.


    ENDIF.

    IF  lv_str_main IS NOT INITIAL.

      CLEAR ls_main-formula.
      ls_main-formula = lv_str_main.
    ENDIF.


  ENDMETHOD.

  METHOD fill_structure.

    ls_main-matnr   = matnr_in.

  ENDMETHOD.


  METHOD save_entrie.

    CLEAR ls_save.

    lcl_main=>fill_tab_text( ).

    IF ls_main IS NOT INITIAL.

    IF formula_name IS NOT INITIAL.

      CLEAR ls_save.
      ls_save-formula_name = formula_name.
      ls_save-formula = ls_main-formula.

      IF ls_save IS NOT INITIAL.

       MODIFY zformula_tab FROM ls_save.

        IF sy-subrc EQ 0.
          MESSAGE i001(zlrn239_msg_eval_cal).
        ELSE.
          MESSAGE i002(zlrn239_msg_eval_cal).
        ENDIF.
      ENDIF.

    ELSE.

      MESSAGE i003(zlrn239_msg_eval_cal) DISPLAY LIKE 'E'.
    ENDIF.

ENDIF.

  ENDMETHOD.


  METHOD clear_entrie.

    CLEAR:




      ls_main,
      lv_flag_exist.


    REFRESH lt_t_text.

    lr_editor->delete_text( ).


  ENDMETHOD.

  METHOD pop_up_save.

    DATA lv_popup_return_2(1).

    CALL FUNCTION 'POPUP_TO_CONFIRM'
      EXPORTING
        titlebar              =  text-102 "Confirmation
        text_question         =  text-103 "Do you want to save ?
        text_button_1         =  text-100 "Yes
        text_button_2         =  text-101 "No
        default_button        = '2'
        display_cancel_button = 'X'
      IMPORTING
        answer                = lv_popup_return_2
      EXCEPTIONS
        text_not_found        = 1
        OTHERS                = 2.
    IF sy-subrc <> 0.
      MESSAGE ID sy-msgid TYPE sy-msgty NUMBER sy-msgno
      WITH sy-msgv1 sy-msgv2 sy-msgv3 sy-msgv4.
    ENDIF.
    CASE lv_popup_return_2.
      WHEN '1'.

        lcl_main=>check_exist( ).

        IF ls_exist IS INITIAL.

        lcl_main=>save_entrie( ).

        ELSE.

        lcl_main=>pop_up_save_exist( ).

        ENDIF.

      WHEN '2'.

      WHEN 'A'.

    ENDCASE.



  ENDMETHOD.

  METHOD pop_up_save_exist.

    DATA lv_popup_return_3(1).

    CALL FUNCTION 'POPUP_TO_CONFIRM'
      EXPORTING
        titlebar              =  text-102 "Confirmation
        text_question         =  text-104 "Entry exists , overwrite ?
        text_button_1         =  text-100 "Yes
        text_button_2         =  text-101 "No
        default_button        = '2'
        display_cancel_button = 'X'
      IMPORTING
        answer                = lv_popup_return_3
      EXCEPTIONS
        text_not_found        = 1
        OTHERS                = 2.
    IF sy-subrc <> 0.
      MESSAGE ID sy-msgid TYPE sy-msgty NUMBER sy-msgno
      WITH sy-msgv1 sy-msgv2 sy-msgv3 sy-msgv4.
    ENDIF.
    CASE lv_popup_return_3.
      WHEN '1'.

        lcl_main=>save_entrie( ).

      WHEN '2'.

      WHEN 'A'.

    ENDCASE.



  ENDMETHOD.


  METHOD pop_up_delete.

    DATA lv_popup_return_3(1).

    CALL FUNCTION 'POPUP_TO_CONFIRM'
      EXPORTING
        titlebar              = text-102 "Confirmation
        text_question         = text-105 "Are you sure you want to delete?
        text_button_1         = text-100 "Yes
        text_button_2         = text-101 "No
        default_button        = '2'
        display_cancel_button = 'X'
      IMPORTING
        answer                = lv_popup_return_3
      EXCEPTIONS
        text_not_found        = 1
        OTHERS                = 2.
    IF sy-subrc <> 0.
      MESSAGE ID sy-msgid TYPE sy-msgty NUMBER sy-msgno
      WITH sy-msgv1 sy-msgv2 sy-msgv3 sy-msgv4.
    ENDIF.
    CASE lv_popup_return_3.
      WHEN '1'.


        lcl_main=>check_exist( ).

        IF ls_exist IS NOT INITIAL.

        lcl_main=>delete_entrie( ).

        MESSAGE i007(zlrn239_msg_eval_cal).

        ELSE.

        ENDIF.

      WHEN '2'.

      WHEN 'A'.

    ENDCASE.

  ENDMETHOD.



  METHOD fill_struct_exist.


    TYPES: BEGIN OF ty_text_set,
             text TYPE char200,
           END OF  ty_text_set.

    DATA: lt_t_text_set TYPE TABLE OF ty_text_set,
          ls_t_text_set TYPE ty_text_set.

    CLEAR: lt_t_text_set,
        ls_t_text_set.



  ENDMETHOD.



  METHOD vrm_variables.

    CLEAR: lv_value,
           lv_values,
           lv_value2,
           lv_values2,
           lv_value3,
           lv_values3.



    lv_values = VALUE #(
                          ( key = 1 text = `LEN_EL` )
                          ( key = 2 text = `WID_EL` )
                          ( key = 3 text = `HEI_EL` )

                          ).



    CALL FUNCTION 'VRM_SET_VALUES'
      EXPORTING
        id              = 'LV_ZMIENNE_ID'
        values          = lv_values
      EXCEPTIONS
        id_illegal_name = 1
        OTHERS          = 2.


     lv_values2 = VALUE #(
                          ( key = 1 text = `eval( )` )
                          ( key = 2 text = `eval("if ( condition ) { true; }")` )
                          ( key = 3 text = `eval("if ( condition ) { true; } else { false; }")` )
                          ( key = 4 text = `eval("if ( condition ) { true; } else if ( condition2 ) { true; } else { ; } ")` )
                          ( key = 5 text = `eval("if ( cond1 && cond2 ) { true; } else { false; }")` )
                          ( key = 6 text = `eval("if ( cond1 || cond2 ) { true; } else { false; }")` )
                          ( key = 7 text = `if ( condition ) { true; } else { false; }` )
                          ( key = 8 text = `Math.min( , n..)`)
                          ( key = 9 text = `Math.max( , n..)`)
                          ( key = 10 text = `Math.pow( , )`)
                          ( key = 11 text = `Math.round( )`)
                          ( key = 12 text = `Math.ceil( )`)
                          ( key = 13 text = `Math.floor( )`)
                          ( key = 14 text = `Math.abs(- )`)
                          ( key = 15 text = `Math.sqrt( )`)
                          ( key = 16 text = `Math.random( )`)
                          ( key = 17 text = `Math.PI`)
                          ( key = 18 text = `parseFloat(())`)
                          ( key = 19 text = `new Date()`)
                          ( key = 19 text = `new Date("MonthName Day, Year Hour:Minute:Second")`)
                          ( key = 20 text = `new Date(year, month, day, hour, minute, second,millisecond)`)
                          ( key = 21 text = `new Date().getFullYear()`)
                          ( key = 22 text = `new Date().getMonth()`)
                          ( key = 23 text = `new Date().getDate()`)
                          ( key = 24 text = `new Date().getDay()`)
                          ( key = 25 text = `new Date().getHours()`)
                          ( key = 26 text = `new Date().getMinutes()`)
                          ( key = 27 text = `new Date().getSeconds()`)
                          ( key = 28 text = `new Date().getMilliseconds()`)
                          ( key = 29 text = `new Date().getTime()`)

                          ).





    CALL FUNCTION 'VRM_SET_VALUES'
      EXPORTING
        id              = 'LV_FX_ID'
        values          = lv_values2
      EXCEPTIONS
        id_illegal_name = 1
        OTHERS          = 2.




    lv_values3 = VALUE #(
                          ( key = 1 text = `+` )
                          ( key = 2 text = `-` )
                          ( key = 3 text = `*` )
                          ( key = 4 text = `/` )
                          ( key = 5 text = `==` )
                          ( key = 6 text = `!=` )
                          ( key = 7 text = `%` )
                          ( key = 8 text = `>` )
                          ( key = 9 text = `<` )
                          ( key = 10 text = `>=` )
                          ( key = 11 text = `<=` )
                          ( key = 12 text = `&&` )
                          ( key = 13 text = `||` )
                          ( key = 14 text = `!` )
                          ( key = 15 text = `()` )
                          ( key = 16 text = `(` )
                          ( key = 17 text = `)` )

                          ).



    CALL FUNCTION 'VRM_SET_VALUES'
      EXPORTING
        id              = 'LV_OPERATORS_ID'
        values          = lv_values3
      EXCEPTIONS
        id_illegal_name = 1
        OTHERS          = 2.




  ENDMETHOD.


  METHOD get_matnr_data.

    CLEAR: ls_main-laeng,
           ls_main-breit,
           ls_main-hoehe.



    IF  matnr_in IS NOT INITIAL.

      SELECT SINGLE laeng ,breit ,hoehe
               FROM mara
               INTO CORRESPONDING FIELDS OF @ls_main
      WHERE matnr = @matnr_in.


      len_in = ls_main-laeng.
      wid_in = ls_main-breit.
      hei_in = ls_main-hoehe.

    ENDIF.



  ENDMETHOD.




  METHOD set_variables.

*** concatanating text in editor depending on setting variable from "variables"

    CLEAR:
    lv_from_line, "
    lv_from_pos,  "
    lv_to_line,   "
    lv_to_pos,    "
    lv_string_lengh,
    lv_string_sub,
    lv_string_part1,
    lv_string_part2,
    ls_read_values,

    ls_t_text_set2,
    lt_t_text_set2,

    lt_t_text,
    ls_t_text,
    lt_t_text2,
    lv_string_formula,
    lv_string_formula_main.

    lr_editor->get_text_as_r3table(
     IMPORTING
       table           = lt_t_text
     EXCEPTIONS
       error_dp        = 1
       error_dp_create = 2
       OTHERS          = 3 ).



    lr_editor->get_selection_pos(
      IMPORTING
        from_line              = lv_from_line
        from_pos               = lv_from_pos
        to_line                = lv_to_line
        to_pos                 = lv_to_pos
      EXCEPTIONS
        error_cntl_call_method = 1
        OTHERS                 = 2 ).


    IF lv_from_line IS NOT INITIAL.
      READ TABLE lt_t_text INDEX lv_from_line INTO ls_t_text.

      IF ls_t_text IS NOT INITIAL.

        IF lv_zmienne_id IS NOT INITIAL.

          READ TABLE lv_values INTO ls_read_values WITH KEY key = lv_zmienne_id.

          TRY.

              lv_real_position = lv_from_pos - 1.

              lv_string_lengh = strlen( ls_t_text ).
              lv_string_sub = lv_string_lengh - lv_real_position.


              IF lv_real_position = 0.

                CONCATENATE  ls_read_values-text  ls_t_text  INTO ls_t_text_set2-text.

                CLEAR ls_t_text_set2-text.

                ls_t_text_set2-text = |{ ls_read_values-text  }| & |{ ls_t_text }|.

              ELSEIF lv_real_position > lv_string_lengh.
                lv_string_part1 = ls_t_text(lv_string_lengh).
                lv_string_part2 = ''.


                ls_t_text_set2-text = |{ lv_string_part1 }| & |{ ls_read_values-text }| & |{ lv_string_part2 }|.


              ELSEIF lv_real_position < lv_string_lengh.

                lv_string_part1 = ls_t_text(lv_real_position).
                lv_string_part2 = ls_t_text+lv_real_position(lv_string_sub).


                ls_t_text_set2-text = |{ lv_string_part1  }| & |{ ls_read_values-text }| & |{ lv_string_part2 }|.


              ELSEIF lv_string_lengh = lv_real_position.

                ls_t_text_set2-text = |{ ls_t_text  }| & |{ ls_read_values-text }|.

              ENDIF.


            CATCH cx_root INTO DATA(e_text).
              MESSAGE e_text->get_text( ) TYPE 'I'.
          ENDTRY.



          IF ls_t_text_set2-text IS NOT INITIAL.
            MODIFY lt_t_text FROM ls_t_text_set2-text INDEX lv_from_line.

            lr_editor->delete_text(
               EXCEPTIONS
                 error_cntl_call_method = 1
                 OTHERS                 = 2 ).



            lr_editor->set_selected_text_as_r3table(
              EXPORTING
                table           = lt_t_text

              EXCEPTIONS
                error_dp        = 1
                error_dp_create = 2
                OTHERS          = 3 ).


            CLEAR ls_t_text.

            LOOP AT lt_t_text INTO ls_t_text.
              IF ls_t_text IS NOT INITIAL.
                CLEAR lv_string_formula.
                lv_string_formula = ls_t_text.

                lv_string_formula_main = | { lv_string_formula_main  } | & | { lv_string_formula  } |.

              ENDIF.
              CLEAR ls_t_text.
            ENDLOOP.

            IF  lv_string_formula_main IS NOT INITIAL.
              CLEAR ls_main-formula.
              ls_main-formula = lv_string_formula_main.
            ENDIF.


          ENDIF.


        ENDIF.


      ELSE.


        IF lv_zmienne_id IS NOT INITIAL.

          READ TABLE lv_values INTO ls_read_values WITH KEY key = lv_zmienne_id.

        ENDIF.

        IF ls_read_values-text IS NOT INITIAL.
          ls_t_text_set2-text = ls_read_values-text.

          APPEND ls_t_text_set2-text TO lt_t_text.


          lr_editor->delete_text(
            EXCEPTIONS
              error_cntl_call_method = 1
              OTHERS                 = 2 ).

          lr_editor->set_selected_text_as_r3table(
            EXPORTING
              table           = lt_t_text

            EXCEPTIONS
              error_dp        = 1
              error_dp_create = 2
              OTHERS          = 3 ).


          CLEAR ls_t_text.

          LOOP AT lt_t_text INTO ls_t_text.
            IF ls_t_text IS NOT INITIAL.
              CLEAR lv_string_formula.
              lv_string_formula = ls_t_text.

              lv_string_formula_main = |{ lv_string_formula_main }| & |{ lv_string_formula }|.

            ENDIF.
            CLEAR ls_t_text.
          ENDLOOP.

          IF  lv_string_formula_main IS NOT INITIAL.
            CLEAR ls_main-formula.
            ls_main-formula = lv_string_formula_main.
          ENDIF.

        ENDIF.

      ENDIF.

    ENDIF.

  ENDMETHOD.

  METHOD set_functions.

*** concatanating text in editor depending on setting formula from "fx"

    CLEAR:
  lv_from_line, "
  lv_from_pos,  "
  lv_to_line,   "
  lv_to_pos,    "
  lv_string_lengh,
  lv_string_sub,
  lv_string_part1,
  lv_string_part2,
  ls_read_values,

  ls_t_text_set2,
  lt_t_text_set2,

  lt_t_text,
  ls_t_text,
  lt_t_text2,
  lv_string_formula,
  lv_string_formula_main.


    lr_editor->get_text_as_r3table(
     IMPORTING
       table           = lt_t_text
     EXCEPTIONS
       error_dp        = 1
       error_dp_create = 2
       OTHERS          = 3 ).


    lr_editor->get_selection_pos(
     IMPORTING
       from_line              = lv_from_line
       from_pos               = lv_from_pos
       to_line                = lv_to_line
       to_pos                 = lv_to_pos
     EXCEPTIONS
       error_cntl_call_method = 1
       OTHERS                 = 2 ).


    IF lv_from_line IS NOT INITIAL.
      READ TABLE lt_t_text INDEX lv_from_line INTO ls_t_text.

      IF ls_t_text IS NOT INITIAL.

        IF lv_fx_id IS NOT INITIAL.

          READ TABLE lv_values2 INTO ls_read_values WITH KEY key = lv_fx_id.

          TRY.

              lv_real_position = lv_from_pos - 1.

              lv_string_lengh = strlen( ls_t_text ).
              lv_string_sub = lv_string_lengh - lv_real_position.


              IF lv_real_position = 0.

                ls_t_text_set2-text = |{ ls_read_values-text }| & |{ ls_t_text }|.

              ELSEIF lv_real_position > lv_string_lengh.
                lv_string_part1 = ls_t_text(lv_string_lengh).
                lv_string_part2 = ''.

                ls_t_text_set2-text = |{ lv_string_part1 }| & |{ ls_read_values-text }| & |{ lv_string_part2 }|.

              ELSEIF lv_real_position < lv_string_lengh.

                lv_string_part1 = ls_t_text(lv_real_position).
                lv_string_part2 = ls_t_text+lv_real_position(lv_string_sub).

                ls_t_text_set2-text = |{ lv_string_part1 }| & |{ ls_read_values-text }| & |{ lv_string_part2 }|.


              ELSEIF lv_string_lengh = lv_real_position.


                ls_t_text_set2-text = |{ ls_t_text }| & |{ ls_read_values-text }|.


              ENDIF.


            CATCH cx_root INTO DATA(e_text).
              MESSAGE e_text->get_text( ) TYPE 'I'.
          ENDTRY.



          IF ls_t_text_set2-text IS NOT INITIAL.
            MODIFY lt_t_text FROM ls_t_text_set2-text INDEX lv_from_line.


            lr_editor->delete_text(
             EXCEPTIONS
               error_cntl_call_method = 1
               OTHERS                 = 2 ).


            lr_editor->delete_text(
           EXCEPTIONS
             error_cntl_call_method = 1
             OTHERS                 = 2 ).



            lr_editor->set_selected_text_as_r3table(
               EXPORTING
                 table           = lt_t_text
               EXCEPTIONS
                 error_dp        = 1
                 error_dp_create = 2
                 OTHERS          = 3 ).



            CLEAR ls_t_text.

            LOOP AT lt_t_text INTO ls_t_text.
              IF ls_t_text IS NOT INITIAL.
                CLEAR lv_string_formula.
                lv_string_formula = ls_t_text.

                lv_string_formula_main = |{ lv_string_formula_main }| & |{ lv_string_formula }|.

              ENDIF.
              CLEAR ls_t_text.
            ENDLOOP.

            IF  lv_string_formula_main IS NOT INITIAL.
              CLEAR ls_main-formula.
              ls_main-formula = lv_string_formula_main.
            ENDIF.

          ENDIF.

        ENDIF.


      ELSE.

        IF lv_fx_id IS NOT INITIAL.

          READ TABLE lv_values2 INTO ls_read_values WITH KEY key = lv_fx_id.

        ENDIF.

        IF ls_read_values-text IS NOT INITIAL.
          ls_t_text_set2-text = ls_read_values-text.

          APPEND ls_t_text_set2-text TO lt_t_text.

          lr_editor->delete_text(
            EXCEPTIONS
              error_cntl_call_method = 1
              OTHERS                 = 2 ).


          lr_editor->set_selected_text_as_r3table(
            EXPORTING
              table           = lt_t_text

            EXCEPTIONS
              error_dp        = 1
              error_dp_create = 2
              OTHERS          = 3 ).


          CLEAR ls_t_text.

          LOOP AT lt_t_text INTO ls_t_text.
            IF ls_t_text IS NOT INITIAL.
              CLEAR lv_string_formula.
              lv_string_formula = ls_t_text.

              lv_string_formula_main = |{ lv_string_formula_main }| & |{ lv_string_formula }|.

            ENDIF.
            CLEAR ls_t_text.
          ENDLOOP.

          IF  lv_string_formula_main IS NOT INITIAL.
            CLEAR ls_main-formula.
            ls_main-formula = lv_string_formula_main.
          ENDIF.

        ENDIF.

      ENDIF.

    ENDIF.

  ENDMETHOD.


 METHOD set_operators.

*** concatanating text in editor depending on setting formula from "operators"

    CLEAR:
  lv_from_line, "
  lv_from_pos,  "
  lv_to_line,   "
  lv_to_pos,    "
  lv_string_lengh,
  lv_string_sub,
  lv_string_part1,
  lv_string_part2,
  ls_read_values,

  ls_t_text_set2,
  lt_t_text_set2,

  lt_t_text,
  ls_t_text,
  lt_t_text2,
  lv_string_formula,
  lv_string_formula_main.


    lr_editor->get_text_as_r3table(
     IMPORTING
       table           = lt_t_text
     EXCEPTIONS
       error_dp        = 1
       error_dp_create = 2
       OTHERS          = 3 ).


    lr_editor->get_selection_pos(
     IMPORTING
       from_line              = lv_from_line
       from_pos               = lv_from_pos
       to_line                = lv_to_line
       to_pos                 = lv_to_pos
     EXCEPTIONS
       error_cntl_call_method = 1
       OTHERS                 = 2 ).


    IF lv_from_line IS NOT INITIAL.
      READ TABLE lt_t_text INDEX lv_from_line INTO ls_t_text.

      IF ls_t_text IS NOT INITIAL.

        IF lv_operators_id IS NOT INITIAL.

          READ TABLE lv_values3 INTO ls_read_values WITH KEY key = lv_operators_id.

          TRY.

              lv_real_position = lv_from_pos - 1.

              lv_string_lengh = strlen( ls_t_text ).
              lv_string_sub = lv_string_lengh - lv_real_position.


              IF lv_real_position = 0.

                ls_t_text_set2-text = |{ ls_read_values-text }| & |{ ls_t_text }|.

              ELSEIF lv_real_position > lv_string_lengh.
                lv_string_part1 = ls_t_text(lv_string_lengh).
                lv_string_part2 = ''.

                ls_t_text_set2-text = |{ lv_string_part1 }| & |{ ls_read_values-text }| & |{ lv_string_part2 }|.

              ELSEIF lv_real_position < lv_string_lengh.

                lv_string_part1 = ls_t_text(lv_real_position).
                lv_string_part2 = ls_t_text+lv_real_position(lv_string_sub).

                ls_t_text_set2-text = |{ lv_string_part1 }| & |{ ls_read_values-text }| & |{ lv_string_part2 }|.


              ELSEIF lv_string_lengh = lv_real_position.


                ls_t_text_set2-text = |{ ls_t_text }| & |{ ls_read_values-text }|.


              ENDIF.


            CATCH cx_root INTO DATA(e_text).
              MESSAGE e_text->get_text( ) TYPE 'I'.
          ENDTRY.



          IF ls_t_text_set2-text IS NOT INITIAL.
            MODIFY lt_t_text FROM ls_t_text_set2-text INDEX lv_from_line.


            lr_editor->delete_text(
             EXCEPTIONS
               error_cntl_call_method = 1
               OTHERS                 = 2 ).


            lr_editor->delete_text(
           EXCEPTIONS
             error_cntl_call_method = 1
             OTHERS                 = 2 ).



            lr_editor->set_selected_text_as_r3table(
               EXPORTING
                 table           = lt_t_text
               EXCEPTIONS
                 error_dp        = 1
                 error_dp_create = 2
                 OTHERS          = 3 ).



            CLEAR ls_t_text.

            LOOP AT lt_t_text INTO ls_t_text.
              IF ls_t_text IS NOT INITIAL.
                CLEAR lv_string_formula.
                lv_string_formula = ls_t_text.

                lv_string_formula_main = |{ lv_string_formula_main }| & |{ lv_string_formula }|.

              ENDIF.
              CLEAR ls_t_text.
            ENDLOOP.

            IF  lv_string_formula_main IS NOT INITIAL.
              CLEAR ls_main-formula.
              ls_main-formula = lv_string_formula_main.
            ENDIF.


          ENDIF.


        ENDIF.


      ELSE.


        IF lv_operators_id IS NOT INITIAL.

          READ TABLE lv_values3 INTO ls_read_values WITH KEY key = lv_operators_id.

        ENDIF.

        IF ls_read_values-text IS NOT INITIAL.
          ls_t_text_set2-text = ls_read_values-text.

          APPEND ls_t_text_set2-text TO lt_t_text.

          lr_editor->delete_text(
            EXCEPTIONS
              error_cntl_call_method = 1
              OTHERS                 = 2 ).


          lr_editor->set_selected_text_as_r3table(
            EXPORTING
              table           = lt_t_text

            EXCEPTIONS
              error_dp        = 1
              error_dp_create = 2
              OTHERS          = 3 ).



          CLEAR ls_t_text.

          LOOP AT lt_t_text INTO ls_t_text.
            IF ls_t_text IS NOT INITIAL.
              CLEAR lv_string_formula.
              lv_string_formula = ls_t_text.

              lv_string_formula_main = |{ lv_string_formula_main }| & |{ lv_string_formula }|.

            ENDIF.
            CLEAR ls_t_text.
          ENDLOOP.

          IF  lv_string_formula_main IS NOT INITIAL.
            CLEAR ls_main-formula.
            ls_main-formula = lv_string_formula_main.
          ENDIF.

        ENDIF.

      ENDIF.

    ENDIF.


  ENDMETHOD.



  METHOD macros_variables.


    DATA: lv_len TYPE char11,
          lv_wid TYPE char11,
          lv_hei TYPE char11.


    DEFINE zvariables_formula.

      DATA: result TYPE string.

      result =  ls_main-formula.

      IF ls_main-formula CS 'LEN_EL'. " p_inp.
      REPLACE ALL OCCURRENCES OF 'LEN_EL' IN ls_main-formula WITH &1.
      ENDIF.
      IF ls_main-formula CS 'WID_EL'. " p_inp.
      REPLACE ALL OCCURRENCES OF 'WID_EL' IN ls_main-formula WITH &2.
      ENDIF.
      IF ls_main-formula CS 'HEI_EL'. " p_inp.
      REPLACE ALL OCCURRENCES OF 'HEI_EL' IN ls_main-formula WITH &3.
      ENDIF.


    END-OF-DEFINITION.



    IF len_in IS  NOT INITIAL.
      lv_len = len_in.
      CONDENSE lv_len NO-GAPS.
    ENDIF.

    IF wid_in IS  NOT INITIAL.
      lv_wid = wid_in.
      CONDENSE lv_wid NO-GAPS.
    ENDIF.

    IF hei_in IS  NOT INITIAL.
      lv_hei = hei_in.
      CONDENSE lv_hei NO-GAPS.
    ENDIF.



    IF ls_main-formula IS NOT INITIAL.
      zvariables_formula lv_len lv_wid lv_hei .
    ENDIF.


  ENDMETHOD.


  METHOD evaluator.

    CLEAR result_out.

    DATA: message      TYPE string,
          source       TYPE string,
          return_value TYPE string.

    DATA: js_processor TYPE REF TO cl_java_script.


    IF ls_main-formula IS NOT INITIAL.

      lcl_main=>macros_variables( ).

      message = ls_main-formula.


      js_processor = cl_java_script=>create( ).
      CONCATENATE
        'var string = ' message ';'
        'function Set_String()                          '
        '  { string = eval(string);                     '
        '  }                                            '
        'Set_String();                                  '
        'string;                                        '
          INTO source SEPARATED BY cl_abap_char_utilities=>cr_lf.

      return_value = js_processor->evaluate( source ).


      result_out = return_value.

    ENDIF.

  ENDMETHOD.



  METHOD set_display_text.

    IF   gv_reminder = 0.
      lr_editor->set_readonly_mode(
       EXPORTING
         readonly_mode          = 1
       EXCEPTIONS
         error_cntl_call_method = 1
         invalid_parameter      = 2
         OTHERS                 = 3 ).
    ENDIF.


  ENDMETHOD.


  METHOD set_change_text.

    gv_evenodd = gv_evenodd + 1.
    gv_reminder = gv_evenodd  MOD 2.

    IF   gv_reminder <> 0.
      lr_editor->set_readonly_mode(
       EXPORTING
         readonly_mode          = 0
       EXCEPTIONS
         error_cntl_call_method = 1
         invalid_parameter      = 2
         OTHERS                 = 3 ).
    ENDIF.


  ENDMETHOD.


METHOD clear_result.
  CLEAR result_out.
ENDMETHOD.

METHOD check_exist.

CLEAR ls_exist.

  SELECT SINGLE formula_name, formula
     FROM zformula_tab
     INTO CORRESPONDING FIELDS OF @ls_exist
             WHERE formula_name = @formula_name.


ENDMETHOD.


METHOD check_set_exist.

  lcl_main=>check_exist( ).

   IF ls_exist IS NOT INITIAL.

  CLEAR ls_main-formula.

          ls_main-formula = ls_exist-formula.

          lr_editor->set_textstream(
            EXPORTING
              text                   = ls_main-formula
            EXCEPTIONS
              error_cntl_call_method = 1
              not_supported_by_gui   = 2
              OTHERS                 = 3 ).

    ELSE.

       lr_editor->delete_text( ).

 ENDIF.

ENDMETHOD.


METHOD delete_entrie.



  IF ls_main IS NOT INITIAL.

    IF formula_name IS NOT INITIAL.

      CLEAR ls_delete.
      ls_delete-formula_name = formula_name.
      ls_delete-formula = ls_main-formula.

      IF ls_delete IS NOT INITIAL.

     DELETE zformula_tab FROM ls_delete.

        IF sy-subrc EQ 0.
          MESSAGE i004(zlrn239_msg_eval_cal).
          lr_editor->delete_text( ).
          CLEAR formula_name.
        ELSE.
          MESSAGE i005(zlrn239_msg_eval_cal).
        ENDIF.
      ENDIF.

    ELSE.
      MESSAGE i006(zlrn239_msg_eval_cal) DISPLAY LIKE 'E'.
    ENDIF.

ENDIF.


ENDMETHOD.

ENDCLASS.               "lcl_main



```

<p>*&---------------------------------------------------------------------*</p><h5 id="sfs1"><a href="#sfs11b">INCLUDE zgklrn239_eval_calc_sfs1_1</a></h5>
 <a href="#d011b">Back to main includes</a> <br>
*&---------------------------------------------------------------------* <br><br> <br><br> 

```console

START-OF-SELECTION.

  lcl_main=>screen_100( ).


```
<p>*&---------------------------------------------------------------------*</p><h5 id="pbo1"><a href="#pbo11b">INCLUDE zgklrn239_eval_calc_pbo1_1</a></h5>
 <!--  <a href="#d011b">Back to main includes</a> <br> -->
*&---------------------------------------------------------------------* <br><br> <br><br> 

```console
*&---------------------------------------------------------------------*
*&      Module  STATUS_0100  OUTPUT
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
MODULE status_0100 OUTPUT.
  SET PF-STATUS '0100'.
  SET TITLEBAR '0100'.

  lcl_main=>pbo( ).

ENDMODULE.
```
<p>*&---------------------------------------------------------------------*</p><h5 id="pbo2"><a href="#pbo21b">INCLUDE zgklrn239_eval_calc_pbo2_1</a></h5>
 <a href="#d011b">Back to main includes</a> <br>
*&---------------------------------------------------------------------* <br><br> <br><br> 

```console
MODULE list_fill_100 OUTPUT.
  lcl_main=>vrm_variables( ).
ENDMODULE.
```
<p>*&---------------------------------------------------------------------*</p><h5 id="pai1"><a href="#pai11b">INCLUDE zgklrn239_eval_calc_pai1_1</a></h5>
 <!--  <a href="#d011b">Back to main includes</a> <br> -->
*&---------------------------------------------------------------------* <br><br> <br><br> 

```console

MODULE user_command_0100 INPUT.

  lcl_main=>pai( ).

ENDMODULE.

```
<br><br>  
<h4 id="usage"> <a href="#usageb">Back top</a></h4>
<h2>Usage/behavior of program : </h2>  
<br><br>   

![description_of_usage](https://github.com/user-attachments/assets/71821c0b-499d-4725-b8ac-bb829399443a)

<ol type="1">
<li>Section - main tools for program</li>
<br>
         
<ul>
<li>a - Button to exit from program </li>
<li>b - Button to  Save entrie with formula in Database</li>
<li>c - Button to  Display/Change Editor (Section 2)</li>
<li>d - Button to  Clear all from Editor (Section 2)</li>
<li>b - Button to  Delete entrie with formula from Database</li>
</ul>

<br>       
<li>Section - tools for Editor</li>
<br>
<ul>
<li>a - Area to fill calculations/formula in Editor </li>
<li>b - Button to cut in Editor</li>
<li>c - Button to copy into Editor</li>
<li>d - Button to insert into Editor</li>
<li>e - Button to Undo in Editor</li>         
<li>f - Button to Restore in Editor</li>
<li>g - Button to Find/replace in Editor</li>
<li>h - Button to Find next in Editor</li>
<li>i - Button to Load local file in Editor</li>
<li>j - Button to Save as local file in Editor</li>
</ul>
<br>         
<li>Section - set of input/output fields</li>
<br>
<ul>
<li>a - input/output field for formula name to save (using save button from section 1) in database or get from database using "Get" button from this level</li> 
<li>b - Listbox field for provided variabels. Use "Insert" button from this level to insert variable into Edtior </li>
<br>
         
![var_section](https://github.com/user-attachments/assets/5b8582e8-69f1-493f-a07b-9f4c1be15c97)

<br>
<li>c - Listbox field for JS Logic or Functions. Use "Insert" button from this level to insert Logic or Function into Edtior </li>
<br>

![Log_Fun_section](https://github.com/user-attachments/assets/ba6c50cc-0a3b-4d0f-8829-0c037e9fbd38)

<br>
<li>d - Listbox field for accessible  JS Operators. Use "Insert" button from this level to insert Operator into Edtior </li>
<br>

![oper_section](https://github.com/user-attachments/assets/06ae300c-992d-4ea4-8e2e-2acef57ffb5c)

<br>
<li>e - This is example of input field which can be use to get data from Database. In this case if the field "Matnr" ( Material number) will be fielled , after "Get" button press from this level, fields from level f from that section will be populated (of course, if the data exists for the material) </li>
<li>f -  The reasult fields from e of this Section. Also to use values from this fields in Editor ,  they  must be embedded in b (Var)  of this section </li>
<li>g - Output field with the result of caculations from editor . Use "check" button to make calculation from editor and get result. Use anotfer button from this level ( from g level ) to clear reasult from output field</li>
</ul>
<br>
</ol>


<h3>JS Operators</h3>


<ol type="a">

<li>Supported</li>
<br>
<ul>
<li>Addition ( + ) </li>

![addition](https://github.com/user-attachments/assets/2cee3804-13eb-4153-bb0f-966324e2b137)
       
<li>Subtraction ( - ) </li>

![subtraction](https://github.com/user-attachments/assets/cf0bae41-63c9-4d81-a681-9daf5791e85e)

<li>Multiplication ( * ) </li>

![multiplication](https://github.com/user-attachments/assets/6cec5b57-390a-4a8f-8d9a-1fbc5472340a)

<li>Division ( / ) </li> 

![Division](https://github.com/user-attachments/assets/4cfa5ba5-ab9d-4920-89ac-849caddf3e5f)

<li>Modulus ( % ) </li>

![modulus](https://github.com/user-attachments/assets/a4ae5fbc-2675-4ce6-9ef6-65561a0a31cd)

</ul>
<br>
<li>Not supported</li>
<br>
<ul>
<li> Assignment (=) </li>
<li>Increment ( ++ ) </li>
<li>Decrement ( -- ) </li>
 <h6 id=mathpowb"></h6>        
<li>Exponentiation ( ** )   =>use this object <a href="#mathpow">Math.pow()</a></li> 
</ul>
</ol>

<br>
<h3>JS Comparison and Logical Operators</h3>
</ul>
<li>equal to ( == ) </li>

![equal to](https://github.com/user-attachments/assets/b703dc46-6e6f-4523-b002-fd6a070c2590)

<li>not equal ( != ) </li>

![not equal](https://github.com/user-attachments/assets/5495810f-2a62-4d8a-81cc-9ba89ce19f39)

<li> greater than ( > ) </li>

![greater than](https://github.com/user-attachments/assets/a57c16fa-c7ec-4ef1-9f18-95891a1264cb)

<li> less than ( < ) </li>

![less than](https://github.com/user-attachments/assets/fc709394-9d83-4c93-abc1-e127f2e2afcc)
       
<li> greater than or equal to ( >= ) </li>

![greater than or equal to](https://github.com/user-attachments/assets/d183730e-5d73-4d8b-937e-a6e03acefcd6)
       
<li> less than or equal to ( <= ) </li>

![less than or equal to](https://github.com/user-attachments/assets/2fb790b3-b26c-447c-aab2-be7b941d70bb)
       
<li> logical and ( && ) </li>

![ANDEXAMPLE](https://github.com/user-attachments/assets/f5a9377b-7670-44ec-9594-769616b64340)
       
<li> logical or ( || ) </li>

![OREXAMPLE](https://github.com/user-attachments/assets/fcc341e5-c3c6-4843-9cee-cbf0b8a59fb5)
       
<li> logical not ( ! ) </li>

![NOTEXAMPLE](https://github.com/user-attachments/assets/beb3165a-3bbf-42ef-b6e5-d442703266f9)
       
</ul>
<br>

<h3>Booleans</h3>

![booleanex1](https://github.com/user-attachments/assets/9a79b81a-8173-49ad-b31a-a845490948fc)

![booleanex2](https://github.com/user-attachments/assets/17324229-3d25-4a0b-ae6f-de09ee787012)

<br>
<h3>JS Math Object</h3>

<br>
</ul>
<li>min</li>

![Math min](https://github.com/user-attachments/assets/7d869193-00b8-49d2-af08-e32631140a86)

<li>max</li>

![Math max](https://github.com/user-attachments/assets/21f17eed-0c6b-4005-94dd-e753e8973aef)

<li><a href="#mathpowb">pow</a></li>

![Math pow](https://github.com/user-attachments/assets/7932043f-9d32-46bc-92bb-b31b0a699a0d)

<li>round</li>

![Math Round](https://github.com/user-attachments/assets/6af4fecf-9df2-4fef-9dc2-831e90e2ea86)

<li>ceil</li>

![Math ceil](https://github.com/user-attachments/assets/31580cf6-9623-4c4f-97a0-3d4750885cdd)

<li>floor</li>

![Math floor](https://github.com/user-attachments/assets/2a429f2b-ef6a-4a53-bc43-24bea927a2f7)

<li>abs</li>

![Math abs](https://github.com/user-attachments/assets/305be8d1-f55c-47f6-80f1-438a9413e306)

<li>sqrt</li>

![math sqrt](https://github.com/user-attachments/assets/80433325-f18e-44d6-9152-1f708608f63d)

<li>random</li>

![Math random](https://github.com/user-attachments/assets/a4a46343-c2a7-4672-b03c-df7ca6d8ce1f)

<li>PI</li>

![Math PI](https://github.com/user-attachments/assets/e9050ecb-c0c5-4bd8-b333-785134944f95)

</ul>


<br>
<h3>String methods</h3>

</ul>
<li>length</li>

!['' lenght](https://github.com/user-attachments/assets/8683c0df-e4d5-4899-aa2b-203664b64409)

<li>charAt()</li>

!['' charAt()](https://github.com/user-attachments/assets/3acd268a-488e-4254-a992-8ac56be25951)

<li>Property Access [ ]</li>

First sign

![''](https://github.com/user-attachments/assets/e1651ed5-5b7a-4f69-bde0-6b5ea84e24a2)

Last sign

![''  to get last char](https://github.com/user-attachments/assets/ecedbb0e-3dcd-4d5f-8218-1b04cb74bb2a)

<li>substring()</li>

!['' substring( , )](https://github.com/user-attachments/assets/a9424bc0-c49f-4c06-9dca-f82d96962c47)


<li>substr()</li>

!['' substr( , )](https://github.com/user-attachments/assets/09eb3b74-e1f8-4fee-a077-a36670c52ba6)

!['' substr( -) get last char](https://github.com/user-attachments/assets/288a08ad-2da7-40c9-a693-a42b4b6ef7e0)

</ul>


<br>

<h3>JS Array Methods</h3>
<br>
</ul>
<li>join</li>

![Array join](https://github.com/user-attachments/assets/8461c0ee-f1d8-4de6-aca8-fd36ec2e5953)

<li>lenght</li>

![Array lenght](https://github.com/user-attachments/assets/a664dc4f-1450-4f79-9cb7-b44650994565)

<li>[]</li>

![array](https://github.com/user-attachments/assets/bda966a2-506d-491e-8a73-038a86d8d1e4)

</ul>

<h3>Conditional Statements - if, else, else if</h3>
<o> To use them the source code must be embedded in eval(" ")</o>
</ul>
<li>if</li>

eval("if ( condition ) { true; } ")

![ifonly](https://github.com/user-attachments/assets/5a6525cc-6099-4b55-9154-41ecacc5cc12)

<li>else</li>

eval("if ( condition ) { true; } else { false; }")

![if](https://github.com/user-attachments/assets/aa853218-2dae-4f04-b330-c461fae60688)

<li>else if</li>

eval("if ( condition ) { true; } else if ( condition2 ) { true; } else { ; } ")

![elseif21](https://github.com/user-attachments/assets/fd9a7a36-8e5c-47c3-a5ec-2d607133f85d)

or we can use

eval("if ( condition ) { true; } else {if ( condition ) { true; } else { false; }; }")

![ifelseex1](https://github.com/user-attachments/assets/11b7b621-a2b3-4309-ad76-a8db827575ab)

<li>with logical operators</li>

if with logical and ( && )

eval("if ( cond1 && cond2 ) { true; } else { false; }")

![if ex1](https://github.com/user-attachments/assets/c8b68fe6-8ac2-43b6-8e5b-c9b4fd477f3e)

if with logical or ( || )

eval("if ( cond1 || cond2 ) { true; } else { false; }")

![IForex1](https://github.com/user-attachments/assets/4fcb55ec-1898-47f0-a72b-0f237a65a232)

</ul>
<br><br>

<h3>ParseFloat</h3>

![parsefloat](https://github.com/user-attachments/assets/aec3f2bd-d020-467c-9261-844c27c4258e)

<h3>Date</h3>
<br>
</ul>
<li>new Date() - cunnrent date and time </li>

![new Date()](https://github.com/user-attachments/assets/f70b702c-8701-4236-9518-8b77126837b3)


<li>new Date(date string)</li>

![new DateSeptember 24 2024 215400](https://github.com/user-attachments/assets/a4d5a1c3-25ea-4a02-8a3b-b5013cdc61f0)

<li>new Date(year, month, ...) - 7 numbers in that order specify year, month, day, hour, minute, second, and millisecond </li>

![new Date(year,month, )](https://github.com/user-attachments/assets/b7b6732c-eee5-42e7-bf7b-2dc368c8d5db)

<li>Short Date</li>

![short](https://github.com/user-attachments/assets/e5b354c1-9271-4e09-939e-c550391ae0a8)

<li>Long Date</li>

![new date( sep 24 2024)](https://github.com/user-attachments/assets/f8e8c88c-0e6c-4cec-9a1c-d799fae9a15c)


<li>getFullYear() - year(yyyy)</li>

![getfullyear](https://github.com/user-attachments/assets/11e7418f-f9b1-4825-893a-1fe21caf6bcb)

<li>getMonth() - month(0-11)</li>

![getMonth()](https://github.com/user-attachments/assets/2fbf8e24-9cbf-4763-b3b9-2c5883019280)

<li>getDate() - day(1-31)</li>

![getDate()](https://github.com/user-attachments/assets/7f98a16a-9042-4f9e-9c43-0a1573abb7c5)

<li>getDay() - weekday(1-31)</li>

![getDay()](https://github.com/user-attachments/assets/09c04980-ccf7-4c0f-a775-65dff4fb3d96)

<li>getHours() - hour(0-23)</li>

![getHours()](https://github.com/user-attachments/assets/87eb31ae-41b9-4119-9e7b-9ccda91ad910)

<li>getMinutes() - minute(0-59)</li>

![getMinutes()](https://github.com/user-attachments/assets/a954fb7b-19e0-4495-83c8-24c8e336381e)

<li>getSeconds() - second(0-59)</li>

![getSeconds()](https://github.com/user-attachments/assets/3445e59d-3bc3-4cca-9d57-8be2c4ea6e15)


<li>getMilliseconds() - millisecond(0-999)</li>

![getMilliseconds()](https://github.com/user-attachments/assets/c0b3ded2-b04c-47cf-92da-78ece11cf11f)

<li>getTime() - time(milliseconds since January 1, 1970)</li>

![getTime()](https://github.com/user-attachments/assets/cd60f733-f0ba-4f9e-a64e-9f4bbd032c4b)

</ul>





<br><br>
<h3>More compleks formula example:</h3>

![more_complex_formula_example](https://github.com/user-attachments/assets/87b5afeb-50d2-4fa3-87e0-7722c824137d)

<br>
<p>Thank you!</p>
<br>
<p>Sources for JS Code :</p>
<p>https://www.w3schools.com/js/</p>







