# *Digital-electronics-1*

## *Laborotory 8*

[My Digital-electronics-1 git repository](https://github.com/shad0w3y3/Digital-electronics-1)

## *1. Preparation tasks*

#### *State table*

Input P | 0 | 0 | 1 | 1 | 0 | 1 | 0 | 1 | 1 | 1 | 1 | 0 | 0 | 1 | 1 | 1 
---|---|---|---|---|--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- 
Clock | up | up | up | up | up | up | up | up | up | up | up | up | up | up | up | up 
State | A | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 
Output R | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 

#### *RGB LED connections*

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/08-traffic_lights/Pictures/schematic.png)

#### *Table with colour settings*

LED | Artix-7 pins | Red | Yellow | Green 
---|---|---|---|---
LD16 | N15, M16, R12 | 1,0,0 | 1,1,0 | 0,1,0
LD17 | N16, R11, G14 | 1,0,0 | 1,1,0 | 0,1,0 

## *2. TLC*

#### *State diagram*

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/08-traffic_lights/Pictures/s1.jpg)

#### *VHDL code of p_traffic_fsm*

``` VHDL
p_traffic_fsm : process(clk)
    begin
        if rising_edge(clk) then
            if (reset = '1') then       -- Synchronous reset
                s_state <= STOP1 ;      -- Set initial state
                s_cnt   <= c_ZERO;      -- Clear all bits

            elsif (s_en = '1') then
                -- Every 250 ms, CASE checks the value of the s_state 
                -- variable and changes to the next state according 
                -- to the delay value.
                case s_state is

                    -- If the current state is STOP1, then wait 1 sec
                    -- and move to the next GO_WAIT state.
                    when STOP1 =>
                        -- Count up to c_DELAY_1SEC
                        if (s_cnt < c_DELAY_1SEC) then
                            s_cnt <= s_cnt + 1;
                        else
                            -- Move to the next state
                            s_state <= WEST_GO;
                            -- Reset local counter value
                            s_cnt   <= c_ZERO;
                        end if;

                    when WEST_GO =>
                    -- Count up to 4sek
                         if (s_cnt < c_DELAY_4SEC) then
                            s_cnt <= s_cnt + 1;
                        else
                            -- Move to the next state
                            s_state <= WEST_WAIT;
                            -- Reset local counter value
                            s_cnt   <= c_ZERO;
                        end if;
                        
                    when WEST_WAIT =>
                    
                         if (s_cnt < c_DELAY_2SEC) then
                            s_cnt <= s_cnt + 1;
                        else
                            -- Move to the next state
                            s_state <= STOP2;
                            -- Reset local counter value
                            s_cnt   <= c_ZERO;
                        end if;
                        
                    when STOP2 =>
                    
                         if (s_cnt < c_DELAY_1SEC) then
                            s_cnt <= s_cnt + 1;
                        else
                            -- Move to the next state
                            s_state <= SOUTH_GO;
                            -- Reset local counter value
                            s_cnt   <= c_ZERO;
                        end if;
                    
                    when SOUTH_GO =>
                    
                         if (s_cnt < c_DELAY_4SEC) then
                            s_cnt <= s_cnt + 1;
                        else
                            -- Move to the next state
                            s_state <= SOUTH_WAIT;
                            -- Reset local counter value
                            s_cnt   <= c_ZERO;
                        end if;
                    
                    when SOUTH_WAIT =>
                    
                         if (s_cnt < c_DELAY_2SEC) then
                            s_cnt <= s_cnt + 1;
                        else
                            -- Move to the next state
                            s_state <= STOP1;
                            -- Reset local counter value
                            s_cnt   <= c_ZERO;
                        end if;
                        
                        
                    -- It is a good programming practice to use the 
                    -- OTHERS clause, even if all CASE choices have 
                    -- been made. 
                    when others =>
                        s_state <= STOP1;

                end case;
            end if; -- Synchronous reset
        end if; -- Rising edge
    end process p_traffic_fsm;
```

#### *VHDL code of p_output_fsm*

``` VHDL
p_output_fsm : process(s_state)
    begin
        case s_state is
            when STOP1 =>
                south_o <= "100";   -- Red
                west_o  <= "100";   -- Red
                
            when WEST_GO =>
                south_o <= "100";   -- Red
                west_o  <= "010";   -- green
                
            when WEST_WAIT =>
                south_o <= "100";   -- Red
                west_o  <= "110";   -- yellow
                
            when STOP2 =>
                south_o <= "100";   -- Red 
                west_o  <= "100";   -- Red
                
            when SOUTH_GO =>
                south_o <= "010";   -- green
                west_o  <= "100";   -- Red
            when SOUTH_WAIT =>
                south_o <= "110";   -- yellow
                west_o  <= "100";   -- Red

            when others =>
                south_o <= "100";  --red
                west_o  <= "100";  --red

        end case;
    end process p_output_fsm;
```

#### *Simulation of tlc*

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/08-traffic_lights/Pictures/wave1.png)

## *3. Smart controller*

#### *State table*

Current State | Direction South | Direction West | Delay | No Cars | Cars West | Cars South | Cars Both Directions 
---|---|---|---|---|---|---|---
goS | green | red | atleast 3 sec | goS | waitS | goS | waitS
waitS | yellow | red | 0.5 sec | goW | goW | goW | goW
goW | red | green | atleast 3 sec | goW | goW | waitW | waitW
waitW | red | yellow | 0.5 sec | goS | goS | goS | goS


#### *State diagram*

![Image](https://github.com/shad0w3y3/Digital-electronics-1/blob/main/Labs/08-traffic_lights/Pictures/s2.jpg)

#### *VHDL code of p_smart_traffic_fsm*

``` VHDL
p_smart_traffic_fsm : process(clk)
    begin
        if rising_edge(clk) then
            if (reset = '1') then       -- Synchronous reset
                s_state <= goS ;      -- Set initial state
                s_cnt   <= c_ZERO;      -- Clear all bits

            elsif (s_en = '1') then
                -- Every 250 ms, CASE checks the value of the s_state 
                -- variable and changes to the next state according 
                -- to the delay value.
                case s_state is

                    when goS =>
                        if (s_cnt < c_DELAY_3SEC) then
                            s_cnt <= s_cnt + 1;
                        elsif (west_e = '1') then
                            -- Move to the next state
                            s_state <= waitS;
                            -- Reset local counter value
                            s_cnt   <= c_ZERO;
                        end if;

                    when waitS =>
                         if (s_cnt < c_DELAY_0p5SEC) then
                            s_cnt <= s_cnt + 1;
                        else
                            -- Move to the next state
                            s_state <= goW;
                            -- Reset local counter value
                            s_cnt   <= c_ZERO;
                        end if;
                        
                    when goW =>
                    
                         if (s_cnt < c_DELAY_3SEC) then
                            s_cnt <= s_cnt + 1;
                        elsif (south_e = '1') then
                            -- Move to the next state
                            s_state <= waitW;
                            -- Reset local counter value
                            s_cnt   <= c_ZERO;
                        end if;
                        
                    when waitW =>
                    
                         if (s_cnt < c_DELAY_0p5SEC) then
                            s_cnt <= s_cnt + 1;
                        else
                            -- Move to the next state
                            s_state <= goS;
                            -- Reset local counter value
                            s_cnt   <= c_ZERO;
                        end if;
                    
                   
                    when others =>
                        s_state <= goS;

                end case;
            end if; -- Synchronous reset
        end if; -- Rising edge
    end process p_smart_traffic_fsm;
```