# GDB Baby Step 1
For this challenge we need to use gdb so i downloaded it in linus terminal using the following command:
```
aditya_cryptonite@DESKTOP-SG1LV2M:~$ sudo apt-get install gdb
```
After this i opened gdb by typing 'gdb'

Since i didnt know much abould gdb i used the help command to know more abould what commands i should use for this challenge
```
(gdb) help
Output:
List of classes of commands:

aliases -- User-defined aliases of other commands.
breakpoints -- Making program stop at certain points.
data -- Examining data.
files -- Specifying and examining files.
internals -- Maintenance commands.
obscure -- Obscure features.
running -- Running the program.
stack -- Examining the stack.
status -- Status inquiries.
support -- Support facilities.
text-user-interface -- TUI is the GDB text based interface.
tracepoints -- Tracing of program execution without stopping the program.
user-defined -- User-defined commands.
```
I entered help files to know more file commands.

From there i saw that i can debug a file using the file command and then specify the path of the file as arguments. 

Since the file is located in c drive i cant specify its file path directly and have to use mounting by using /mnt/c...
So i tried that:
```
(gdb) file /mnt/c/Users/Admin/Downloads/debugger0_a
Output:
Reading symbols from /mnt/c/Users/Admin/Downloads/debugger0_a...
(No debugging symbols found in /mnt/c/Users/Admin/Downloads/debugger0_a)
```
Now this didnt do anything of use so i saw the hints of the challenge from where i found out i had to disassemble the main function.
I found out abould the disassemble command from the help page by using the help data and disassembled main
```
(gdb) disassemble main
Output:
Dump of assembler code for function main:
   0x0000555555555129 <+0>:     endbr64
   0x000055555555512d <+4>:     push   %rbp
   0x000055555555512e <+5>:     mov    %rsp,%rbp
   0x0000555555555131 <+8>:     mov    %edi,-0x4(%rbp)
   0x0000555555555134 <+11>:    mov    %rsi,-0x10(%rbp)
   0x0000555555555138 <+15>:    mov    $0x86342,%eax
   0x000055555555513d <+20>:    pop    %rbp
   0x000055555555513e <+21>:    ret
End of assembler dump.
```
Now we know that the eas register is 0x86342, and converting it do decimal we get : 549698.

### Flag:
>picoCTF{549698}
