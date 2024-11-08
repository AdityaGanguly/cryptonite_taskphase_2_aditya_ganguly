# Vault Door-3
In this challenge a java program was given to us so i analysed the program to figure out what the password might be.

From the source code we can interpret that if if the password i entered matches the string "jU5t_a_sna_3lpm18g947_u_4_m9r54f" after the loops i will get access granted as output. So i went in reverse direction and used the string to get the derised input.

(actually i solved the logic of the challenge on pen&paper but i have explained below what i did).

Firstly i wrote the entire string "jU5t_a_sna_3lpm18g947_u_4_m9r54f" and wrote the indexes below each element from 0 to 31.

The if condition tells us that the password should be of 32 characters.
```
if (password.length() != 32) {
            return false;
        }
```
The first for loop tells us that the characters from index 0 to 7 will be unchanged, so i left them as it is : "jU5t_a_"
```
for (i=0; i<8; i++) {
            buffer[i] = password.charAt(i);
        }
```
the second for loop tells us that the characters from index 8 to 15 will be in reverse order so took the element at index 15 and exchanged it with 8 and 14 with 9 and so on."jU5t_a_s1mpl3_an"
```
for (; i<16; i++) {
            buffer[i] = password.charAt(23-i);
        }
```
The last and the second last for loops tell us that from index 16  to 31 the characters at even indexs will be exchanged(as per the loop condition) and odd indexes will remain as it is. So i exchanged the element at index 16 with 46-16 = 30, and 18 with 46-18=28 and so on while keeping the elements at odd indexes 17,19,... as it is: "jU5t_a_s1mpl3_an4gr4m_4_u_79958f"
```
for (; i<32; i+=2) {
            buffer[i] = password.charAt(46-i);
        }
for (i=31; i>=17; i-=2) {
            buffer[i] = password.charAt(i);
        }
```
After all the processes are done i fot the string "jU5t_a_s1mpl3_an4gr4m_4_u_79958f".
so i ran the program using this and got access granted as output.
```bluej
Enter vault password: picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_79958f}
Access granted.
```
### Flag:
>picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_79958f}
