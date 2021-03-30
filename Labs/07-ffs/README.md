# *Digital-electronics-1*

## *Laborotory 7*

[My Digital-electronics-1 git repository](https://github.com/shad0w3y3/Digital-electronics-1)

## *1. Preparation tasks*

clk | D | Qn | Q(n+1) | Comments
---|---|---|---|---
cnt up | 0 | 0 | 0 | stores
cnt up | 0 | 1 | 0 | stores
cnt up | 1 | 0 | 1 | stores
cnt up | 1 | 1 | 1 | stores

clk | J | K | Qn | Q(n+1) | Comments
---|---|---|---|---|---
cnt up | 0 | 0 | 0 | 0 | No change
cnt up | 0 | 0 | 1 | 1 | No change
cnt up | 0 | 1 | 0 | 0 | Reset
cnt up | 0 | 1 | 1 | 0 | Reset
cnt up | 1 | 0 | 0 | 1 | Set
cnt up | 1 | 0 | 1 | 1 | Set
cnt up | 1 | 1 | 0 | 1 | Invert
cnt up | 1 | 1 | 1 | 0 | Invert


clk | T | Qn | Q(n+1) | Comments
---|---|---|---|---
cnt up | 0 | 0 | 0 | No change
cnt up | 0 | 1 | 1 | No change
cnt up | 1 | 0 | 1 | Invert
cnt up | 1 | 1 | 0 | Invert


## *2. D laches*

#### *VHDL code of p_d_latch*

``` VHDL
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
```

#### *VHDL code of tb_d_latch*

``` VHDL
 p_reset_gen : process
 begin

	 s_arst  <= '0';
	 wait for 38 ns;
	 
	 -- Reset on
	 s_arst  <= '1';
	 wait for 53 ns;

	 --Reset off
	 s_arst  <= '0';
	
	 wait for 80 ns;
	 s_arst  <= '1';

	 wait;

 end process p_reset_gen;

p_stimulus : process
begin

	report "Stimulus process started" severity note;
	
	s_d   <= '0';
	s_en  <= '0';
	
	wait for 10 ns;
	s_d  <= '1';
	wait for 10 ns;
	s_d  <= '0';
	wait for 10 ns;
	s_d  <= '1';
	wait for 10 ns;
	s_d  <= '0';
	wait for 5 ns;
	
	assert ((s_arst = '1') and (s_en = '0') and (s_q = '0') and (s_q_bar = '1'))
	report "Test failed on reset high, en low when s_d = '0'" severity error;
	
	wait for 5 ns;
	s_d    <= '1';
	wait for 5 ns;
	
	assert ((s_arst = '1') and (s_en = '0') and (s_q = '0') and (s_q_bar = '1'))
	report "Test failed on reset high, en low when s_d = '1'" severity error;
	
	wait for 5 ns;
	s_d  <= '0';
	
	s_en <= '1';
	
	wait for 10 ns;
	s_d  <= '1';
	wait for 5 ns;
	
	assert ((s_arst = '1') and (s_en = '1') and (s_q = '0') and (s_q_bar = '1'))
	report "Test failed on reset high, en high when s_d = '1'" severity error;
	
	wait for 5 ns;
	s_d  <= '0';
	wait for 5 ns;
	
	assert ((s_arst = '1') and (s_en = '1') and (s_q = '0') and (s_q_bar = '1'))
	report "Test failed on reset high, en high when s_d = '0'" severity error;          
	
	wait for 5 ns;
	s_d  <= '1';
	wait for 10 ns;
	s_d  <= '0';
	wait for 10 ns;
	s_d  <= '1';
	wait for 5 ns;
	
	assert ((s_arst = '0') and (s_en = '1') and (s_q = '1') and (s_q_bar = '0'))
	report "Test failed on reset low, en high when s_d = '1'" severity error;
	
	wait for 15 ns;
	s_d  <= '0';
	wait for 5 ns;
	
	assert ((s_arst = '0') and (s_en = '1') and (s_q = '0') and (s_q_bar = '1'))
	report "Test failed on reset low, en high when s_d = '0'" severity error;
	
	wait for 5 ns;
	s_d  <= '1';
	wait for 5 ns;
	s_en <= '0';
	wait for 5 ns;
	s_d  <= '0';

	wait for 10 ns;
	s_d  <= '1';
	wait for 10 ns;
	s_d  <= '0';
	wait for 10 ns;
	s_d  <= '1';
	wait for 10 ns;
	s_d  <= '0';

	report "Stimulus process finished" severity note;

	wait;

end process p_stimulus;
```

#### *Waveform*

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/07-ffs/Pictures/wave1.png)

