# *Digital-electronics-1*

## *Laborotory 2*

[My Digital-electronics-1 git repository](https://github.com/shad0w3y3/Digital-electronics-1)

## *1. 2-bit comparator truth table*

  Dec. equivalent    |    B[1:0]   |    A[1:0]   |    B is greater than A   |    B equals A   |    B is less then A 
---|---|---|---|---|---
0 |     0 0     |      0 0    |            0             |        1        |           0
1 |     0 0     |      0 1    |            0             |        0        |           1
2 |     0 0     |      1 0    |            0             |        0        |           1
3 |     0 0     |      1 1    |            0             |        0        |           1
4 |     0 1     |      0 0    |            1             |        0        |           0
5 |     0 1     |      0 1    |            0             |        1        |           0
6 |     0 1     |      1 0    |            0             |        0        |           1
7 |     0 1     |      1 1    |            0             |        0        |           1
8 |     1 0     |      0 0    |            1             |        0        |           0
9 |     1 0     |      0 1    |            1             |        0        |           0
10 |     1 0     |      1 0    |            0             |        1        |           0
11 |     1 0     |      1 1    |            0             |        0        |           1
12 |     1 1     |      0 0    |            1             |        0        |           0
13 |     1 1     |      0 1    |            1             |        0        |           0
14 |     1 1     |      1 0    |            1             |        0        |           0
15 |     1 1     |      1 1    |            0             |        1        |           0 



## *2. 2-bit comparator*

### *K-maps*

#### Greater than K-map

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/02-logic/Pictures/greater%20than.jpg)

#### Equal to K-map

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/02-logic/Pictures/ab%20equals.png)

#### Less than K-map

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/02-logic/Pictures/less%20than.jpg)

### *Simplified SoP form of "greater than"*

*Greater_SoP = m4 + m8 + m9 + m12 + m13 + m14 = (/b1.b0./a1./a0) + (b1./b0./a1./a0) + (b1./b0./a1.a0) + (b1.b0./a1./a0) + (b1.b0./a1.a0) + (b1.b0.a1./a0)*


### *Simplified PoS form of "less than"*
*Less_PoS = m0 . m4 . m5 . m8 . m9. m10 . m12 . m13 . m14 . m15 = (b1+b0+a1+a0) . (b1+/b0+a1+a0) . (b1+/b0+a1+/a0) . (/b1+b0+a1+a0) . (/b1+b0+a1+/a0) . (/b1+b0+/a1+a0) . (/b1+/b0+a1+a0) . (/b1+/b0+a1+/a0) . (/b1+/b0+/a1+a0) . (/b1+/b0+/a1+/a0)*


### *My K-map examples in VHDL: https://www.edaplayground.com/x/Cctk * 

## *3. 4-bit binary comparator*

### *Design code:*

``` VHDL code
------------------------------------------------------------------------
--
-- Example of 4-bit binary comparator using the when/else assignment.
-- EDA Playground
--
-- Copyright (c) 2020-2021 Tomas Fryza
-- Dept. of Radio Electronics, Brno University of Technology, Czechia
-- This work is licensed under the terms of the MIT license.
--
------------------------------------------------------------------------

library ieee;
use ieee.std_logic_1164.all;

------------------------------------------------------------------------
-- Entity declaration for 4-bit binary comparator
------------------------------------------------------------------------
entity comparator_4bit is
    port(
        a_i           : in  std_logic_vector(4 - 1 downto 0);
		b_i           : in  std_logic_vector(4 - 1 downto 0);
		B_greater_A_o    : out std_logic;
		B_equals_A_o     : out std_logic;
        B_less_A_o       : out std_logic       -- B is less than A
        
    );
end entity comparator_4bit;

------------------------------------------------------------------------
-- Architecture body for 4-bit binary comparator
------------------------------------------------------------------------
architecture Behavioral of comparator_4bit is
begin
    B_greater_A_o <= '1' when (b_i > a_i) else '0';
    B_equals_A_o <= '1' when (b_i = a_i) else '0';
    B_less_A_o   <= '1' when (b_i < a_i) else '0';


end architecture Behavioral;
```

### *Testbench code:*

