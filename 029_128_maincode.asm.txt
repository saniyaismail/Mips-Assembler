move $t0, $t3           #copy the contents of $t3 to $t0
move $s7, $zero         #initialise $s7 to 0, $s7 is the loop counter of outer loop (i = 0)
addi $s6, $zero, 1
sub $s6, $t1, $s6       #set $s6 with  N - 1, where $t1 has the value N(no. of integers to sort)

#Copy elements from input to output
loopi:  beq $s7, $t1, loopiend  #if i == N, exit the loop
        lw $s5, 0($t2)   #load the value from input address into $s5
        sw $s5, 0($t3)   #store the loaded value into output address
        addi $t2, $t2, 4 #increment input address by 4(to get next integer)
        addi $t3, $t3, 4 #increment output address by 4(to store next integer)
        addi $s7, $s7, 1 #increment loop counter(i++)
        j loopi

#end of loop
loopiend:       move $t3, $t0   #output address is reset to initial value       
                move $s7, $zero #reset loop counter to 0(i = 0)

#Selection sort algorithm
loop2:  beq $s7, $s6, loop2end  #if i == N -1, exit the loop
        move $t4, $s7           #$t4 is initialised to $s7 value (max_index = i)
        addi $t5, $s7, 1        #loop counter for inner loop (j = i + 1)


loop3:  beq $t5, $t1, loop3end  #if j == N, exit the inner loop
        #loading of arr[max_ind] into $s0
        sll $t8, $t4, 2
        add $t8, $t3, $t8
        lw $s0, 0($t8)
        #loading of arr[j] into $s1
        sll $t9, $t5, 2
        add $t9, $t9,  $t3
        lw $s1, 0($t9)
        bgt $s0, $s1, L         #jump to L if $s0 > $s1 (arr[max_ind] > arr[j])
        move $t4, $t5           #else, update the variable max_ind with j


L:      addi $t5, $t5, 1        #increment the inner loop counter by 1
        j loop3


#at the end of inner loop, swap the elements arr[max_ind] and arr[i]   
loop3end:       sll $t7, $t4, 2
                add $t7, $t3, $t7
                sll $t6, $s7, 2
                add $t6, $t6, $t3
                #swap
                lw $s2, 0($t7)    #load arr[max_ind] from memeory into $s2 
                lw $s3, 0($t6)    #load arr[i] from memory into $s3
                sw $s2, 0($t6)    #store arr[max_ind] in arr[i]'s loaction
                sw $s3, 0($t7)    #store arr[i] in arr[max_ind]'s loaction
                addi $s7, $s7, 1  #increment the outer loop counter(i++)
                j loop2           #jump to outer loop

#end of outer loop      
loop2end:       move $t3, $t0     #reset the output address to initial value(as stored in $t0)




