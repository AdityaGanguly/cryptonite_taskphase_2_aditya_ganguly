# Vault Door-3
In this challenge a java program was given to us so i analysed the program to figure out what the password might be.

From the source code we can interpret that if if the password i entered matches the string "jU5t_a_sna_3lpm18g947_u_4_m9r54f" after the loops i will get access granted as output. So i went in reverse direction and used the string to get the derised input.

The if condition tells us that the password should be of 32 characters.
```
if (password.length() != 32) {
            return false;
        }
```
The first for loop tells us that the characters from index 0 to 7 will be unchanged.
```
for (i=0; i<8; i++) {
            buffer[i] = password.charAt(i);
        }
```
the second for loop tells us that the characters from index 8 to 15 will be in reverse order.
```
for (; i<16; i++) {
            buffer[i] = password.charAt(23-i);
        }
```
The last and the second last for loops tell us that from index 16  to 31 the characters at even indexs will be exchanged(as per the loop condition) and odd indexes will remain as it is.
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
thr flag is :picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_79958f}
