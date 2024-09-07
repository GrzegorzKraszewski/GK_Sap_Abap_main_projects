<h6 id="top1b"></h6>
<a href="#top1">INCLUDE zgklrn239_eval_calc_top_1..</a>

<h6 id="d011b"></h6>
<a href="#d01">INCLUDE zgklrn239_eval_calc_d01_1..</a>

<h6 id="i011b"></h6>
<a href="#i01">INCLUDE zgklrn239_eval_calc_i01_1...</a>


INCLUDE zgklrn239_eval_calc_sfs1_1.

INCLUDE zgklrn239_eval_calc_pbo1_1.

INCLUDE zgklrn239_eval_calc_pbo2_1.

INCLUDE zgklrn239_eval_calc_pai1_1.



<p>*&---------------------------------------------------------------------*</p><h5 id="top1"> <a href="#top1b">INCLUDE zgklrn239_eval_calc_top_1</a></h5>

*&---------------------------------------------------------------------*  


```console
TYPE-POOLS : vrm.

DATA : lv_zmienne_id TYPE vrm_id,
       lv_values        TYPE vrm_values,
       lv_value         LIKE LINE OF lv_values,

       lv_fx_id      TYPE vrm_id,
       lv_values2    TYPE vrm_values,
       lv_value2     LIKE LINE OF lv_values,

       lv_operators_id   TYPE vrm_id,
       lv_values3       TYPE vrm_values,
       lv_value3        LIKE LINE OF lv_values.



TYPES: BEGIN OF ty_main,reg,
         formula    TYPE string,
         matnr      TYPE mara-matnr,
         laeng      TYPE mara-laeng,
         breit      TYPE mara-breit,
         hoehe      TYPE mara-hoehe,
         result_out TYPE char11,
       END OF   ty_main.


DATA: ls_main       TYPE ty_main,
*      ls_save       TYPE , "tabela w której będą zapisywane dane
*      ls_delete     TYPE , "tabela w której będą zapisywane dane
*      ls_exist      TYPE , "tabela w której będą zapisywane dane

      lv_flag_exist TYPE char1.



DATA: ok_code     TYPE sy-ucomm,
      lr_container TYPE REF TO cl_gui_custom_container, "g_container
      lr_editor    TYPE REF TO cl_gui_textedit. "g_editor


DATA:
  matnr_in   TYPE mara-matnr,
  len_in     TYPE mara-laeng,
  wid_in     TYPE mara-breit,
  hei_in     TYPE mara-hoehe,
  result_out TYPE char11.


DATA:
  lv_from_line     TYPE i,
  lv_from_pos      TYPE i,
  lv_to_line       TYPE i,
  lv_to_pos        TYPE i,
  lv_string_lengh  TYPE i,
  lv_string_sub    TYPE i,
  lv_string_part1  TYPE string,
  lv_string_part2  TYPE string,
  lv_str_main_part TYPE string,
  lv_real_position TYPE i.



DATA: lv_str_main  TYPE string,
      lv_str       TYPE string,
      lt_split_tab TYPE TABLE OF string.


TYPES: BEGIN OF ty_read_val,
         key  TYPE char40,
         text TYPE char80,
       END OF ty_read_val.



DATA: ls_read_values TYPE ty_read_val.


TYPES: BEGIN OF ty_text_set2,
         text TYPE string,
       END OF  ty_text_set2.

DATA: lt_t_text_set2 TYPE TABLE OF ty_text_set2,
      ls_t_text_set2 TYPE ty_text_set2.


TYPES: BEGIN OF ty_text_from_edit,
         line TYPE i,
         text TYPE char200,
       END OF ty_text_from_edit.

DATA : lt_t_text              TYPE TABLE OF char200,
       ls_t_text              TYPE char200,
       lt_t_text2             TYPE STANDARD TABLE OF ty_text_from_edit,
       lv_string_formula      TYPE string,
       lv_string_formula_main TYPE string.


DATA: lv_pierwsz TYPE char1,
      gv_flag(1).

DATA: gv_reminder TYPE i.
DATA: gv_evenodd TYPE i.


DATA:lv_spac  TYPE char1 VALUE '#'.
DATA:lv_spaces  TYPE string.
DATA:lv_line_edit_len  TYPE i.
DATA:lv_line_edit_sub  TYPE i.

```


<p>*&---------------------------------------------------------------------*</p><h5 id="top1"><a href="#d011b">INCLUDE zgklrn239_eval_calc_d01_1</a></h5>
 <a href="#d011b">Back to main includes</a> <br>
*&---------------------------------------------------------------------*  