## *3. Flip-flops*

#### *VHDL codes of d_ff_arst, d_ff_rst, jk_ff_rst, t_ff_rst*

##### *d_ff_arst*
``` VHDL
  p_d_ff_arst : process (clk, arst)
    begin

        if (arst = '1') then
           q     <= '0';
           q_bar <= '1';
           
        elsif rising_edge(clk) then
           q     <= d; 
           q_bar <= not d;
        end if;     

    end process p_d_ff_arst;
```

##### *d_ff_rst*
``` VHDL
p_d_ff_rst : process (clk)
begin

	if rising_edge(clk) then
		if (rst = '1') then
			q     <= '0';
			q_bar <= '1';
		else
			q     <= d;
			q_bar <= not d;
		end if;
	end if;

end process p_d_ff_rst;
```
##### *jk_ff_rst*
``` VHDL
p_jk_ff_rst : process (clk)
begin

    if rising_edge(clk) then
        if (rst = '1') then
            s_q <= '0';
        else
            if    (j = '0' and k = '0') then
                s_q <= s_q;
            elsif (j = '0' and k = '1') then
                s_q <= '0';
            elsif (j = '1' and k = '0') then
                s_q <= '1';
            elsif (j = '1' and k = '1') then
                s_q <= not s_q;
            
            end if;
        end if;
    end if;

end process p_jk_ff_rst;

q     <= s_q;
q_bar <= not s_q;
```

##### *t_ff_rst*
``` VHDL
p_t_ff_rst : process (clk)
begin

    if rising_edge(clk) then
        if  (rst = '1') then
            s_q <= '0';
        elsif (t = '1') then
            s_q <= not s_q;
        end if;
    end if;

end process p_t_ff_rst;

q     <= s_q;
q_bar <= not s_q;
```

#### *VHDL tb codes*

##### *tb_d_ff_arst*
``` VHDL
p_clk_gen : process
    begin

        while now < 40 ms loop        
            s_clk_100MHz <= '0';
            wait for c_CLK_100MHZ_PERIOD / 2;
            s_clk_100MHz <= '1';
            wait for c_CLK_100MHZ_PERIOD / 2;
        end loop;
        wait;

    end process p_clk_gen;

     p_reset_gen : process
        begin
            s_arst  <= '0';
            wait for 28 ns;
            
            -- Reset on
            s_arst  <= '1';
            wait for 13 ns;
    
            --Reset off
            s_arst  <= '0';
            wait for 17 ns;
            
            s_arst  <= '1';
            wait for 33 ns;
            
            wait for 660 ns;
            s_arst   <= '1';
    
            wait;
     end process p_reset_gen;

    p_stimulus : process
    begin

        report "Stimulus process started" severity note;
        
        s_d  <= '0';
        
        wait for 14 ns;
        s_d  <= '1';
        wait for 2 ns;
        
        assert ((s_arst = '0') and (s_q = '1') and (s_q_bar = '0'))
        report "Test failed on reset low, after clk rising when s_d = '1'" severity error;
        
        wait for 8 ns;
        s_d  <= '0';
        wait for 6 ns;
        
        wait for 4 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 5 ns;
        
        assert ((s_arst = '1') and (s_q = '0') and (s_q_bar = '1'))
        report "Test failed on reset high, before clk rising when s_d = '1'" severity error;
        
        wait for 5 ns;
        s_d  <= '0';

        wait for 14 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';

        report "Stimulus process finished" severity note;
        wait;

    end process p_stimulus;
```

