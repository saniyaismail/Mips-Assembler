# Mips-Assembler


● 029_128_full_template_code.asm → contains the template
code given along with the sorting algorithm that we have written.

● 029_128_assembler.py → contains the python code that
works as an assembler to convert the mips assembly contents of
the file 029_128_full_template_code.asm into machine code and
output into a 029_128_machine_code.txt file.

● 029_128_maincode.asm → contains the sorting
algorithm(selection sort) that we have written in mips along with
comments to explain the code lines.

● 029_128_machine_code.txt → the output of the assembler i.e
the machine code is written into this file.


1. Assembly Programming:

● The program that we chose for this question is ‘Sorting
algorithm’.

● We have written the mips assembly code for ‘Selection Sort
algorithm’ to sort the given input array of integers in descending
order.Brief Explanation of the code:

● To perform in-place sorting, we have copied the input array of
integers to the output address and then performed the sorting at
the output array’s address location.

● The ‘loopi’ in the code does this task of loading of integers from
input array to output array.

● After this loop, the output address is reset.

● The loop2 is the outer loop that iterates through the array.

● loop3 is the inner loop that finds the maximum element’s index
from the unsorted part.

● The inner loop runs for N times, where N is the number of
integers to sort.

● At the end of the inner loop i.e loop3, the elements at maximum
index and outer loop’s loop counter are swapped.

● The process is repeated until the array is sorted.

● In each iteration of the outer loop, the maximum element from
the unsorted part is found and is swapped with the first element
of that unsorted part.

● This Selection sort algorithm that we chose to do the assembly
programming stores the sorted integers in descending order at
the output address range.

● This function reads mips assembly code from a file line by line,
and ignores the lines which are comments and part of the .data
segment.

● It processes each instruction from the mips code and modifies
them and stores them in a list called lines_list.

● The pseudo instructions like ‘move’, ‘bge’, ‘la’, ‘li’ which don't
have a direct one-to-one correspondence with actual machine
instructions are replaced by one or more instructions.

● For the jump instructions the label mappings are used to replace
the labels with addresses.

We have put label-address mappings using the labels table from Mars
as reference.
jal_map = {'print_int': 4194592, 'input_int': 4194576, 'print_line':
4194608, 'print_inp_int_statement': 4194648,
'print_out_int_statement': 4194668, 'print_inp_statement':
4194628, 'print_enter_int': 4194688}Also there are mappings op_code, function_code and register.These
dictionaries store mappings for registers, opcodes, and function codes
used in MIPS instructions.

We wrote a function (assembly_machine) that takes in the assembly
instructions(in string format) and converts it into machine code(in
string format).

What the function does:

● If the instruction is syscall it directly returns the machine code
that is 0x0000000c.

● With the help of the split() function it separates the op_code,
registers, target address, and immediate value.

● The op_code is stored in a variable op.

● If the op is add or sub or addu or slt, that means it is a r type
instruction( format example [add $s1 $s2 $s3] )with 3 registers in
which the first two registers contain data to be worked on and the
third being the destination register and shift_amt is zero. The
machine code for this instruction is op code(6 bit we get from
op_code map) + 3 registers(rs rt rd 5 bits each we get from
register map) + shamt(5 bit) + fun_code(6 bit we get from the
fun_code map)

● If the op is sll, that means it is a r type instruction with 2
registers( rt which contains data to be worked on and rd the
destination register )and shift_amt not equal to zero (format
example [ sll $s1 $s2 2]) The shift_amt is converted to 5 bit
binary.The machine code for this instruction is op code(6 bit we
get from op_code map) + 3 registers((rs is $0 here) rt rd 5 bits
each we get from register map) + shamt(5 bit) + fun_code(6 bit
we get from the fun_code map)

● If the op is addi or ori or addiu, that means it is an i type
instruction with rt rs and immediate value(format example[ addi
$s1 $s2 1] Here rt is s1 and rs is s2). The immediate value isconverted into a 16 bit binary. The machine code for this
instruction is opcode + rs + rt + immediate

● If the op is bne or beq, that means it is an i type instruction with rt
rs and relative address (format example [bne $s1 $s2 4 ] Here s1
is rt, s2 is rs and 4 is the relative address ). The relative address
is converted into a 16 bit binary. The machine code for this
instruction is opcode + rt + rs + relative address.

● If the op is sw or lw, that means it is an i type instruction of the
format [ sw $s1 8($s2) ] In this example 8 is the offset value, s2
is the base register, and s1 is rt. The offset is converted to 16 bit
binary. The machine code for this instruction is opcode + base
register + rt + offset

● If the op is lui, that means it is a i type instruction of the format
[ lui $s1 immediate value ]. Here $s1 is rt. The immediate value
is converted into a 16 bit binary. The machine code is op_code +
$0 register + rt + immediate.

● If the op is jal or j, that means it is a j type instruction of the
format [ j 48989] Here 48989 is the target address. It is
converted into a 32 bit binary. The last two bits will always be
zero as the address is multiple of 4. The first four bits are always
zero as it is restricted for the text segment. We need to fit the
address in 26 bits. So the first 4 and last 2 bits are removed.The
machine code is op_code + target_address.

● If the op is jr, that means it is a r type instruction of the format
[ jr $ra ] $ra contains the return address of the function. The
Shamt field is zero here. Machine code is op + register ra +
register $0 + register $0 + shamt + function_code
write_to_file(code, output_file):

● This function takes a list of machine code instructions (code) and
writes them to a specified output file i.e a .txt file here.To the .txt we are writing both binary and hexadecimal representations
of the mips instructions as converted by the assembler we created.
The machine code written into the output file is verified with the code
generated by the MARS simulator to verify the correctness of our
assembler.
