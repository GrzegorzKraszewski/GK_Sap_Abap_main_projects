

<h2>This program shows the way how you can generate formula/calculations using javascript evaluator class and macros in SAP ABAP </h2><br><br>

<h3>Key factors</h3><br><br> 
<p>We aren't declairng variables in editor as let, var etc.</p>
<p>Posible variables to use are embedded in var listbox => there are used macros for it and we creating a list with accessible variables to use in program</p>

<h3>Final look of program:</h3><br><br> 



<h6 id="usageb"></h6>
<h3> Too see usage/behavior of program please click <a href="#usage">here</a></h3<br><br>


<h3>Source code:</h3> <br><br><br>
*&---------------------------------------------------------------------*<br>
*& Report ZGKLRN239_EVAL_CALC<br>
*&---------------------------------------------------------------------*<br>
*& <br>
*&---------------------------------------------------------------------*<br>
REPORT zgklrn239_eval_calc MESSAGE-ID zlrn239_msg_eval_cal.<br>
<br>

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


<p>*&---------------------------------------------------------------------*</p><h5 id="d01"><a href="#d011b">INCLUDE zgklrn239_eval_calc_d01_1</a></h5>
 <a href="#d011b">Back to main includes</a> <br>
*&---------------------------------------------------------------------* <br><br> <br><br> 

<h5 id="usage"> <a href="#usageb">Back top</a></h5>
<h2>Usage/behavior of program : </h2>  
<br><br>   

<h3>explanation</h3>


<h3>JS Operators</h3>


<ol type="a">

<li>Supported</li>
<br>
<ul>
<li>Addition ( + ) </li>
<li>Subtraction ( - ) </li>
<li>Multiplication ( * ) </li>
<li>Division ( / ) </li> 
<li>Modulus ( % ) </li>
</ul>
<br>
<li>Not supported</li>
<br>
<ul>
<li> Assignment (=) </li>
<li>Increment ( ++ ) </li>
<li>Decrement ( -- ) </li>
<li>Exponentiation ( ** )   =>use this object Math.pow()</li> 
</ul>
</ol>

<br>
<h3>JS Comparison and Logical Operators</h3>
</ul>
<li>equal to ( == ) </li>
<li>not equal ( != ) </li>
<li> greater than ( > ) </li>
<li> less than ( < ) </li>
<li> greater than or equal to ( >= ) </li>
<li> less than or equal to ( <= ) </li>
<li> logical and ( && ) </li>
<li> logical or ( || ) </li>
<li> logical not ( ! ) </li>
</ul>
<br>

<h3>JS Math Object</h3>

<br>
</ul>
<li>min</li>

![Math min](https://github.com/user-attachments/assets/7d869193-00b8-49d2-af08-e32631140a86)

<li>max</li>

![Math max](https://github.com/user-attachments/assets/21f17eed-0c6b-4005-94dd-e753e8973aef)

<li>pow</li>

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

![''](https://github.com/user-attachments/assets/e1651ed5-5b7a-4f69-bde0-6b5ea84e24a2)

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
eval("if ( condition ) { true; } else {if ( condition ) { true; } else { false; }; }")

![ifelseex1](https://github.com/user-attachments/assets/11b7b621-a2b3-4309-ad76-a8db827575ab)

<li>with logical operators</li>

</ul>
<br><br>
<h3>More compleks formula</h3>

<br>
<p>Sources for JS Code :</p>
<p>https://www.w3schools.com/js/</p>







