# *Digital-electronics-1*

## *Laborotory 7*

[My Digital-electronics-1 git repository](https://github.com/shad0w3y3/Digital-electronics-1)

## *1. Preparation tasks*

 D | Qn | Q(n+1) | Comments
---|---|---|---
0 | 0 | 1 | 
0 | 1 | 0 | 
1 | 0 | 0 | 
1 | 1 | 1 | 

 J | K | Qn | Q(n+1) | Comments
---|---|---|---|---
0 | 0 | 0 | 0 | qn+1 = 0 · qn + 1 · qn = qn
0 | 0 | 1 | 1 | qn+1 = 0 · qn + 1 · qn = qn
0 | 1 | 0 | 0 | qn+1 = 0 · qn + 0 · qn = 0
0 | 1 | 1 | 0 | qn+1 = 0 · qn + 0 · qn = 0
1 | 0 | 0 | 1 | qn+1 = 1 · qn + 1 · qn = 1
1 | 0 | 1 | 1 | qn+1 = 1 · qn + 1 · qn = 1
1 | 1 | 0 | 1 | qn+1 = 1 · qn + 0 · qn = qn
1 | 1 | 1 | 0 | qn+1 = 1 · qn + 0 · qn = qn


 T | Qn | Q(n+1) | Comments
---|---|---|---
0 | 0 | 0 | No change
0 | 1 | 1 | No change
1 | 0 | 1 | Invert
1 | 1 | 0 | Invert


## *2. D laches*

#### *VHDL code of D latch architecture*

``` VHDL
----------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
-- 
-- Create Date: 03/24/2021 11:14:43 AM
-- Design Name: 
-- Module Name: d_latch - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
-- 
-- Dependencies: 
-- 
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
-- 
----------------------------------------------------------------------------------


library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;

entity d_latch is
    Port (
    
           en :     in STD_LOGIC;
           d :      in STD_LOGIC;
           arst :   in STD_LOGIC;
           q :      out STD_LOGIC;
           q_bar :  out STD_LOGIC
           
         );
end d_latch;

architecture Behavioral of d_latch is
begin
       p_d_latch : process (en, d, arst)
    begin
        if (arst = '1') then
           q     <= '0';
           q_bar <= '1';
           
        elsif (en <= '1') then
           q     <= d; 
           q_bar <= not d;
        
        end if;       

    end process p_d_latch;
end Behavioral;
```

#### *VHDL code of p_stimulus*

``` VHDL

```

#### *Simulation screenshot*

![Image]()

#### *VHDL of top architecture*

``` VHDL

```

## *3. Stopwatch with minutes*

#### *Hand-drawn sketch of stopwatch schematic*

![Image]()