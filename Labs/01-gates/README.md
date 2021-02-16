# *Digital-electronics-1*


## *1. Verification of De Morgan's laws*

### *f(c,b,a)=((not b) and a) or ((not c)and (not b))*


c | b | a | f(c,b,a)
--| --|---|---------
0 | 0 | 0 |    1
0 | 0 | 1 |    1
0 | 1 | 0 |    0
0 | 1 | 1 |    0
1 | 0 | 0 |    0
1 | 0 | 1 |    1
1 | 1 | 0 |    0
1 | 1 | 1 |    0


### **VHDL code**

------------------------------------------------------------------------
--
-- Example of basic OR, AND, XOR gates.
-- Nexys A7-50T, Vivado v2020.1, EDA Playground
--
-- Copyright (c) 2019-2020 Tomas Fryza
-- Dept. of Radio Electronics, Brno University of Technology, Czechia
-- This work is licensed under the terms of the MIT license.
--
------------------------------------------------------------------------

library ieee;               -- Standard library
use ieee.std_logic_1164.all;-- Package for data types and logic operations

------------------------------------------------------------------------
-- Entity declaration for basic gates
------------------------------------------------------------------------
entity gates is
    port(
        a_i    : in  std_logic;         -- Data input
        b_i    : in  std_logic;         -- Data input
        c_i    : in  std_logic;          -- Data input
     --   for_o  : out std_logic;         -- OR output function
       -- fand_o : out std_logic;         -- AND output function
       -- fxor_o : out std_logic          -- XOR output function
       f_o : out std_logic               -- XOR output function
    );
end entity gates;

------------------------------------------------------------------------
-- Architecture body for basic gates
------------------------------------------------------------------------
architecture dataflow of gates is
begin
  --  for_o  <= a_i or b_i;
 --   fand_o <= a_i and b_i;
  --  fxor_o <= a_i xor b_i;
  f_o <= ((not b_i) and a_i) or ((not c_i) and (not b_i));

end architecture dataflow;


### *Wave form image from my EDA Playground Digital electronics 1*

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/digi%20electronics%201%20wave%20form.png)


### *Here is my [EDA Playground](https://www.edaplayground.com/x/8Lk8) for Digital electronics 1*