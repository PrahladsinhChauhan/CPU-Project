# CPU-Project
This CPU was made for University Project on traditional Logisim.<br>
It draws inspiration from “Computer System Architecture” by M. Morris Mano and Intel 8085 which were part of curriculum. However, certain features were removed and some were added from my side.<br>

Some notable features of this microprocessor are as follows:
<h2>LIDS</h2>
LIDS stand for Load, Increment, Decrement, Show<br>
I came up with this system when facing the problem of managing inputs and outputs of each register on Common Data Bus. As all 8bit registers also contribute to 16bit register pair, this problem grew larger. For two register, 3 seperate load controls existed, same amount of increment and decrement controls. To prevent messy wirings all around, I designed this 4bit control signal for each register, be it 8bit or 16bit. In Control Unit of microprocessor, a seperate module is dedicated to combine all register control signals into individual 4bit LIDS control signal.<br>

<h2>Memory</h2>
This microcontroller uses an array of sixteen 12bit address 1Byte data memory modules and a single 16bit address 1bit data memory module. The array is arranged in order to utilize the 16bit Address Bus of microcontroller and it stores instruction as well as data for a program. The 1bit data of single memory module is used as flag for 8bit data from array. The isINSTRUCTION flag is used to distinguish between instruction and data for each byte stored in the array.<br>

<h2>Control Unit</h2>
The Control Unit follows the basic Fetch and Decode method to get instruction (and data) from memory and execute them. However, due to different control signal requirement and different data storing technique leading to varying instruction length. The Control Unit required an indigenous approach for Fetch and Decode. I implemented a 3-stage shift register to aggregate next 3bytes from memory and pass them to Control Unit, depending on the OPCODE and isINSTRUCTION flag, Control Unit can execute 8bit(OPCODE) instruction, 16bit(OPCODE + 8bit data) instruction, or 24bit(OPCODE + 16bit data) instruction.<br>

<h2>Other Features</h2>
<h2>ALU</h2>
The ALU of this microprocessor has seperate blocks for Addition, Addition with Carry, Subtraction, Subtraction with Borrow, AND operation, XOR operation, and OR operation. These operations can be performed on any 8bit operand supported by microprocessor. The ALU also has a wide range of Status Flags, viz. CarryOUT, Zero, isFF, A Greater, A Equal, A Smaller, isODD, and isNEGATIVE.<br>

<h2>Operands</h2>
Operands used in this microprocessor are:<br>

|8bit Operands|16bit Operands|
|:----------:|:-----------:|
|8bit Immediate|16bit Immediate|
|Register A    |PSW (Program Status Word) {Reg Pair A and Flags from ALU}|
|Register B    |Register Pair BC|
|Register C    |Register Pair DE|
|Register D    |SP (Stack Pointer)|
|Register E    |PC (Program Counter)|
|Memory        |                |


<h2>Instruction Set</h2>
Below is the Planned Instruction set for this microprocessor<br>