```VHDL code
------------------------------------------------------------------------
--
-- Testbench for 4-bit binary comparator.
-- EDA Playground
--
-- Copyright (c) 2020-2021 Tomas Fryza
-- Dept. of Radio Electronics, Brno University of Technology, Czechia
-- This work is licensed under the terms of the MIT license.
--
------------------------------------------------------------------------

library ieee;
use ieee.std_logic_1164.all;

------------------------------------------------------------------------
-- Entity declaration for testbench
------------------------------------------------------------------------
entity tb_comparator_4bit is
    -- Entity of testbench is always empty
end entity tb_comparator_4bit;

------------------------------------------------------------------------
-- Architecture body for testbench
------------------------------------------------------------------------
architecture testbench of tb_comparator_4bit is

    --Internal Local signals
    signal s_a       : std_logic_vector(4 - 1 downto 0);
    signal s_b       : std_logic_vector(4 - 1 downto 0);
    signal s_B_greater_A : std_logic;
    signal s_B_equals_A  : std_logic;
    signal s_B_less_A    : std_logic;

begin
    -- Connecting testbench signals with comparator_4bit entity (Unit Under Test)
    uut_comparator_4bit : entity work.comparator_4bit
        port map(
            a_i           => s_a,
            b_i           => s_b,
            B_greater_A_o => s_B_greater_A,
            B_equals_A_o  => s_B_equals_A,
            B_less_A_o    => s_B_less_A
        );

    --------------------------------------------------------------------
    -- Data generation process
    --------------------------------------------------------------------
    p_stimulus : process
    begin
        -- Report a note at the begining of stimulus process
        report "Stimulus process started" severity note;
		

        -- 1st test values
        s_b <= "0000"; s_a <= "0000"; wait for 100 ns;        
        -- Expected output      
    assert ((s_B_greater_A = '0') and (s_B_equals_A = '1') and (s_B_less_A = '0'))    
        -- If false, then report an error
        report "Test failed for input combination: 0000, 0000" severity error;

		-- 2nd test values
        s_b <= "0000"; s_a <= "0001"; wait for 100 ns;        
        -- Expected output      
    assert ((s_B_greater_A = '0') and (s_B_equals_A = '0') and (s_B_less_A = '1'))    
        -- If false, then report an error
        report "Test failed for input combination: 0000, 0001" severity error;
        
        -- 3rd test values
        s_b <= "0000"; s_a <= "0101"; wait for 100 ns;        
        -- Expected output      
    assert ((s_B_greater_A = '0') and (s_B_equals_A = '0') and (s_B_less_A = '1'))    
        -- If false, then report an error
        report "Test failed for input combination: 0000, 0101" severity error;
        
        -- 4th test values
        s_b <= "1000"; s_a <= "1001"; wait for 100 ns;        
        -- Expected output      
    assert ((s_B_greater_A = '0') and (s_B_equals_A = '0') and (s_B_less_A = '1'))    
        -- If false, then report an error
        report "Test failed for input combination: 1000, 1001" severity error;
        
        -- 5th test values
        s_b <= "0001"; s_a <= "0000"; wait for 100 ns;        
        -- Expected output      
    assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '0'))    
        -- If false, then report an error
        report "Test failed for input combination: 0001, 0000" severity error;
        
        -- 6th test values
        s_b <= "0011"; s_a <= "1001"; wait for 100 ns;        
        -- Expected output      
    assert ((s_B_greater_A = '0') and (s_B_equals_A = '0') and (s_B_less_A = '1'))    
        -- If false, then report an error
        report "Test failed for input combination: 0011, 1001" severity error;
        
        -- 7th test values
        s_b <= "0100"; s_a <= "0010"; wait for 100 ns;        
        -- Expected output      
    assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '0'))    
        -- If false, then report an error
        report "Test failed for input combination: 0100, 0010" severity error;
        
        -- 8th test values
        s_b <= "0111"; s_a <= "1100"; wait for 100 ns;        
        -- Expected output      
    assert ((s_B_greater_A = '0') and (s_B_equals_A = '0') and (s_B_less_A = '1'))    
        -- If false, then report an error
        report "Test failed for input combination: 0111, 1100" severity error;
        
        -- 9th test values
        s_b <= "1110"; s_a <= "1111"; wait for 100 ns;        
        -- Expected output      
    assert ((s_B_greater_A = '0') and (s_B_equals_A = '0') and (s_B_less_A = '1'))   
        -- If false, then report an error
        report "Test failed for input combination: 1110, 1111" severity error;
        
        -- 10th test values
        s_b <= "1111"; s_a <= "1111"; wait for 100 ns;        
        -- Expected output      
    assert ((s_B_greater_A = '0') and (s_B_equals_A = '0') and (s_B_less_A = '1'))    
        -- If false, then report an error
        report "Test failed for input combination: 1111, 1111" severity error;
        



        -- Report a note at the end of stimulus process
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;

end architecture testbench;
```

### *Listing of simulation console output:*

``` VHDL
[2021-02-22 13:24:37 EST] ghdl -i design.vhd testbench.vhd  && ghdl -m  tb_comparator_4bit && ghdl -r  tb_comparator_4bit   --vcd=dump.vcd && sed -i 's/^U/X/g; s/^-/X/g; s/^H/1/g; s/^L/0/g' dump.vcd 
analyze design.vhd
analyze testbench.vhd
elaborate tb_comparator_4bit
testbench.vhd:51:9:@0ms:(report note): Stimulus process started
testbench.vhd:120:5:@1us:(assertion error): Test failed for input combination: 1111, 1111
testbench.vhd:128:9:@1us:(report note): Stimulus process finished
Finding VCD file...
./dump.vcd
[2021-02-22 13:24:38 EST] Opening EPWave...
Done
```

### *EDA playgound: https://www.edaplayground.com/x/G8z2 *