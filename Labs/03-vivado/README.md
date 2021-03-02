# *Digital-electronics-1*

## *Laborotory 3*

[My Digital-electronics-1 git repository](https://github.com/shad0w3y3/Digital-electronics-1)

## *1. Preparation tasks*

### 16 slide switches and 16 LEDs on Nexys A7 board



## *2. 2-bit wide 4-to-1 multiplexer*

### *VHDL design*

``` VHDL
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity mux_2bit_4to1 is
    port(
        a_i     : in  std_logic_vector(2 - 1 downto 0);
        b_i     : in  std_logic_vector(2 - 1 downto 0);
        c_i     : in  std_logic_vector(2 - 1 downto 0);
        d_i     : in  std_logic_vector(2 - 1 downto 0);
        sel_i   : in  std_logic_vector(2 - 1 downto 0);
        f_o     : out std_logic_vector(2 - 1 downto 0)
    );
end mux_2bit_4to1;

architecture Behavioral of mux_2bit_4to1 is
begin
    f_o <= a_i when (sel_i = "00") else
           b_i when (sel_i = "01") else
           c_i when (sel_i = "10") else
           d_i; 
    
end architecture Behavioral;
```

### *VHDL testbench*

``` VHDL
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity tb_mux_2bit_4to1 is
    -- Entity of testbench is always empty
end tb_mux_2bit_4to1;

architecture Behavioral of tb_mux_2bit_4to1 is

    -- Local signals
    signal s_a      : std_logic_vector(2 - 1 downto 0);
    signal s_b      : std_logic_vector(2 - 1 downto 0);
    signal s_c      : std_logic_vector(2 - 1 downto 0);
    signal s_d      : std_logic_vector(2 - 1 downto 0);
    signal s_sel    : std_logic_vector(2 - 1 downto 0);
    signal s_f      : std_logic_vector(2 - 1 downto 0);
        
begin

    uut_mux_2bit_4tol : entity work.mux_2bit_4to1
        port map(
            a_i   => s_a,
            b_i   => s_b,
            c_i   => s_c,
            d_i   => s_d,
            sel_i => s_sel,
            f_o   => s_f
        );

    p_stimulus : process
    begin
        
        report "Stimulus process started" severity note;
        
        s_d <= "11"; s_c <= "10"; s_b <= "01"; s_a <= "00";
        s_sel <= "00"; wait for 100 ns;
        s_sel <= "01"; wait for 100 ns;
        s_sel <= "10"; wait for 100 ns;
        s_sel <= "11"; wait for 100 ns; 
    end process p_stimulus; 
        
end architecture Behavioral;
```

## *3. Personal Vivado tutorial*

### *Tutorial how to start a new project and how to create XDC files with constraints and how to run a simulation*

### Step 1: Click "Create Project"

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/1.png)

### Step 2: Click "Next"

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/2.png)

### Step 3: Name your project, select where does the project save and click "Next"

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/3.png)

### Step 4: Select the "RTL Project" and click "Next"

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/4.png)

### Step 5: Click "Create File"

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/5.png)

### Step 6: Select the "File Type" to be "VHDL" and name your file, then click "OK"

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/6.png)

### Step 7: Select both languages to be "VHDL" and click "Next"

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/7.png)

### Step 8: Click "Create File"

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/8.png)

### Step 9: Write the according file name you need for the constraint and click "OK"

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/9.png)

### Step 10: Click "Next"

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/10.png)

### Step 11: Click "Boards" on the top left of the window

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/11.png)

### Step 12: Search from the search bar what board you need to find, click on it and click "Next

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/12.png)

### Step 13: Click "Finish"

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/13.png)

### Step 14: Type the main entity name you have in your VHDL code and click "OK"

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/14.png)

### Step 15: Click "Yes"

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/15.png)

### Step 16: Click "File" on the top left and then from the dropdown menu click add sources if you have or need them

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/16.png)

### Step 17: Select "Add or create simulation sources" and click "Next"

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/17.png)

### Step 18: Click "Create File"

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/18.png)

### Step 19: Select VHDL for file type, write a testbench file name and click "OK" and then "Finish

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/19.png)

### Step 20: Select "Run Behavioral Simulation" on the left under "Run Simulation"

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/03-vivado/Pictures/20.png)