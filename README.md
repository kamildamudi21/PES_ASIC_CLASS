
# PES_ASIC_CLASS
# Week1


<details>
  <summary>Day 1 - Introduction to RISCV ISA and GNU Compiler Toolchain </summary>
  <br>
  
### C program for Sum 1 to N.

#### Compiling it using C compiler.

![WhatsApp Image 2023-08-21 at 13 50 19](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/b3f87d49-77e7-47c5-a5ca-17116cad44a9)

#### Compiling using RISC-V Compiler.

```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
spike pk sum1ton.o
```
![WhatsApp Image 2023-08-21 at 13 50 20](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/e929d2ce-49f9-45a9-871e-3eb169cbbd35)

#### To debug the ALP generated by the compiler

```
spike -d pk sum1ton.o
```
![debug](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/a8ae913e-1623-4e06-84c8-0fef0e932645)

### C program that shows the maximum and minimum values of 64bit unsigend and signed numbers.

```
unsign_sign.c
```
![unsign](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/817ccbd8-79a1-4fa7-b45d-a1b5a5b0f722)
</details>

<details>
  <summary>Day 2 - Introduction to ABI and Basic Verification Flow </summary>
  <br>

### Simulate a C program using ABI function call (using registers) and execute
![DAy2_1](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/53ae4c51-715c-45ba-bc4e-46220b187f76)
![DAY2_2](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/700d7efe-a7b0-48a9-94fb-cd7200f3eef1)


### Further we will see how to run a C program on on RISC-V CPU

- Input : C Program loaded into memory to RISC-V CPU in Hex format

CPU processes the contents of the memory and provides with output using iverilog 

- Risc-V CPU : ```Picorv32.v```
- Testbench for verification : ```testbench.v```
- Tool : ```iverilog```
- script : ```rv32im.sh``` : has the commands to get the c-program, ALP, converts into hex format, loads into memory of riscv cpu, passes it iverilog and provides the output

![picorv1](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/fa0081d4-9d47-4243-8bc9-d64b8bda8ecb)


</details>

# week 2
#  RTL design using Verilog with SKY130 Technology 

<details>
    <summary> Day 1 - Introduction to iVerilog, GTKwave and Yosys  </summary>
    <br>
      
## Task 1
## Loading a mux and it's testbench into iverilog 
    
+ `cd vsd/sky130RTLDesignAndSynthesisWorkshop/verilog_files`
+ `iverilog good_mux.v tb_good_mux.v`
+ `./a.out`
+ ` gtkwave tb_good_mux.vcd`

  ![iverilogGTKwave1_2](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/ba25f023-c25b-4bc3-8092-a0af5f6fdf19)
    
    To see The Contents of the files:

    `gvim tb_good_mux.v -o good_mux.v`

  ![iverilogGTK2](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/be036b5d-5a72-4870-ace5-379d0b580024)

## Task 2
## Labs using Yosys and Sky130 PDKs
     
  +   To invoke **yosys**
  -  `cd`
  -  `cd vsd/sky130RTLDesignAndSynthesisWorkshop/verilog_files`
  -   Type `yosys`
     <br>

  
![yosyshh](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/f2c25433-33d2-4aad-a3e1-6479a20a66c8)



  + ` read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`
  +  `read_verilog good_mux.v`
  +  ` synth -top good_mux`

![goddmux](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/4365d2f3-0a62-4cf8-87aa-461813a6c148)


  + To generate the netlist

  `abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`
  
  <img width="271" alt="yosysabc" src="https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/4418e9f7-a304-4516-983c-274dffb2ef6f">

  + To see the logic realised
   `show`

![syn](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/31aa6ab2-d933-4971-a91e-9f7645748fff)


  To write the netlist

   - `write_verilog good_mux_netlist.v`
   - `!gvim good_mux_netlist.v`

  - To view a simplified code
     
     ` write_verilog -noattr good_mux_netlist.v`
     
     `!gvim good_mux_netlist.v`
    
<img width="224" alt="yosysreadnetlist" src="https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/4b0c12a1-f520-43bc-bcea-5df602a15373">

![YOSYSNETVIEW](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/c42e86b6-1eee-4b6f-ae43-5dc65f3338da)
</details>  

<details>
  <summary> Day 2 - Timing libs, hierarchical vs flat synthesis and efficient flop coding styles </summary>
  <br>

## Introduction to .lib

## Task 1
### Command to invoke sky130_fd_sc_hd__tt_025C_1v80.lib file 

```
 vim ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![vim task1](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/f5c3ed13-8151-4712-875c-e012b6371ea6)

## Task 2
## Hier synthesis flat synthesis 

```
yosys
read_liberty -lib ../lib//sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_modules
```

![gvimtask2](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/eff510c4-cc38-496f-908b-16da24b27f96)
![gvimshowtask2](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/8778bf52-67b4-4b35-8304-4b81f04e08ca)

```
write_verilog multiple_modules_hier.v
!vim multiple_modules_hier.v 
```
![task2last](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/d1492953-aa73-459b-a3b3-f837759fc80c)

## Task 3

## Various Flop Coding Styles and optimization

### For asynchronous reset
```
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd 
```
![AsynchornousTask3](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/80cbe20d-5692-4bec-a77f-502303f98995)

### For asynchronous set
```
iverilog dff_async_set.v tb_dff_async_set.v
./a.out
gtkwave tb_dff_async_set.vcd
```
![AsynchornousTask33](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/b59bb041-f3a4-4fd5-acec-71922d50d273)

### For Synchronous reset
```
iverilog dff_syncres.v tb_dff_syncres.v
./a.out
gtkwave tb_dff_syncres.vcd 
```
![SynchronousTask3](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/1f12c9c3-72a0-493f-8393-636ead6965e6)

## Task 4
### Synthesizing all 3 codes using yosys

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib//sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![Task4show](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/2c1cde75-55cd-4749-8102-aa660bf0c607)

```
read_verilog dff_async_set.v
synth -top dff_async_set
dfflibmap -liberty ../lib//sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![TASK4Show2](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/a1eb1a7a-697a-4bd2-b81e-a241c2255795)

