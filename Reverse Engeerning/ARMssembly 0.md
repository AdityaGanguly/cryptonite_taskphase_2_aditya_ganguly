In this challenge we were given with a program in assembly language:
```
	.arch armv8-a
	.file	"chall_1.c"
	.text
	.align	2
	.global	func
	.type	func, %function
func:
	sub	sp, sp, #32
	str	w0, [sp, 12]
	mov	w0, 87
	str	w0, [sp, 16]
	mov	w0, 3
	str	w0, [sp, 20]
	mov	w0, 3
	str	w0, [sp, 24]
	ldr	w0, [sp, 20]
	ldr	w1, [sp, 16]
	lsl	w0, w1, w0
	str	w0, [sp, 28]
	ldr	w1, [sp, 28]
	ldr	w0, [sp, 24]
	sdiv	w0, w1, w0
	str	w0, [sp, 28]
	ldr	w1, [sp, 28]
	ldr	w0, [sp, 12]
	sub	w0, w1, w0
	str	w0, [sp, 28]
	ldr	w0, [sp, 28]
	add	sp, sp, 32
	ret
	.size	func, .-func
	.section	.rodata
	.align	3
.LC0:
	.string	"You win!"
	.align	3
.LC1:
	.string	"You Lose :("
	.text
	.align	2
	.global	main
	.type	main, %function
main:
	stp	x29, x30, [sp, -48]!
	add	x29, sp, 0
	str	w0, [x29, 28]
	str	x1, [x29, 16]
	ldr	x0, [x29, 16]
	add	x0, x0, 8
	ldr	x0, [x0]
	bl	atoi
	str	w0, [x29, 44]
	ldr	w0, [x29, 44]
	bl	func
	cmp	w0, 0
	bne	.L4
	adrp	x0, .LC0
	add	x0, x0, :lo12:.LC0
	bl	puts
	b	.L6
.L4:
	adrp	x0, .LC1
	add	x0, x0, :lo12:.LC1
	bl	puts
.L6:
	nop
	ldp	x29, x30, [sp], 48
	ret
	.size	main, .-main
	.ident	"GCC: (Ubuntu/Linaro 7.5.0-3ubuntu1~18.04) 7.5.0"
	.section	.note.GNU-stack,"",@progbits
```
Lets break this program into parts and try to understand what it means.

1st part will be the function:
```
func1:
    sub sp, sp, #16          ; this line allocate 16 bytes on the stack
    str w0, [sp, 12]         ; this line stores the first argument (w0) at offset 12
    str w1, [sp, 8]          ; stores the second argument (w1) at offset 8
    ldr w1, [sp, 12]         ; load the first argument into w1
    ldr w0, [sp, 8]          ; load the second argument into w0
    cmp w1, w0               ; compare w1 and w0
    bls .L2                  ; branch to .L2 if w1 <= w0
    ldr w0, [sp, 12]         ; if w1 > w0, load w1 into w0
    b .L3                    ; jump to .L3
.L2:
    ldr w0, [sp, 8]          ; if w1 <= w0, load w0 into w0
.L3:
    add sp, sp, 16           ; restore the stack pointer
    ret                      ; return
```
So basicallly this function takes 2 numbers as arguments and calculates which one is larger and returns it, just like a C program does.

2nd part is the main function:
```
main:
    stp x29, x30, [sp, -48]! ; Push the frame pointer (x29) and return address (x30) onto the stack, adjust stack by -48
    add x29, sp, 0           ; Set the frame pointer to the current stack pointer
    str x19, [sp, 16]        ; Save x19 (caller-saved register) at offset 16
    str w0, [x29, 44]        ; Store argc (number of arguments) at offset 44
    str x1, [x29, 32]        ; Store argv (pointer to arguments) at offset 32
    ldr x0, [x29, 32]        ; Load argv into x0
    add x0, x0, 8            ; Point to argv[1]
    ldr x0, [x0]             ; Dereference argv[1] to get the first argument
    bl atoi                  ; Convert argv[1] to an integer, result in w0
    mov w19, w0              ; Save the result (first integer) in w19
    ldr x0, [x29, 32]        ; Load argv into x0 again
    add x0, x0, 16           ; Point to argv[2]
    ldr x0, [x0]             ; Dereference argv[2] to get the second argument
    bl atoi                  ; Convert argv[2] to an integer, result in w0
    mov w1, w0               ; Move the second integer to w1
    mov w0, w19              ; Move the first integer back to w0
    bl func1                 ; Call func1 with the two integers
    mov w1, w0               ; Move the result from w0 to w1 for printf
    adrp x0, .LC0            ; Load the address of the format string into x0
    add x0, x0, :lo12:.LC0   ; Adjust the address to the exact location
    bl printf                ; Call printf to print the result
    mov w0, 0                ; Set the return value of main to 0
    ldr x19, [sp, 16]        ; Restore x19
    ldp x29, x30, [sp], 48   ; Restore x29 and x30, adjust stack pointer
    ret                      ; Return from main

```
This function takes two command-line arguments argv[1] and argv[2] and converts them to integers and passes them to the func1 function which computes the largest number amoung them and returns them. 
It then prints the result returned by func1.


So now we know what the program does so now we need to get the output for the inputs : 266134863 and 1592237099

The program computes the larger of 2 numbers so the output will be :1592237099 .
Converting this to hex we will get :5EE79C2B

So now we will write this in the flag format provided : 5ee79c2b
 ### Flag:
 >picoCTF{5ee79c2b}


