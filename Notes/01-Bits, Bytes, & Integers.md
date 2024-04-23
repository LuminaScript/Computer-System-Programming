#### I. **Representing information as bits**

- **Byte = 8 bits**

  - <u>Binary</u>: 00000000 ~ 11111111
  - <u>Decimal</u>: 0 ~ 255
  - <u>Hexadecimal</u>: 0x00 ~ 0xFF (Base 16 Number representation)

  | Hex  | Decimal | Binary |
  | ---- | ------- | ------ |
  | 0    | 0       | 0000   |
  | 1    | 1       | 0001   |
  | 2    | 2       | 0010   |
  | 3    | 3       | 0011   |
  | 4    | 4       | 0100   |
  | 5    | 5       | 0101   |
  | 6    | 6       | 0110   |
  | 7    | 7       | 0111   |
  | 8    | 8       | 1000   |
  | 9    | 9       | 1001   |
  | A    | 10      | 1010   |
  | B    | 11      | 1011   |
  | C    | 12      | 1100   |
  | D    | 13      | 1101   |
  | E    | 14      | 1110   |
  | F    | 15      | 1111   |

- **Example Data Representation**

  | C Data Type | Typical 32-bit | Typical 64-bit | x86-64 |
  | ----------- | -------------- | -------------- | ------ |
  | char        | 1              | 1              | 1      |
  | short       | 2              | 2              | 2      |
  | int         | 4              | 4              | 4      |
  | long        | 4              | 8              | 8      |
  | float       | 4              | 4              | 4      |
  | double      | 8              | 8              | 8      |
  | pointer     | 4              | 8              | 8      |



#### II. **Bit-level manipulations**

- **Boolean Algebra** 
  - <u>And</u>: A & B = 1 when A = 1 and B = 1
  - <u>Or</u>: A | B = 1 when wither A = 1 or B = 1
  - <u>Not</u>: ~A = 1 when A = 0
  - <u>Xor</u> A^B = 1 when either A = 1 or B = 1, but not both
- **Operate on Bit Vectors**
  - <u>Operations applied bitwise</u>: 01101001 & 0101010 = 0100001

- **Representing & Manipulating Sets** 
  - <u>SET</u>: {76543210}
  - <u>Representation</u>: 
    - 01101001 -> 0356
    - 01010101 -> 0246
  - <u>Operation</u>:
    - &
    - |
    - ^
    - ~
- **Bit-Level Operations in C**
  - ~0x41 -> ~0100 0001 -> 1011 1110 -> 0xBE
- **Logic Operations in C**
  - <u>Rules</u>:
    - 0 is False
    - Anything nonzeror is True
    - Alwasy return 0 or 1
    - Early termination
  - <u>Examples</u>:
    - ~0x41 -> 0x00
    - !0x00 -> 0x01
    - !!0x41 -> 0x01
    - p && *p (avoids null pointer access) (Early termination)
- **Shift Operations**
  - <u>Left shift</u>: ```x << y```
    - Fill with 0's on right
  - <u>Right shift</u>: ```x >> y```
    - Logical shift: fill with 0's on left ```10100010b >> 2 -> 00101000```
    - Arithmetic shift: replicate most significant bit on left ```10100010b >> 2 -> 11101000```
  - <u>Undefined behaviorus</u>:
    - Shift amount < 0 or >= word size



#### III. **Integers**

* **Representation: unsigned and signed**

  * <u>Unsigned</u> 
    * Numeric Range: **0** (000...0) ~ **2<sup>w</sup> - 1**(111...1)
  * <u>Signed / two's complements</u> 
    * Numeric Range: **-2<sup>w - 1</sup>** (100...0)  ~  **2<sup>w - 1</sup> - 1**(011...1)
    * Examples:
      * ```10 = 0(-16)  1(8)  0(4)  1(2)   0(1) = 8 + 2 = 10```
      * ```-10 = 1(-16)  0(8)  1(4)  1(2)   0(1) = -16 + 4 + 2 = -10```

  - <u>Observatoion</u>

    -  UMAX = 2 * TMAX + 1
    - |TMin| = TMAX + 1
    - C Programming: ```#include <limits.h>```

    | W    | 8    | 16      | 32             | 64                         |
    | ---- | ---- | ------- | -------------- | -------------------------- |
    | UMax | 255  | 65,535  | 4,294,967,295  | 18,446,744,073,709,551,615 |
    | TMax | 127  | 32,767  | 2,147,483,647  | 9,223,372,036,854,775,807  |
    | TMin | -128 | -32,768 | -2,147,483,648 | -9,223,372,036,854,775,808 |