|UpperHex|LowerHex|Mnemonics|Description|UpperHex|LowerHex|Mnemonics|Description|UpperHex|LowerHex|Mnemonics|Description|UpperHex|LowerHex|Mnemonics|Description|UpperHex|LowerHex|Mnemonics|Description|UpperHex|LowerHex|Mnemonics|Description|UpperHex|LowerHex|Mnemonics|Description|UpperHex|LowerHex|Mnemonics|
|:---:|:---:|:-------------:|---|:---:|:---:|:-------:|---|:---:|:---:|:------------:|---|:---:|:---:|:------------:|---|:---:|:---:|:------------:|---|:---:|:---:|:------------:|---|:---:|:---:|:----------:|---|:---:|:---:|:-------:|
| 0 | 0 | NOP         |   | 0 | 8 |       |   | 1 | 0 |            |   | 1 | 8 |            |   | 2 | 0 |            |   | 2 | 8 |            |   | 3 | 0 |          |   | 3 | 8 |       |
| 0 | 1 | JMP         |   | 0 | 9 |       |   | 1 | 1 |            |   | 1 | 9 |            |   | 2 | 1 |            |   | 2 | 9 |            |   | 3 | 1 |          |   | 3 | 9 |       |
| 0 | 2 | CALL        |   | 0 | A |       |   | 1 | 2 |            |   | 1 | A |            |   | 2 | 2 |            |   | 2 | A |            |   | 3 | 2 |          |   | 3 | A |       |
| 0 | 3 | RET         |   | 0 | B |       |   | 1 | 3 |            |   | 1 | B |            |   | 2 | 3 |            |   | 2 | B |            |   | 3 | 3 |          |   | 3 | B |       |
| 0 | 4 | DI          |   | 0 | C |       |   | 1 | 4 |            |   | 1 | C |            |   | 2 | 4 |            |   | 2 | C |            |   | 3 | 4 |          |   | 3 | C |       |
| 0 | 5 | EI          |   | 0 | D |       |   | 1 | 5 |            |   | 1 | D |            |   | 2 | 5 |            |   | 2 | D |            |   | 3 | 5 |          |   | 3 | D |       |
| 0 | 6 | OUT         |   | 0 | E |       |   | 1 | 6 |            |   | 1 | E |            |   | 2 | 6 |            |   | 2 | E |            |   | 3 | 6 |          |   | 3 | E |       |
| 0 | 7 | IN          |   | 0 | F |       |   | 1 | 7 |            |   | 1 | F |            |   | 2 | 7 |            |   | 2 | F |            |   | 3 | 7 |          |   | 3 | F |       |
|   |   |             |   |   |   |       |   |   |   |            |   |   |   |            |   |   |   |            |   |   |   |            |   |   |   |          |   |   |   |       |
| 4 | 0 |  LXI PSW    |   | 4 | 8 |       |   | 5 | 0 |  LXI BC    |   | 5 | 8 |  LXI DE    |   | 6 | 0 |  LXI SP    |   | 6 | 8 |  LXI PC    |   | 7 | 0 |          |   | 7 | 8 |       |
| 4 | 1 | LDAX A, PSW |   | 4 | 9 |       |   | 5 | 1 | LDAX A, BC |   | 5 | 9 | LDAX A, DE |   | 6 | 1 | LDAX A, SP |   | 6 | 9 | LDAX A, PC |   | 7 | 1 |          |   | 7 | 9 |       |
| 4 | 2 | STAX A, PSW |   | 4 | A |       |   | 5 | 2 | STAX A, BC |   | 5 | A | STAX A, DE |   | 6 | 2 | STAX A, SP |   | 6 | A | STAX A, PC |   | 7 | 2 |          |   | 7 | A |       |
| 4 | 3 | INX PSW     |   | 4 | B |       |   | 5 | 3 | INX BC     |   | 5 | B | INX DE     |   | 6 | 3 | INX SP     |   | 6 | B | INX PC     |   | 7 | 3 |          |   | 7 | B |       |
| 4 | 4 | DCX PSW     |   | 4 | C |       |   | 5 | 4 | DCX BC     |   | 5 | C | DCX DE     |   | 6 | 4 | DCX SP     |   | 6 | C | DCX PC     |   | 7 | 4 |          |   | 7 | C |       |
| 4 | 5 |             |   | 4 | D |       |   | 5 | 5 |            |   | 5 | D |            |   | 6 | 5 |            |   | 6 | D |            |   | 7 | 5 |          |   | 7 | D |       |
| 4 | 6 | POP PSW     |   | 4 | E |       |   | 5 | 6 | POP BC     |   | 5 | E | POP DE     |   | 6 | 6 | LDAX A, 16 |   | 6 | E | POP PC     |   | 7 | 6 |          |   | 7 | E |       |
| 4 | 7 | PUSH PSW    |   | 4 | F |       |   | 5 | 7 | PUSH BC    |   | 5 | F | PUSH DE    |   | 6 | 7 | STAX A, 16 |   | 6 | F | PUSH PC    |   | 7 | 7 |          |   | 7 | F |       |
|   |   |             |   |   |   |       |   |   |   |            |   |   |   |            |   |   |   |            |   |   |   |            |   |   |   |          |   |   |   |       |
| 8 | 0 |             |   | 8 | 8 |       |   | 9 | 0 |            |   | 9 | 8 |            |   | A | 0 |            |   | A | 8 |            |   | B | 0 |          |   | B | 8 |       |
| 8 | 1 |             |   | 8 | 9 |       |   | 9 | 1 |            |   | 9 | 9 |            |   | A | 1 |            |   | A | 9 |            |   | B | 1 |          |   | B | 9 |       |
| 8 | 2 | CMP B       |   | 8 | A | ADD B |   | 9 | 2 | ADC B      |   | 9 | A | SUB B      |   | A | 2 | SBB B      |   | A | A | ANA B      |   | B | 2 | XRA B    |   | B | A | ORA B |
| 8 | 3 | CMP C       |   | 8 | B | ADD C |   | 9 | 3 | ADC C      |   | 9 | B | SUB C      |   | A | 3 | SBB C      |   | A | B | ANA C      |   | B | 3 | XRA C    |   | B | B | ORA C |
| 8 | 4 | CMP D       |   | 8 | C | ADD D |   | 9 | 4 | ADC D      |   | 9 | C | SUB D      |   | A | 4 | SBB D      |   | A | C | ANA D      |   | B | 4 | XRA D    |   | B | C | ORA D |
| 8 | 5 | CMP E       |   | 8 | D | ADD E |   | 9 | 5 | ADC E      |   | 9 | D | SUB E      |   | A | 5 | SBB E      |   | A | D | ANA E      |   | B | 5 | XRA E    |   | B | D | ORA E |
| 8 | 6 | CMP M       |   | 8 | E | ADD M |   | 9 | 6 | ADC M      |   | 9 | E | SUB M      |   | A | 6 | SBB M      |   | A | E | ANA M      |   | B | 6 | XRA M    |   | B | E | ORA M |
| 8 | 7 | CPI 8       |   | 8 | F | ADI 8 |   | 9 | 7 | ACI 8      |   | 9 | F | SUI 8      |   | A | 7 | SBI 8      |   | A | F | ANI 8      |   | B | 7 | XRI 8    |   | B | F | ORI 8 |
|   |   |             |   |   |   |       |   |   |   |            |   |   |   |            |   |   |   |            |   |   |   |            |   |   |   |          |   |   |   |       |
| C | 0 |             |   | C | 8 |       |   | D | 0 | MOV B, A   |   | D | 8 | MOV C, A   |   | E | 0 | MOV D, A   |   | E | 8 | MOV E, A   |   | F | 0 | MOV M, A |   | F | 8 | INR A |
| C | 1 | MOV A, 8    |   | C | 9 |       |   | D | 1 | MOV B, 8   |   | D | 9 | MOV C, 8   |   | E | 1 | MOV D, 8   |   | E | 9 | MOV E, 8   |   | F | 1 | MOV M, 8 |   | F | 9 |       |
| C | 2 | MOV A, B    |   | C | A |       |   | D | 2 |            |   | D | A | MOV C, B   |   | E | 2 | MOV D, B   |   | E | A | MOV E, B   |   | F | 2 | MOV M, B |   | F | A | INR B |
| C | 3 | MOV A, C    |   | C | B |       |   | D | 3 | MOV B, C   |   | D | B |            |   | E | 3 | MOV D, C   |   | E | B | MOV E, C   |   | F | 3 | MOV M, C |   | F | B | INR C |
| C | 4 | MOV A, D    |   | C | C |       |   | D | 4 | MOV B, D   |   | D | C | MOV C, D   |   | E | 4 |            |   | E | C | MOV E, D   |   | F | 4 | MOV M, D |   | F | C | INR D |
| C | 5 | MOV A, E    |   | C | D |       |   | D | 5 | MOV B, E   |   | D | D | MOV C, E   |   | E | 5 | MOV D, E   |   | E | D |            |   | F | 5 | MOV M, E |   | F | D | INR E |
| C | 6 | MOV A, M    |   | C | E |       |   | D | 6 | MOV B, M   |   | D | E | MOV C, M   |   | E | 6 | MOV D, M   |   | E | E | MOV E, M   |   | F | 6 |          |   | F | E | INR M |
| C | 7 | DCR A       |   | C | F |       |   | D | 7 | DCR B      |   | D | F | DCR C      |   | E | 7 | DCR D      |   | E | F | DCR E      |   | F | 7 | DCR M    |   | F | F | HLT   |
