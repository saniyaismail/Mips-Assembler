#run in linux terminal by java -jar Mars4_5.jar nc filename.asm(take inputs from console)

#system calls by MARS simulator:
#http://courses.missouristate.edu/kenvollmar/mars/help/syscallhelp.html
.data
        next_line: .asciiz "\n"
        inp_statement: .asciiz "Enter No. of integers to be taken as input: "
        inp_int_statement: .asciiz "Enter starting address of inputs(in decimal format): "
        out_int_statement: .asciiz "Enter starting address of outputs (in decimal format): "
        enter_int: .asciiz "Enter the integer: "
.text
#input: N= how many numbers to sort should be entered from terminal. 
#It is stored in $t1
jal print_inp_statement
jal input_int
move $t1,$t4

#input: X=The Starting address of input numbers (each 32bits) should be entered from
# terminal in decimal format. It is stored in $t2
jal print_inp_int_statement
jal input_int
move $t2,$t4

#input:Y= The Starting address of output numbers(each 32bits) should be entered
# from terminal in decimal. It is stored in $t3
jal print_out_int_statement
jal input_int
move $t3,$t4

#input: The numbers to be sorted are now entered from terminal.
# They are stored in memory array whose starting address is given by $t2
move $t8,$t2
move $s7,$zero  #i = 0
loop1:  beq $s7,$t1,loop1end
        jal print_enter_int
        jal input_int
        sw $t4,0($t2)
        addi $t2,$t2,4
        addi $s7,$s7,1
        j loop1
loop1end: move $t2,$t8
#############################################################
#Do not change any code above this line
#Occupied registers $t1,$t2,$t3. Don't use them in your sort function.
#############################################################
move $t0, $t3
move $s7, $zero  #i = 0
addi $s6, $zero, 1
sub $s6, $t1, $s6 

loopi: 	beq $s7, $t1, loopiend
	lw $s5, 0($t2)
	sw $s5, 0($t3)
	addi $t2, $t2, 4
	addi $t3, $t3, 4
	addi $s7, $s7, 1
	j loopi


loopiend: 	move $t3, $t0	
		move $s7, $zero


loop2:	beq $s7, $s6, loop2end
	move $t4, $s7
	addi $t5, $s7, 1
	
	
loop3:	beq $t5, $t1, loop3end
	sll $t8, $t4, 2
	add $t8, $t3, $t8 
	lw $s0, 0($t8)
	sll $t9, $t5, 2
	add $t9, $t9,  $t3
	lw $s1, 0($t9)
	bgt $s0, $s1, L
	move $t4, $t5

				
L: 	addi $t5, $t5, 1
	j loop3
   
   
   
loop3end: 	sll $t7, $t4, 2
		add $t7, $t3, $t7
		sll $t6, $s7, 2
		add $t6, $t6, $t3
		lw $s2, 0($t7)
		lw $s3, 0($t6)
		sw $s2, 0($t6)
		sw $s3, 0($t7)
		addi $s7, $s7, 1
		j loop2

   	
loop2end: 	move $t3, $t0



#############################################################
#You need not change any code below this line

#print sorted numbers
move $s7,$zero  #i = 0
loop: beq $s7,$t1,end
      lw $t4,0($t3)
      jal print_int
      jal print_line
      addi $t3,$t3,4
      addi $s7,$s7,1
      j loop
#end
end:  li $v0,10
      syscall
#input from command line(takes input and stores it in $t6)
input_int: li $v0,5
           syscall
           move $t4,$v0
           jr $ra
#print integer(prints the value of $t6 )
print_int: li $v0,1
           move $a0,$t4
           syscall
           jr $ra
#print nextline
print_line:li $v0,4
           la $a0,next_line
           syscall
           jr $ra

#print number of inputs statement
print_inp_statement: li $v0,4
                la $a0,inp_statement
                syscall
                jr $ra
#print input address statement
print_inp_int_statement: li $v0,4
                la $a0,inp_int_statement
                syscall
                jr $ra
#print output address statement
print_out_int_statement: li $v0,4
                la $a0,out_int_statement
                syscall
                jr $ra
#print enter integer statement
print_enter_int: li $v0,4
                la $a0,enter_int
                syscall
                jr $ra
                       