##### *tb_d_ff_rst*
``` VHDL
p_clk_gen : process
begin

	while now < 40 ms loop        
		s_clk_100MHz <= '0';
		wait for c_CLK_100MHZ_PERIOD / 2;
		s_clk_100MHz <= '1';
		wait for c_CLK_100MHZ_PERIOD / 2;
	end loop;
	wait;

end process p_clk_gen;

 p_reset_gen : process
	begin

		s_rst   <= '0';
		wait for 28 ns;
		
		-- Reset on
		s_rst   <= '1';
		wait for 13 ns;

		--Reset off
		s_rst   <= '0';
		wait for 17 ns;
		
		s_rst   <= '1';
		wait for 33 ns;
		
		wait for 660 ns;
		s_rst    <= '1';

		wait;

 end process p_reset_gen;

p_stimulus : process
begin

	report "Stimulus process started" severity note;
	
	s_d  <= '0';
	
	wait for 14 ns;
	s_d  <= '1';
	wait for 2 ns;
	
	assert ((s_rst = '0') and (s_q = '1') and (s_q_bar = '0'))
	report "Test failed on reset low, after clk rising when s_d = '1'" severity error;
	
	wait for 8 ns;
	s_d  <= '0';
	wait for 6 ns;
	
	wait for 4 ns;
	s_d  <= '1';
	wait for 10 ns;
	s_d  <= '0';
	wait for 10 ns;
	s_d  <= '1';
	wait for 5 ns;

	assert ((s_rst = '1') and (s_q = '1') and (s_q_bar = '0'))
	report "Test failed on reset high, before clk rising when s_d = '1'" severity error;
	
	wait for 5 ns;
	s_d  <= '0';

	wait for 14 ns;
	s_d  <= '1';
	wait for 10 ns;
	s_d  <= '0';
	wait for 10 ns;
	s_d  <= '1';
	wait for 10 ns;
	s_d  <= '0';
	wait for 10 ns;
	s_d  <= '1';
	wait for 10 ns;
	s_d  <= '0';

	report "Stimulus process finished" severity note;
	wait;

end process p_stimulus;
```

##### *tb_jk_ff_rst*
``` VHDL
p_clk_gen : process
begin

	while now < 40 ms loop        
		s_clk_100MHz <= '0';
		wait for c_CLK_100MHZ_PERIOD / 2;
		s_clk_100MHz <= '1';
		wait for c_CLK_100MHZ_PERIOD / 2;
	end loop;
	wait;

end process p_clk_gen;

 p_reset_gen : process
	begin
		s_rst   <= '0';
		wait for 18 ns;
		
		-- Reset on
		s_rst   <= '1';
		wait for 13 ns;

		--Reset off
		s_rst   <= '0';
		wait for 47 ns;
		
		s_rst   <= '1';
		wait for 33 ns;
		
		wait for 660 ns;
		s_rst    <= '1';

		wait;

 end process p_reset_gen;

p_stimulus : process
begin

	report "Stimulus process started" severity note;
	
	s_j  <= '0';
	s_k  <= '0';
	
	wait for 38 ns;
	
	assert ((s_rst = '0') and (s_j = '0') and (s_k = '0') and (s_q = '0') and (s_q_bar = '1'))
	report "Test 'no change' failed on reset low, after clk rising when s_j = '0' and s_k = '0'" severity error;
	
	wait for 2 ns;
	s_j  <= '1';
	s_k  <= '0';
	wait for 6 ns;
	
	assert ((s_rst = '0') and (s_j = '1') and (s_k = '0') and (s_q = '1') and (s_q_bar = '0'))
	report "Test 'set' failed on reset low, after clk rising when s_j = '1' and s_k = '0'" severity error;
	
	wait for 1 ns;
	s_j  <= '0';
	s_k  <= '1';
	wait for 13 ns;
	
	assert ((s_rst = '0') and (s_j = '0') and (s_k = '1') and (s_q = '0') and (s_q_bar = '1'))
	report "Test 'reset' failed on reset low, after clk rising when s_j = '0' and s_k = '1'" severity error;
	
	wait for 1 ns;
	s_j  <= '1';
	s_k  <= '0';

	wait for 7 ns;
	s_j  <= '1';
	s_k  <= '1';
	
	wait for 8 ns;
	
	assert ((s_rst = '0') and (s_j = '1') and (s_k = '1') and (s_q = '0') and (s_q_bar = '1'))
	report "Test 'toggle' failed on reset low, after clk rising when s_j = '1' and s_k = '1'" severity error;
	
	wait for 2 ns;
	s_j  <= '0';
	s_k  <= '0';

	wait for 7 ns;
	s_j  <= '0';
	s_k  <= '1';

	wait for 7 ns;
	s_j  <= '1';
	s_k  <= '0';

	wait for 7 ns;
	s_j  <= '1';
	s_k  <= '1';

	report "Stimulus process finished" severity note;
	wait;

end process p_stimulus;
```