- **Conversion, casting**

  - <u>Basic rules</u>: bit pattern are maintained but reinterpreted

  - <u>Example</u>:

    ```c
    int x = -1; // 11111111 11111111 11111111 11111111
    unsigned int u = (unsigned int) x; // x = INT_MAX
    ```

  - <u>Signed vs Unsigend in C</u>

    - Constants:
      - Default: considered to be signed integers
      - unsigned: 0U, 314134U
    - Comparison
      - Both signed
      - Either one of them unsigned: converted all to unsigned: 1 (converted to INT_MAX) > 0U == true

- **Expanding, truncating**

  - <u>Sign Extension</u> (two 's complement)

    - make it bigger without changing the numeric value

      - ```6 : 0 1 1 0 -> 0 0 1 1 0```

      - ```-2 : 1(-8) 1(4) 1(2) 0(0) -> 1(-16) 1(8) 1(4) 1(2) 0(0)```
      - ```ffff0123 == f0123```

  - S<u>ign truncation (two 's complement)</u>

    - unsigned: drop MSB: ```1 1 0 1 1 -> 1 0 1 1 (27 mod 16 = 11)```
    - signed: ```1 0 0 1 1 -> 1 0 1 1```

- **Addition, negation, multiplication, shifting § Summary**

  - <u>Addition</u>

    - <u>Unsigned</u>

      - Ignores carry output: s = UAdd(u, v) = (u + v) mod 2<sup>W</sup>

    - <u>Signed in C</u>

      - Ignores carry outputs 

        | u + v + 2^w | If u + v < TMin_w (NegOver)  |
        | ----------- | ---------------------------- |
        | u + v       | If TMin_w <= u + v <= TMax_w |
        | u + v - 2^w | If TMax_w < u + v (PosOver)  |

  - <u>Multiplication</u>

    - Unsigned in C: UMul<sub>w</sub>a(u, v) = (u * v) mod 2<sup>W</sup>
    - Signed in C: Ignore High order w bits 
      - Some of high bits differ, but lower bits are the same

  - <u>Shifting</u> 

    - Multiply with shift

      - Signed & Unsigned: u << k  = u * 2<sup>k</sup>
      - Discard k bits for w + b bits.
      - ```u << 3 == u * 8```
      - ```(u << 5) - (u << 3) = u * 24```

    - Divide with shift

      - Unsigned (logical shift): u >> k == u / 2<sup>k</sup>
      - Signed (Arithmetic shift)
      - Ceil

    - **Negation: Complement & Increment**

      - ```~x + 1 == -x```

        - For `x = 0`:

          | Decimal | Hex   | Binary            |
          | ------- | ----- | ----------------- |
          | 0       | 00 00 | 00000000 00000000 |
          | -1      | FF FF | 11111111 11111111 |
          | 0       | 00 00 | 00000000 00000000 |

        - For `x = TMin`:

          | Decimal | Hex   | Binary            |
          | ------- | ----- | ----------------- |
          | -32768  | 80 00 | 10000000 00000000 |
          | 32767   | 7F FF | 01111111 11111111 |
          | -32768  | 80 00 | 10000000 00000000 |

- **When to use unsigned?**

  - <u>Loop count</u>

    ```c
    unsigned i;
    for(i = cnt - 2; i < cnt; i--) { // if i < 0, i becomes INT_MAX
      //
    }
    
    size_t i;
    // size_t data type defined as unsigned value with length = word size 
    ```

  - <u>Modular Arithmetic: multiprecision arithmetic</u>

  - <u>Using bits to represent Sets</u> (logical rightshift, no sign extension)

  

#### IV. **Representations in memory, pointers, strings**

- **Byte-oriented Memory Organization**: 

  - Program/process refer to as a big array of bytes
  - System provides private address spaces to each Process

- **Machine Words**

  - Machine word = size of addresses on machine
  - 4 bytes (32-bit) & 8 bytes (64-bit)

- **Byte Ordering**

  - Big endian (LSB has highest address): Internet 
  - Small endian (LSB has lowest addresses): x86, ARM, IOS, Linux
  - Example:
    - Small Endian: 0000 0001
    - Big Endian: 1000 0000

- **Pointers**: Index into array of virtual memory

- **Strings**: Array of chars

  

#### V. Integer C Puzzles

```c
int x = foo();
int y = bar();
unsigned ux = x;
unsigned uy = y;
```

- **False**: ```x < 0```  =>  ``` (x * 2) < 0``` 
  - counter-example: 1000 000
- **True**: ```x & 7 == 7```  =>  ```(x << 30) < 0```
- **False**: ```ux > -1``` 
  - it is an **unsigned comparison**
  - ux > FFFF....F (TMax) -> false for any ux
- **False**: ```x > y```  =>  ```-x < y```
  - Counter-example: y = TMin, -y = Tmin -> no value is less than Tmin
- **False**: ```x * x >= 0```  =>  ```x + y > 0```
  - Counte-example: Overflow
- **True**: ```x >= 0```  =>  ```-x <= 0```
- **False**: ```(x | -x) >> 31 == -1```
  - Counter-exmaple: x = 0



