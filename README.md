#Assembly Basics

Assembly program code is written in various segments

		1. .data   ---> All Initialized Data
		2. .bss	   ---> All uninitialized Data
		3. .text   ---> Program Instructions
			3.1  .globl _start  ---> Externally Callable Routines
			3.2  _start:        ---> Main() routine 

# System calls are the library with the kernel exposes to the User space for getting various task done
	List of system calls can be checked under /usr/include/asm_generic/unistd.h
	System calls are invoked by processes using a software interrupt - INT 0x80

#Whenever system calls are invoked, we need to load some registers with the appropriate arguments which the system call may require

		1. EAX : System Call number
		2. EBX : First Argument
		3. ECX : Second Argument
		4. EDX : Third Argument
		5. ESI : Fourth Argument
		6. EDI : fifth argument

# 
		1. movl %eax, %ebx (moves a 32 bit value)
			move eax value into ebx however eax itself doesn't change

		2. movw %ax, %bx (moves a 16 bit value)

		3. movb %ah, %bh (moves a 8 bit value)


# Moving Data
> Between Registers

		movl %eax, %ebx

> Between Registers and Memory

		location:
				.int 10
		movl %eax, location
		movl location, %ebx

> Moving Immediate value in register
		movl $10, %ebx

> Immediate value into memory location

		location:
				.byte 0

		movb $10, location

> Moving data into an indexed memory location
		
		IntegerArray:
			.int 10, 20, 30, 40, 50

		movl %eax, IntegerArray(0, 2, 4)

    Above we are selecting the 3rd integer "30"
	BaseAddress(Offset, Index, Size) = IntegerArray(0, 2, 4)



# Calling exit(0) to exit program

		> Function definition
			void _exit(int status)

		1. Sys call number for exit() is 1, so load EAX with 1

			movl $1, %eax
					number is defined by $1			
	
		2. "status" is lets say 0, EBX must be loaded with 0
			movl $0, %ebx

		3. Calling the System call by Raising the Software interrupt

			int $0x80 
#gdb commands

		1. list <line Number> --> Lists the code from the given line number
			list 1

		2. disassamble <function Name> ---> Shows the assembly code of the function

		3. break <line Number>  ---> Setting the break point 

		4. info registers ---> For looking at the registers

		5. Stack can be viewed in gdb using examine command
			x/10xb <memory address>   ---> Starting with the memory address, 10 bytes are showed in hex format.
			x/10xb 0xbffff5f0

			
			10 - units
			x - hex form
			b - bytes

			x/10xw 0xbffff5f0 ---> 10 words in hex format
					
		6. info variables

		7. 

# Hello world in assembly

# My First Assembly Program
.data
	
HelloWorldString:
	.ascii "Hello World\n"

.text

.globl _start

_start:
	# Load all the arguments for write() function call

	movl $4, %eax
	movl $1, %ebx
	movl $HelloWorldString, %ecx
	movl $12, %edx
	int $0x80

	# Need to exit the program

	movl $1, %eax
	movl $0, %ebx
	int $0x80
 