##### *tb_t_ff_rst*
``` VHDL
p_clk_gen : process
begin

	while now < 40 ms loop        
		s_clk_100MHz <= '0';
		wait for c_CLK_100MHZ_PERIOD / 2;
		s_clk_100MHz <= '1';
		wait for c_CLK_100MHZ_PERIOD / 2;
	end loop;
	wait;

end process p_clk_gen;

 p_reset_gen : process
	begin

		s_rst   <= '0';
		wait for 18 ns;
		
		-- Reset on
		s_rst   <= '1';
		wait for 13 ns;

		--Reset off
		s_rst   <= '0';
		wait for 47 ns;
		
		s_rst   <= '1';
		wait for 33 ns;
		
		wait for 660 ns;
		s_rst    <= '1';

		wait;

 end process p_reset_gen;

p_stimulus : process
begin

	report "Stimulus process started" severity note;
	
	s_t  <= '0';
	
	wait for 38 ns;
	
	assert ((s_rst = '0') and (s_t = '0') and (s_q = '0') and (s_q_bar = '1'))
	report "Test 'no change' failed on reset low, after clk rising when s_t = '0'" severity error;
	
	wait for 2 ns;
	s_t  <= '1';
	wait for 6 ns;
	
	assert ((s_rst = '0') and (s_t = '1') and (s_q = '1') and (s_q_bar = '0'))
	report "Test 'toggle' failed on reset low, after clk rising when s_t = '1'" severity error;
	
	wait for 1 ns;
	s_t  <= '0';
	wait for 13 ns;
	
	assert ((s_rst = '0') and (s_t = '0') and (s_q = '1') and (s_q_bar = '0'))
	report "Test 'no change' failed on reset low, after clk rising when s_t = '0'" severity error;
	
	wait for 1 ns;
	s_t  <= '1';
	wait for 5 ns;
	
	assert ((s_rst = '0') and (s_t = '1') and (s_q = '0') and (s_q_bar = '1'))
	report "Test 'toggle' failed on reset low, after clk rising when s_t = '1'" severity error;
	
	wait for 12 ns;
	s_t  <= '0';

	wait for 7 ns;
	s_t  <= '1';

	wait for 7 ns;
	s_t  <= '0';

	wait for 7 ns;
	s_t  <= '1';
	
	report "Stimulus process finished" severity note;
	wait;

end process p_stimulus;
```

#### *Waveforms*

##### *tb_d_ff_arst*
![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/07-ffs/Pictures/wave2.png)

##### *tb_d_ffarst*
![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/07-ffs/Pictures/wave3.png)

##### *tb_jk_ff_rst*
![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/07-ffs/Pictures/wave4.png)

##### *tb_t_ff_rst*
![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/07-ffs/Pictures/wave5.png)

## *4. Shift register schematic*

##### TOP
![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/07-ffs/Pictures/schematic.png)