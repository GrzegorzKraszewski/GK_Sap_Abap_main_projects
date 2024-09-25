

<h2>This program shows the way how you can generate formula/calculations using javascript evaluator class and macros in SAP ABAP </h2><br><br>

<h3>Key factors</h3><br><br> 
<p>We aren't declairng variables in editor as let, var etc.</p>
<p>Posible variables to use are embedded in var listbox => there are used macros for it and we creating a list with accessible variables to use in program</p>
<p>Program present possible useful operators methods etc.</p>
<h3>The final look of the program:</h3><br><br> 



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
 <a href="#d011b">Back to main includes</a> <br>
*&---------------------------------------------------------------------* <br><br> <br><br> 

```console

CLASS lcl_main DEFINITION.

  PUBLIC SECTION.

    CLASS-METHODS:
      get_dat_form,
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
      pop_up_exist_indb,
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
 <a href="#d011b">Back to main includes</a> <br>
*&---------------------------------------------------------------------* <br><br> <br><br> 

```console

CLASS lcl_main IMPLEMENTATION.

  METHOD screen_100.
    CALL SCREEN 100.
  ENDMETHOD.

  METHOD pbo.

    lcl_main=>create_containers_txt( ).

    lcl_main=>get_dat_form( ).

    lcl_main=>set_display_text( ).


  ENDMETHOD.



```

<p>*&---------------------------------------------------------------------*</p><h5 id="sfs1"><a href="#sfs11b">INCLUDE zgklrn239_eval_calc_sfs1_1</a></h5>
 <a href="#d011b">Back to main includes</a> <br>
*&---------------------------------------------------------------------* <br><br> <br><br> 

```console

START-OF-SELECTION.

  lcl_main=>screen_100( ).


```
<p>*&---------------------------------------------------------------------*</p><h5 id="pbo1"><a href="#pbo11b">INCLUDE zgklrn239_eval_calc_pbo1_1</a></h5>
 <a href="#d011b">Back to main includes</a> <br>
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
 <a href="#d011b">Back to main includes</a> <br>
*&---------------------------------------------------------------------* <br><br> <br><br> 

```console

MODULE user_command_0100 INPUT.

  lcl_main=>pai( ).

ENDMODULE.

```

<h5 id="usage"> <a href="#usageb">Back top</a></h5>
<h2>Usage/behavior of program : </h2>  
<br><br>   

<h3>explanation</h3>

<h3>What we can do/what we can use in program operators methods etc.</h3>
<h3>Useful and supported operators methods</h3>

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
<li>Exponentiation ( ** )   =>use this object Math.pow()</li> 
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
<h3>More compleks formula</h3>

<br>
<p>Sources for JS Code :</p>
<p>https://www.w3schools.com/js/</p>







