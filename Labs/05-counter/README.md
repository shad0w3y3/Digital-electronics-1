# *Digital-electronics-1*

## *Laborotory 5*

[My Digital-electronics-1 git repository](https://github.com/shad0w3y3/Digital-electronics-1)

## *1. Preparation tasks*

#### *Connections of push buttons*

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/05-counter/Pictures/Schematic%20of%20buttons.png)

#### *Table with calculated values*

  Time interval | Number of clk periods | Number of clk periods | Number of clk periods in binary
---|---|---|---
2 ms | 200 000 | x"3_0D40" | b"0011_0000_1101_0100_0000"
4 ms | 400 000 | x"61A80" | b"0110_0001_1010_1000_0000"
10 ms | 1 000 000 | x"F4240" | b"1111_0100_0010_0100_0000" 
250 ms | 25 000 000 | x"17D7840" | b"0001_0111_1101_0111_1000_0100_0000" 
500 ms | 50 000 000 | x"2FAF080" | b"0010_1111_1010_1111_0000_1000_0000" 
1 sek | 100 000 000 | x"5F5E100" | b"0101_1111_0101_1110_0001_0000_0000" 


## *2. Bidirectional counter*

#### *VHDL code of the process p_cnt_up_down*

``` VHDL
    p_cnt_up_down : process(clk)
    begin
        if rising_edge(clk) then
        
            if (reset = '1') then               -- Synchronous reset
                s_cnt_local <= (others => '0'); -- Clear all bits

            elsif (en_i = '1' AND cnt_up_i = '1') then       --adds 1
                s_cnt_local <= s_cnt_local + 1;

                -- TEST COUNTER DIRECTION HERE

             elsif (en_i = '1' AND cnt_down_i = '1') then       --subtracts 1
                s_cnt_local <= s_cnt_local - 1;


            end if;
        end if;
    end process p_cnt_up_down;
```

#### *VHDL reset and stimulus processes from testbench file tb_cnt_up_down.vhd*

``` VHDL
    --------------------------------------------------------------------
    -- Reset generation process
    --------------------------------------------------------------------
    p_reset_gen : process
    begin
        s_reset <= '0';
        wait for 12 ns;
        
        s_reset <= '1';                 -- Reset activated
        wait for 73 ns;
        
        s_reset <= '0';
        wait;
    end process p_reset_gen;

    --------------------------------------------------------------------
    -- Data generation process
    --------------------------------------------------------------------
    p_stimulus : process
    begin
        report "Stimulus process started" severity note;

        s_en     <= '1';                -- Enable counting
        
        s_cnt_up <= '1';
        wait for 380 ns;                -- Change counter direction
        s_cnt_up <= '0';
        wait for 220 ns;
        
        
        s_en     <= '0';                -- Disable counting

        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;
```

#### *Simulated time waveform, all inputs and outputs*

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/05-counter/Pictures/wave1.png)

## *3. Top level*

#### *VHDL code from source file top.vhd*

``` VHDL

```

#### *Image of the top layer*

![Image]()