# *Digital-electronics-1*

## *Laborotory 2*

[My Digital-electronics-1 git repository](https://github.com/shad0w3y3/Digital-electronics-1)

## *1. 2-bit comparator truth table*

  Dec. equivalent    |    B[1:0]   |    A[1:0]   |    B is greater than A   |    B equals A   |    B is less then A 
---------------------|-------------|-------------|--------------------------|-----------------|--------------------
        0            |     0 0     |      0 0    |            0             |        1        |           0
        1            |     0 0     |      0 1    |            0             |        0        |           1
        2            |     0 0     |      1 0    |            0             |        0        |           1
        3            |     0 0     |      1 1    |            0             |        0        |           1
        4            |     0 0     |      0 0    |            0             |        1        |           0
        5            |     0 0     |      0 1    |            0             |        0        |           1
        6            |     0 0     |      1 0    |            0             |        0        |           1
        7            |     0 0     |      1 1    |            0             |        0        |           1
        8            |     0 0     |      0 0    |            0             |        1        |           0
        9            |     0 0     |      0 1    |            0             |        0        |           1
        10           |     0 0     |      1 0    |            0             |        0        |           1
        11           |     0 0     |      1 1    |            0             |        0        |           1
        12           |     0 0     |      0 0    |            0             |        1        |           0
        13           |     0 0     |      0 1    |            0             |        0        |           1
        14           |     0 0     |      1 0    |            0             |        0        |           1
        15           |     0 0     |      1 1    |            0             |        0        |           1 



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







### *Testbench code:*




### *Listing of simulation console output:*