```
read_verilog dff_syncres.v
synth -top dff_syncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![TASk4Show3](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/d41e0d2d-d971-414b-a57f-4139d8fae5b5)

</details>

<details>
  <summary> Day 3 - Combinational and Sequential Optimizations </summary>
  <br>

# Introduction to optimizations
## Combinational logic optimizations
**opt_check1.v**
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog opt_check.v
synth -top opt_check
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![opt_check1](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/64f56b67-68cb-447b-a67c-a72f3c1e87ef)


**opt_check2.v**
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog opt_check2.v
synth -top opt_check2
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![optcheck2](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/ac82624e-d9f4-4251-a6a3-c78276514ed2)

**opt_check3.v**

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog opt_check3.v
synth -top opt_check3
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![optcheck3](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/0f7af77b-53ef-430c-907d-d81986b6e84b)

**multiple_module_opt.v**
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog multiple_module_opt.v
synth -top multiple_module_opt
flatten
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![multiple_module](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/fa8841f9-bdfc-496b-afb9-2e8ca5b9e385)

# Sequential logic optimizations
**dff_const1.v**

```
iverilog dff_const1.v tb_dff_const1.v
./a.out
gtkwave tb_dff_const1.vcd
```
![dffcon1](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/00a2f90b-2f90-4fa5-949f-04bd1337fe9d)

**Synthesis**
```
  read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
  read_verilog dff_const1.v
  synth -top dff_const1
  dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
  show
```
![dffcon1_synth](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/c1ebaa9d-5efc-419a-8e72-e1d73ea929a2)

**dff_const2.v**
```
iverilog dff_const2.v tb_dff_const2.v
./a.out
gtkwave tb_dff_const2.vcd
```
![dffcon2](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/d1b6b370-3d10-4c34-8655-4705d1cc9156)


**Synthesis**
```
  read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
  read_verilog dff_const2.v
  synth -top dff_const2
  dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
  show
```
![dffcon2_synth](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/28e578f6-658b-4383-8540-e79f1a276856)


**dff_const3.v**

```
iverilog dff_const3.v tb_dff_const2.v
./a.out
gtkwave tb_dff_const3.vcd
```
![dffcon3](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/7902c776-d76e-45e8-99d3-83a4d11296e0)


**Synthesis**
```
  read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
  read_verilog dff_const3.v
  synth -top dff_const3
  dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
  show
```
![dffcon3_synth](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/35b2bbe3-f689-418c-a487-b3a56c689f14)

# Sequential optimzations for unused outputs

**counter_opt.v**
**Synthesis**
```
  read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
  read_verilog counter_opt.v
  synth -top counter_opt
  dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
  show
```
![counter_opt](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/e4315532-4ea8-44e1-9375-d5c765f4d77c)

**counter_opt2.v**

**Synthesis**
```
  read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
  read_verilog counter_opt2.v
  synth -top counter_opt
  dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
  show
```

![counteropt](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/b14d90f0-0a6e-4e6a-9268-45dac480f327)


</details>

<details>
  <summary> Day 4 - GLS and Synthesis-Simulation Mismatch </summary>
  <br>

**ternary_operator_mux.v**
	
**Simulation**
```
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
![Ternary operator](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/f77cdb7a-12e4-4ec2-896b-47462c141a00)

**Synthesis**
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog -noattr ternary_operator_mux_netlist.v
show
```
![Ternary operator_synth](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/2b7048f9-7874-458f-8846-f6ef375639d9)

**GLS**
To to Gate level simulation, Invoke iverilog with verilog modules
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_netlist.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
![ternary_operator_gls](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/3fa1f81b-6798-438c-b295-7b3a16de201d)

** bad_mux.v**
**RTL Simulation**
```
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```
![badmux](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/67b6e9b5-38c2-4156-be0b-ba598d8ee8f8)


**Synthesis**
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog bad_mux.v
synth -top bad_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog -noattr bad_mux_netlist.v
show
```
![badmux_synth](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/2e672237-30db-4ef6-90bc-a18e7687faa4)

**GLS**
To to Gate level simulation, Invoke iverilog with verilog modules
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_netlist.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```
![badmuxgls](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/a69f7527-b0e8-4e51-86d8-f6ef8cb1e1da)

# Labs on synth-sim mismatch for blocking statement
**blocking_caveat.v**

**RTL Simulation**
```
iverilog blocking_caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```
![blockingcaveat](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/bb532eae-8f69-47be-a167-e95776fd1822)

**Synthesis**
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog -noattr blocking_caveat_netlist.v
show
```
![blockingcaveatsynth](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/e32d47df-0518-4578-bdc0-985610c63bde)


**GLS**
To to Gate level simulation, Invoke iverilog with verilog modules
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_netlist.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```
![blockingcaveatgls](https://github.com/kamildamudi21/PES_ASIC_CLASS/assets/141449459/a48509d0-f17c-444e-ad34-b50cb3c112a6)















